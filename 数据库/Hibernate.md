# HelloWorld

```java
//创建session工厂
Configuration configure = new Configuration().configure();
//4.0工厂之前的创建方式，4.0废弃
//SessionFactory sessionFactory = configure.buildSessionFactory();
//4.0之后注册 配置文件信息
ServiceRegistry serviceRegistryBuilder = new ServiceRegistryBuilder().applySettings(configure.getProperties()).buildServiceRegistry();
//创建工厂
SessionFactory sessionFactory = configure.buildSessionFactory(serviceRegistryBuilder);
//创建session
Session session = sessionFactory.openSession();
//开启事务
Transaction tx = session.beginTransaction();
//执行保存操作
News news = new News(null,"w","wyq",new Date());
session.save(news);
//提交事务
tx.commit();
//关闭session
session.close();
//关闭sessionFactory对象
sessionFactory.close();
```

# 错误集

```xml
<resources>
   <resource>
      <directory>src/main/java</directory>
      <includes>
         <include>**/*.xml</include>
      </includes>
      <filtering>true</filtering>
   </resource>
</resources>	//在java包下的配置文件都不会被编译，所以如果想扫描填上资源的位置
```

# 增删改查

## 查

```java
News o = (News) session.get(News.class, 1);	//根据id获取一个对象
News o = (News) session.load(News.class, 1);

两者的不同,执行get方法会立即加载对象，而load方法，若不使用则不会进行查询，而返回一个代理对象PersistentSet
如果数据库中没有对应的记录，get会返回Null而load会报错
load有可能会出现懒加载异常，在查询之前关闭了session
```

##增

```java
//执行保存操作
News news = new News(null,"恭喜国足夺冠","wyq",new Date());
session.save(news);

//如果对已经有id的对象进行insert，会抛出异常
session.persist(news);
```

## 改

- update();		//若更新一个持久化对象，可能不需要显示的调用该方法，
- 若更新一个持久化操作，不需要显示的调用该方法，因为在调用tx.commit()；的时候会自动更新
- 将事物，session提交关闭了，那么session查出来的对象就不受监管了，即使再次打开事物，对该对象的操作
- 也不会发送update语句，这种时候才需要显示调用session.update()方法;会把一个游离对象变为持久化对象
- 无论要更新的对象和更新表的数据是否一致，都会发送update语句，再有触发器的时候可能会错误的多次触发触发器
- 若表中没有对应的记录，报异常
- 如update()关联一个游离对象时，如果在Session的缓存中已经粗在相同OID的持久化对象，会抛出异常，因为Session缓存中不能有两个相同OID的对象

### 解决

```xml
<class name="com.fhiber.bean.News" table="`news`" dynamic-update="true" select-before-update="true">	//添加一个select-before-update="true",但是每次更新前都会多一条查询语句，通常不设置
```

## saveOrUpdate();

如果一个对象不存在则保存，如果存在则更新

OID不为空且数据表中没有该数据，报错

##merge();

##delete();
可以删除一个持久化对象，也可以删除一个游离对象
只要OID和数据表中的一条数据对应，就删除，若OID在数据表中没有对应的记录，报错，只根据OID进行判断
##evict();
从内存中将制定的对象移除
session.evict(Object obj);	//因为移除了所以提交后也不会发有关于他的操作的sql语句
# 设置文件

```
<!--指定自动生成表的策略-->
<property name="hbm2ddl.auto">update</property>
//共有四个选择，
create	//每次操作创建一张新的表
create-drop	//每次sessionFactory关闭后删除创建的表
update	//如果插入语句和表结构相同则正常插入，否则为表添加列在插入
validate	//如果插入结构和表结构不同，则会抛出异常
```

# 缓存

一级缓存，第一次查询查询后将数据存储到session缓存中，没有发生更新数据，或者清除缓存，下次执行该条sql语句，则会从缓存中查找

##flush()方法

session按照缓存中的数据进行sql语句同步到数据库

在Transaction的commit()方法中，先调用flush()方法，根据情况发送sql语句，然后事务提交完成更新 

当调用hql,criteria操作时，如果缓存中的属性发生了变化，会先执行flush(),以保证查询结果能够最新数据

## refresh()

如果在两次同样的查询之间数据库发生了改变，那么就查询不到

会强制发送select语句，以使Session的缓存中的状态更新到和数据库一样的状态

MySql数据库中暂时无效，是因为MySql默认的事物隔离级别是可重复读，改成读已提交即可

