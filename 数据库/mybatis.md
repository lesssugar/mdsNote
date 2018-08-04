#ORM

 ORM  object relationship mapping 对象关系映射，是一种数据持久化技术，他在对象模型和关系型数据之间建立起对应关系，并且提供了一种机制，通过javabean对象去操作数据库表中的数据。

使用orm程序员就不需要在使用SWL语句操作数据库中的表，使用API直接操作javaBean对象就可以实现数据的存储，查询，更改和删除等操作。mybatis通过简单的XML或者注解进行配置和原始映射，将实体类和SQL语句之间建立映射关系，是一种半自动化的ORM实现。

基于ORM，mybatis在对象模型和关系数据库的表之间建立了一座桥梁，通过mybatis的关系映射，以便捷的实现数据存储，查询，更改和删除操作。

#使用

将数据库驱动包，mybatis包导入，log4j包导入

创建mybatis核心配置文件 configuration.xml

静态代码块在一个类被加载的时候执行，可以用来初始化类中的数据，避免写类中的属性直接等于多少

##配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 引入datasource.properties文件 -->
	<properties resource="datasource.properties"></properties>
	<!-- 配置mybatis的log为LOG4J -->
	<settings>
		<setting name="logImpl" value="Log4J"/>
	</settings>
	<!-- 配置mybatis多套运行环境 -->
	<environments default="development">
		<environment id="development">
			<!-- 配置事物管理，采用JDBC的事务管理 -->
			<transactionManager type="JDBC"></transactionManager>
			<!-- POOLED:mybaties自带的数据源,JDNI基于tomcat的数据源 -->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}" />
				<property name="url" value="${jdbc.url}" />
                <property name="username" value="${jdbc.username}" />
                <property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>
	</environments>
	<!-- 将mapper文件加入到配置文件中 -->
	<mappers>
		<mapper resource="cn/mybatis/UserMapper.xml "/>
	</mappers>
</configuration>
```

##mapper文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 映射的数据库操作 -->
<mapper namespace="cn/mybatis/UserMapper">	//映射接口所在位置
	<select id="count" resultType="int">
		select count(1) as count from state
	</select>
</mapper>
```

## session工厂

```java
String resource = "mybatis-config.xml";
InputStream is = Resources.getResourceAsStream(resource);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build();	//获取session工厂,我们用的是读取总配置文件的输入流产生SqlSession对象
SqlSession sqlSession = factory.openSession();	//获取session
List<User> users = sqlSession.getMapper(User.class).getList();	//直接映射对应接口，调取方法
sqlSession.close();	//关闭session

sqlSession.commit();//更新操作需要手动进行提交
```

#Param

```java
public void save(@Param("user")User user)
//直接对应sql语句中的一个属性，通过#{user}，#{user.name}调用
```

# 插入后为对象添加主键

```xml
  <insert id="insert" parameterType="cn.crud.bean.Emp" useGenerateKeys="true" keyProperty="id">
    insert into tbl_emp (emp_id, emp_name, gender, 
      email, d_id)
    values (#{empId,jdbcType=INTEGER}, #{empName,jdbcType=VARCHAR}, #{gender,jdbcType=CHAR}, 
      #{email,jdbcType=VARCHAR}, #{dId,jdbcType=INTEGER})
  </insert>
useGenerateKeys,keyProperty两条属性，在添加的对象的主键值为null的情况下附加属性
```

```xml
orcale不支持自增，但支持序列。
<selectKey keyProperty="id" order="before" resultType="Integer">
select 列名.nextval 可以查询序列的下一个取值放在该标签内
keyProperty = "" 要将查出来的值封装给javaBean的那个属性
```

order 在当前SQL语句之前执行查询主键的selectKey标签中的内容,查询出来的结束类型是Integer

keyProperty属性会封装javaBean对象的对应属性

增删改查一定要模仿有可能异常，进行回滚