```xml
<!--事物隔离级别-->
<property name="connection.isolation">2</property>
1		2		4		8		对应不同的隔离级别
```

## clear()

清理缓存

# 配置

##映射文件

```xml
<class name="News" table="`news`" dynamic-update="true">	//动态更新，更新只包含我们输入的字段
dynamic-insert="true"		//动态插入
```

## 映射为一个组件(不重要)

```xml
<class name="News" table="news" dynamic-update="true">
    <id name="id" column="id" type="int">
        <!--指定主键的生成方式-->
        <generator class="native"/>
    </id>
    <property name="title" type="string" column="title"/>
    <component name="info" class="Info">
        <property name="msg" column="msg"></property>
    </component>
</class>	//组件映射出来的属性会出现在一个表当中，所以不能有重复字段
```

## 如何映射时间

util.date是java.sql下的Time,Date,Timetamp三个类的父类，所以Date类可以兼容这三个类
我们可以通过proeprty的type属性来进行映射
```xml
<property name="date" type="date" column="date"/>	sql-type="text"可以指定映射成的字段类型
date只显示日期，time只显示时间,timetamp时间戳
java类应该写成java.util.Date类即可 
```
##主键生成策略

- increment	在最大主键id的基础上+1,存在并发问题，仅在测试时可以使用
- identity           对于支持标识列的数据库mySql,sql Server,DB2等，OID为Long，Int都可，有数据库进行增长
- sequence       对于支持序列的数据库，如DB2,Oracle
- Hilo标识符生成器           
- native             由hirbernate根据数据库选择OID生成策略(推荐)

# 映射关系

## 多对一映射及对应增删改查

```
<many-to-one name="news" column="NEWSID" class="com.fhiber.bean.News"></many-to-one>
NEWSID映射为主外键关系，在多的一方设置
        News news = new News();
        Info info1 = new Info("dsasa", news);
        Info info2 = new Info("dsasa", news);
        news.setTitle("dasdasds");
        session.save(info1);
        没有news对象会先insert一条news数据，在进行info1的insert
```

这样的话添加查询的时候都会根据这个进行映射，应该先把少的一方先保存到数据库，如果先保存多的一方，则在一开始没有该对象的时候先插入空的代替，然后在进行更新，多发sql语句浪费效率

```java
Info info = (Info) session.get(Info.class, 1);	//查询
```

懒加载查询，删除不能违背主外键关系

##一对多映射

```mysq
<set name="infos" table="info" inserve="false"  cascade="delete"(不建议使用) 
order-by="name desc">	
     <key column="NEWSID"></key>
     <one-to-many class="Info"/>
</set>
//inserve为false的一方决定为主动维护方，会对另一张表的数据在插入后进行update检查数据是否插入正确，所以一般在，建议在set设置，inverse="false"，并且先插入1的一端，在插入少的一端，如果不添加双方都会发送update语句，进行维护
//cascade="delete"级联。当多的一方引用1的一方为外键时，不能删除，使用级联会一起删除
//order-by	排序，添加数据库的字段名
```

一对多的进行映射，集合的属性只能用set进行装载，而且集合的对象必须全部进行插入后才可以插入一的一方，双方都添加就是双向维护，需要在少的一的一端 inverse="true" ，让一的一端维护双方关系

## cascade

```xml
cascade="delete-orphan"	//删除孤儿，当父类集合.clear()清空后，数据库中的子数据，本应该作为单独个体继续存在，如果设置删除孤儿，在父元素解除与其关系的时候就会将其删除
cascade="save-update"	//级联添加，父元素中的子元素会级联进行添加
cascade属性不建议使用，如果需要其功能，可以手动实现
```

## 一对一映射

###基于外键

```
//info表引用了news表的NEWS_ID为外键且有唯一约束
<many-to-one name="news" class="com.fhiber.bean.News" column="NEWS_ID" unique="true"/>
<one-to-one name="info" class="Info" property-ref="news"/>
//应该先保存没有外键列的一方，可以少一条update语句
```
### 基于主键映射

表示一张表的主键参照另外一张表的生成

```xml
        <id name="id" column="id" type="int">
            <!--指定主键的生成方式-->
            <generator class="foreign">
                <param name="property">info</param>
            </generator>
        </id>
		<one-to-one name="info" class="Info" property-ref="news" constrained="true"/>
//指定第一个中的主键参照他的info属性的主键值。one to one 进行对照
//先插入哪个都可以，insert语句顺序不会有变化，查询被引用的一方会左外链接，因为不知道它对应的哪条数据
```

## 多对多映射

```xml
        <set name="items" table="EMP_ITEMS" >	//对应的类，中间表的名称
            <key column="E_ID"></key>			//当前持久化类在表中的外键
            <many-to-many class="Item" column="I_ID"/>	//对应类及其在中间表的ID
        </set>
//多对多，双方都进行交叉映射，表明相同，应该在一方设置reverse，否则双方都进行插入会造成主键冲突
```

## 继承映射(用的不多)

```xml
//父类中，映射子类
<class name="News" table="news"  discriminator-value="PERSON"> 最后一个属性随便写
<discriminator column="TYPE" type="java.lang.String"></discriminator>//标识者列
<subclass name="Student" discriminator-value="STUDENT">
      <property name="school" type="string" column="SCHOOL"></property>
</subclass>
插入的一个PERSON类时，TYPE字段就是PERSON，插入式Student时，TYPE就是Student
该种方式无论插入还是查询使用的都是同一张表，缺点多了一列，如果继承较多，表字段会多
```

```xml
//父类中  
<joined-subclass name="Student" table="Students">
    <key column="Student_ID"></key>
</joined-subclass>
//子表只保存自己的字段和父表主键的外键，插入，查询子类记录至少需要操作两张表，性能稍差，但没有冗余字段
```

```xml
//父类中
<union-subclass name="Student" table="Students">
    <property name="school" column="school" type="string" ></property>
</union-subclass>
//主键生成策略不能为indentity,或者native选择为indentity,不推荐，更新要更新多张表，冗余字段多
```

# 检索策略

```xml
<class name="News" table="news"  lazy="true">
//如果改为false，则关闭懒加载，懒加载仅对load()方法有效，get()及Query的list()方法使用的总是立即检索策略
```

```java
//set的三个属性
lazy	true	false	extra	增强，增强的延迟检索，尽可能延迟集合初始化的时机，在我们调用集合.size().contains().isEmpty()时，会尽可能用简单的sql语句进行代替效果。大部分时候取默认值
batch-size	//一次对集合初始化5个，在我们遍历延迟加载的集合时，可以提高效率
fetch 	select		//正常方式初始化集合
	    subselect	//查询1的一端通过where ID in的方式，初始化所有的集合元素，batch-size失效
	    join	//lazy失效，在加载1的一端，使用迫切左外连接(使用左外连接，且把集合初始化)的方式检索n的一端，HQL查询忽略该属性
```

```java
//many to one 
lazy proxy false	//延迟检索和立即检索
fetch join
batch-size	写在1的一端的class上
```

# HQL

```java
        String hql = "FROM Info WHERE id = ? AND id LIKE ?";	//from写的类名
        Query query = session.createQuery(hql);
        query.setInteger(0,1);
        query.setString(1,"%1%");
        List list = query.list();
        System.out.println("list = " + list);
```

## 或基于命名入参

        String hql = "FROM Info WHERE id = :id AND id LIKE :idLike ORDER BY id" ;	//from写的类名
        Query query = session.createQuery(hql);
        query.setInteger(id,1).setString(idLike,"%1%");	//编程链风格
        List list = query.list();
        System.out.println("list = " + list);
## 入参对象

```java
String hql = "FROM Info WHERE id = :id AND id LIKE :idLike AND e.dept = ?";
Query query = session.createQuery(hql);	
query.setEntity(new Dept());	//可以入参对象
```

//不要查询表明，查询实体类名

## HQL分页

```java
int pageNo = 3;
int pageSize = 5;
query.setFirstResult((pageNo - 1) * pageSize).setMaxResults(pageSize).list();
```

## 定义查询语句

```xml
    <query name="salaryEmps">FROM Emploee  e where  e.gender = :gender</query>
</hibernate-mapping>
```

```java
Query query = session.getNamedQuery("salaryEmps");
query.setString("gender","M").list();		//可以调用写好的语句
```

## 投影查询

```java
String hql = "SELECT e.email,e.gender FROM Employees WHERE e.name = :name";
List<Object[]> list = session.createQuery(hql);	//返回的是一组组属性的数组	["w@163.com","M"]
```

### 或