insert ..........................values(#{列名.nextval},#{lastName},#{email});

这样可以使order改成after，就成了先插入属性，后查询，nextval要改成currval就成了查询现在的序列的值

参数的添加。多个参数会被封装成一个map，可以用param1，param2类似的形式指定添加的参数，不过该种方式不推荐使用。

可以用@param("id")注解明确指定sql语句中添加的是哪一个参数的指定值。推荐。

如有很多个参数正好是业务逻辑的数据模型，可以直接传入pojo对象，也可以传入Map<String , Object> map;

用map的key值对应获取对应的value值

如果参数不是业务模型，且需要传入多个参数，并需要经常使用，推荐编写一个POJO

如果是list，collection，数组类型，封装的map的key值分别是list,collection,array.通过下表[0]，[1]使用

${}的取值方式将取出的值直接放在sql语句中，不是预编译执行，对于原生sql语句不支持预编译的地方如，表名，排序等，要用${}进行参数传递

my batis会将null值默认对应成other，而类似oracle数据库对此类型不识别，所以会报错，my sql可以兼容，需要在SQL语句参数#{}里面写上 ，jdbcType=NULL。

也可以在全局设置的setting标签下

当获取一个pojo类的map集合时，其他不变，如果想让map的key值为pojo类的之间值，在接口方法的上面写注解 MapKey("id"); 意思就是封装这个map的时候用id作为map的key值

自定义resultMap，自定义高级映射

在映射文件里写自定义的resultMap

type = "pojo类的全类名"  使用该map的id叫什么，可以填写在返回值类型当中， id标签指定的是主键列，result指定的是非主键列，column指定的是数据库表的字段属性，property指定的是javaBean的属性

​	<resultMap type="" id="">

​		<id column="" property=""/>

​		<result column="" property=""/>

​	</resultMap>

建议只要写就将全部的属性全部写出，虽然没写的属性会自动进行封装。返回值不是map而是type中填写的pojo类

department 部门

将外键表的javaBean作为一个属性添加到原表中，然后在查询后在自定义resultMap中通过 property=" x.y "填写添加的对象的属性。查询完毕后则会自动进行封装，查询的时候通过原表 get外键表属性 .get属性查询需要的对应值

association 联合

第二种方式 <association property="x" javaType="">x：外键命名的对象名(原表中的属性)，变量名，type="类名"，

​		<id column="" property=""/>

​		<result column="" property=""/>

里面一样写这样的标签，javaBean的属性就可以写联合表中的属性，不会冲突，因为表名了联合的java类

使用association进行分步查询 property还是填写要封装的属性，如外键属性。

<association property="x" select="" column="">

如果需要传入多个参数用column="{a=id,b=name}"封装成map的形式提供多个参数

标签内填写 select填写映射文件路径.方法名 

 标签内填写 column="d_id"，将第一个语句查询出来的d_id给select内的方法作为参数使用，并赋值给 property属性

分步查询，通过调用第一个自己写的方法和各个别的已经写好的方法，逐步传递参数，达到封装各个属性的效果，方法明填写namespace下方法id调取 column填入给与的属性

like里的参数一定要用concat进行连接

可以在别的association中resultMap属性重复调用已经写好的封装规则

​		<association property="" javaType="" resultMap="">			

​		</association>	

collection和assosiation类似，区别在于一个封装复杂属性（对象），一个封装的是集合。 javaType替换成 ofType

动态sql

<if test=""></if>如果用户 userName != null and userName ！= ‘’；则拼装sql语句

<where>元素标签会自动识别其标签内是否含有返回值，如果有就自动添加一个where标签，并且只能的处理 and或者or标签。

如果检索语句内部没有返回值，则不写where，如果有，则去除掉第一个and或者or来使语句可以正常使用，其他与句貌似不使用where

trim标签是where语句的升级版，内部属性 prifix="where" prifixOverrides="and | or" 如果内部语句有返回值，则在前面添加一个where并且去除最前面的and或者or

后面是suffix，suffixOverrides同理

更新操作，最外部set标签

内部<if test="">userCode=#{userCode},

使用set标签可以动态的匹配关键字，如果有返回值则添加set，还可以只能的去除末尾的逗号。

在sql语句中遍历传入的集合参数

<foreach collection="array" item="roleId" open="(" separator="," close=")" index="" >				

</foreach>

collection 遍历的参数类型 list,array,若传入的是多个参数，则需要把他们封装成一个map进行处理。

item 遍历的集合

open 开始以什么符号开始，close对应结束

separator迭代之后添加的符号

延迟加载，当一个类中的关联属性不需要一开始进行查询而希望在使用的时候在进行加载的话，可以打开总配置中的懒加载标签。

联合内部属性fetchType="LAZY"可以设置该项进行懒加载

传入参数是集合，数组等类型时，不可以用@param注解

<foreach collection="list" item="usersId" open="(" separator="," close=")">

\#{usersId}

</foreach>

<choose>

<when test="">

<otherwise>

当参数为map时，collection是键名，items=""集合名

在where写个1=1可以使语句不会出问题，后面的在进行语句的拼接

当一张表的多个字段对应其他表的一个属性时，可以将一个字段的表拆分成多个重复表，与原表进行配对查询

也可以通过遍历循环插入多条数据

mysql支持values语法

<insert id = "addEmps">

insert employee(x,x,x,x,x) values

<foreach cloection="emps" item="emp" separator="," >

(#{emp.x},#{emp.x}，#{emp.x})

也可以遍历多个sql语句，不过要在数据库的配置上修改 allowMultiQueries

允许同时支持多个sql语句 url配置后面写

oracle不支持 values语法

可以循环多条语句，写在一个begin - end 中

sql 标签可以抽取可以重用的sql片段进行后面的引用，写上id属性

在需要的地方用include标签调用

if判断有个默认属性是databaseId = ""用于判断数据库种类

一级缓存，与数据库同一次会话期间查询到的数据会放在本地缓存中，以后需要获取相同的数据，直接从缓存中拿，没必要再去数据库中拿

增删改后会使缓存失效，因为有可能增删改改变了数据

二级缓存由一个namepace所对应

工作机制，一个会话查询一条数据，这个数据就会被放在当前会话的一段缓存中，如果会话关闭，以及缓存中的数据就会被保存到二级缓存中

不同namspace的数据会放在自己对应的缓存中，map中。

使用，开启全局二级缓存配置，默认是打开的，但是只要是我们要开启的信息，都要显示的把他配置出来，去每个mapper.xml配置使用二级缓存

在里面填写一个 <cache>标签就可以开启二级缓存

属性 eviction,缓存的回收策略

只有会话关闭了才可以在二级缓存中查询到。没配置缓存的mapper不会有

每个方法的行内由一个useCache(标签)=“true”  不影响二级缓存

flushCache() 清空缓存，对一级缓存有效。二级缓存都有效，在增删改会执行

查询标签内默认为false

session对象的clearSession() 清空的是一级缓存

更新用户数据时一般不会加入if标签，而是根据用户输入的信息全部更新，即使为null或者空字符串，因为用户可能是删除了某一字段信息想使他为空，我们只需要给用户页面时将数据库中已有的信息发给用户，然后让他自行增删改查。在更新到数据库

配置文件和注解的混合使用，配置bean一般都使用配置文件的方式，注入属性都是用注解的方式进行使用

如果xml文件中包含了特殊符号，可以用两种方式进行处理，

<![CDATA[p&G]]>  >，<,&,',"或者转义字符进行使用

&lt; &gt; &amp; &apos; &quot;





都是写在property中。其中的值通常都是字符串类型

如果是bean对象就换成ref

properties类型



引入其他的bean作为参数时，可以用ref bean标签，也可以使用ref local标签，spring文件可以拆分成多个文件，而local只会检索自身配置文件中的bean id

AutoWired()如果没有匹配对对应的元素则会报异常，如果不是必须的元素，可以在里面写上required = false

Resource如果没写参数，根据字段名作为Bean名，或者根据Setter方法。

使用注解实现AOP,创建类的bean

在spring核心配置文件中，开启aop的操作

配置完后，要在类上面写@Aspect注解

方法上写对应的aop操作注解

多参数的需要写上 pointcut=""; returning="return"

方法参数内的 Object return;

或者 RuntimeExecption throwing

创建一个控方法，上面写注解Pointcut("* execution()")可以保存该切入点，在其他需要调用的地方，用pointcut()方法调用

#MyBatis-Gneratoe-Core

```java
selectByExample 按照条件查询出一个对象
selectByPrimaryKey 根据主键查询
deleteByExample 根据条件删除一个对象
deleteByPrimaryKey 根据主键删除
insert 插入
insertSelective 有选择的进行插入 带什么插入什么。可不不用带id
countByExample 根据条件进行计数
updateByExampleSelective 根据条件进行有选择的更新
updateByExample 进行全字段的更新
updateByPrimaryKeySelective 根据主键进行有选择的更新
updateByPrimaryKey根据主键进行全字段的更新
Base_Column_List 全字段的列表
```

#pageHelper

```xml
<plugins>
	<plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
//4.0后实现类为该类，4.0之前为PageHelper
```

## 基本使用

```java
PageHelper.startPage(page,5);
List<Emplovee> emps = emploveeService.getAll();
PageInfo pageInfo = new PageInfo(emps);	//可对查询进行包装
```

## 方法

```java
pageInfo.getNavigatepageNums();	//获取显示页数
pageInfo.getPageSize();	//获取每页显示数量
getStartRow(); 开始查询的记录
        getEndRow();结束的记录
        getTotal(); 获得总记录数
        getPages();总页码
        getFirstPage();获得第一页
        getLastPage()获得最后一页
        isFirstPage();是否是第一页 用在jstl时把is去掉
        isLastPage()是否是最后一页 用在jstl时把is去掉
        isHasPreviousPage()是否有前一页  用在jstl把is去掉
        isHasNextPage()是否有下一页 用在jstl把is去掉
```

```java
PageInfo pi = (PageInfo) request.getAttribute("pageInfo");
		System.out.println("当前页码" + pi.getPageNum());
		System.out.println("总页码" + pi.getPages());
		System.out.println("总记录数" + pi.getTotal());
		int[] navigatepageNums = pi.getNavigatepageNums();
		for (int i : navigatepageNums) {
			System.out.print(" " + i);
		}
		System.out.println("");
		List<Emplovee> emps = pi.getList();
		for (Emplovee emplovee : emps) {
			System.out.println(emplovee);
		}
```

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/1bd7de99033a4c019f77186188b130d7/clipboard.png)

# Mybatis Generater

- 导入项目
- jar包的位置，修改一处*
- 是否生成注释，修改1处
- 数据库连接修改多处*
- bean的生成位置修改前两处，注意Project写到java就好，其他的由package补全
- mapper的生成前两处，同上
- dao的生成前两处，同上
- 表的生成策略，每张表两处*