```java
String hql = "SELECT new Employee(e.email,e.gender) FROM Employees e WHERE e.name = :name";
//通过这种方式就可以直接返回查询的对象，但是需要写对象的对应构造器，注意构造器的顺序
```

```java
SELECT DINSTINCT e FROM Emploee e LEFT JOIN FETCH d.emps
```

#QBC和本地化SQL

```java
Criteria criteria = session.createCriteria(Emploee.class);
criteria.add(Restrictions.eq("email","xxx@163.com"));	//添加相等条件
criteria.add(Restrictions.gt("salary",5000));		//添加大于条件
Emploee emps = (Emploee) criteria.uniqueResult();//返回一个结果，或者list();返回一组对象集合
//AND
        Conjunction conjunction = new Conjunction();
        conjunction.add(Restrictions.like("name","a", MatchMode.ANYWHERE));
        Emploee emploee = new Emploee();
        emploee.setId(80);
        conjunction.add(Restrictions.eq("dept",dept));
//结果
(name like '%a%' and dept = Deparent [id = 80]);

//or
  Disjunction dis = Restrictions.disjunction();
        dis.add(Restrictions.like("name","a", MatchMode.ANYWHERE));
        Emploee emploee = new Emploee();
        emploee.setId(80);
        dis.add(Restrictions.eq("dept",dept));

criteria.add(conjunction);
criteria.add(dis);	//可以将AND和OR的QBC进行拼接，add之间用AND连接
```
## 统计查询

```java
//max
Criteria criteria = session.createCriteria(Emploee.class);
criteria.setProjection(Projections.max("id"));	//类的属性这里是
//order
criteria.addOrder(Order.asc("salary"));
criteria.addOrder(Order.desc("age"));
```

## 本地SQL

```java
        String sql = "SELECT * FROM tbl_emp WHERE emp_id = ?";
        SQLQuery sqlQuery = session.createSQLQuery(sql);
        sqlQuery.setInteger(0,2048);
        List<Emploee> list = sqlQuery.list();	
```

# 缓存

- 同一个查询语句只发一条SQL语句
- 提交事务关闭session后，会发俩条SQL语句，因为是一级缓存

## 二级缓存

```xml
　　　　 <!-- 开启二级缓存 -->
        <property name="hibernate.cache.use_second_level_cache">true</property>
        <!-- 二级缓存的提供类 在hibernate4.0版本以后我们都是配置这个属性来指定二级缓存的提供类-->
        <property name="hibernate.cache.region.factory_class">org.hibernate.cache.ehcache.EhCacheRegionFactory</property>
        <!-- 二级缓存配置文件的位置 -->
        <property name="hibernate.cache.provider_configuration_file_resource_path">ehcache.xml</property>
```

```
<ehcache>

    <!-- Sets the path to the directory where cache .data files are created.

         If the path is a Java System Property it is replaced by
         its value in the running VM.

         The following properties are translated:
         user.home - User's home directory
         user.dir - User's current working directory
         java.io.tmpdir - Default temp file path -->
　　
　　<!--指定二级缓存存放在磁盘上的位置-->
    <diskStore path="user.dir"/>　　

　　<!--我们可以给每个实体类指定一个对应的缓存，如果没有匹配到该类，则使用这个默认的缓存配置-->
    <defaultCache
        maxElementsInMemory="10000"　　//在内存中存放的最大对象数
        eternal="false"　　　　　　　　　//是否永久保存缓存，设置成false
        timeToIdleSeconds="120"　　　　
        timeToLiveSeconds="120"　　　　
        overflowToDisk="true"　　　　　//如果对象数量超过内存中最大的数，是否将其保存到磁盘中，设置成true
        />
　　
　　<!--
```

　　　　1、timeToLiveSeconds的定义是：以创建时间为基准开始计算的超时时长；
　　　　2、timeToIdleSeconds的定义是：在创建时间和最近访问时间中取出离现在最近的时间作为基准计算的超时时长；
　　　　3、如果仅设置了timeToLiveSeconds，则该对象的超时时间=创建时间+timeToLiveSeconds，假设为A；
　　　　4、如果没设置timeToLiveSeconds，则该对象的超时时间=max(创建时间，最近访问时间)+timeToIdleSeconds，假设为B；
　　　　5、如果两者都设置了，则取出A、B最少的值，即min(A,B)，表示只要有一个超时成立即算超时。

```xml
ehcache配置文件:
<ehcache>

    <!-- Sets the path to the directory where cache .data files are created.

         If the path is a Java System Property it is replaced by
         its value in the running VM.

         The following properties are translated:
         user.home - User's home directory
         user.dir - User's current working directory
         java.io.tmpdir - Default temp file path -->
　　
　　<!--指定二级缓存存放在磁盘上的位置-->
    <diskStore path="user.dir"/>　　

　　<!--我们可以给每个实体类指定一个对应的缓存，如果没有匹配到该类，则使用这个默认的缓存配置-->
    <defaultCache
        maxElementsInMemory="10000"　　//在内存中存放的最大对象数
        eternal="false"　　　　　　　　　//是否永久保存缓存，设置成false
        timeToIdleSeconds="120"　　　　
        timeToLiveSeconds="120"　　　　
        overflowToDisk="true"　　　　　//如果对象数量超过内存中最大的数，是否将其保存到磁盘中，设置成true
        />
　　
　　<!--
　　　　1、timeToLiveSeconds的定义是：以创建时间为基准开始计算的超时时长；
　　　　2、timeToIdleSeconds的定义是：在创建时间和最近访问时间中取出离现在最近的时间作为基准计算的超时时长；
　　　　3、如果仅设置了timeToLiveSeconds，则该对象的超时时间=创建时间+timeToLiveSeconds，假设为A；
　　　　4、如果没设置timeToLiveSeconds，则该对象的超时时间=max(创建时间，最近访问时间)+timeToIdleSeconds，假设为B；
　　　　5、如果两者都设置了，则取出A、B最少的值，即min(A,B)，表示只要有一个超时成立即算超时。
　　-->

　　<!--可以给每个实体类指定一个配置文件，通过name属性指定，要使用类的全名-->
    <cache name="com.xiaoluo.bean.Student"
        maxElementsInMemory="10000"
        eternal="false"
        timeToIdleSeconds="300"
        timeToLiveSeconds="600"
        overflowToDisk="true"
        />

    <cache name="sampleCache2"
        maxElementsInMemory="1000"
        eternal="true"
        timeToIdleSeconds="0"
        timeToLiveSeconds="0"
        overflowToDisk="false"
        /> -->


</ehcache>
```

```xml
//实体类配置
<hibernate-mapping package="com.xiaoluo.bean">
    <class name="Student" table="t_student">
        <!-- 二级缓存一般设置为只读的 -->
        <cache usage="read-only"/>
        <id name="id" type="int" column="id">
            <generator class="native"/>
        </id>
        <property name="name" column="name" type="string"></property>
        <property name="sex" column="sex" type="string"></property>
        <many-to-one name="room" column="rid" fetch="join"></many-to-one>
    </class>
</hibernate-mapping>
二级缓存的使用策略一般有这几种：read-only、nonstrict-read-write、read-write、transactional。注意：我们通常使用二级缓存都是将其配置成 read-only ，即我们应当在那些不需要进行修改的实体类上使用二级缓存，否则如果对缓存进行读写的话，性能会变差，这样设置缓存就失去了意义。
```

##缓存详解

https://www.cnblogs.com/xiaoluo501395377/p/3377604.html

# 工具类

```java
package com.hibernate.utils;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HbnUtils {

    private static SessionFactory sessionFactory;
    /**
     * 
     * @return Session
     */
    public static Session getSession(){
        return getSessionFactory().getCurrentSession();
    }
    /**
     * SessionFactory是重量级的
     * 最好做成单例模式
     * @return SessionFactory
     */
    public static SessionFactory getSessionFactory(){
        //保证SessionFactory为单例
        if (sessionFactory == null ||sessionFactory.isClosed()) {
            sessionFactory = new Configuration().configure().buildSessionFactory();
        }
        return sessionFactory;
    }
}

<!--指定session由谁管理 -->
<property name="current_session_context_class">thread</property>
```

# 批量操作

```java
@Test
public void testBatch(){
    session.doWork(new Work() {
        @Override
        public void execute(Connection connection) throws SQLException {
            //原生JDBCAPI
        }
    });
}
PreparedStanment ps = connection.perparedStament(sql);
for(){
    ps.setInt(0,1);
    ...
    ps.addBatch();
    if((i+1)%300 == 0{	//每300条执行一次提交sql并清空
        ps.executeBatch();
        ps.clearBatch();
    }
}
//如果总数不是每次积攒条数的倍数，则在执行一次
if(count % 300 != 0){
	ps.executeBatch();
	ps.clearBatch();       	
}       
```