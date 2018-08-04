spring的测试可以使用spring的测试组件

导入springContext的单元模块

@RunWith(SpringJUnit4ClassRunner.class)

@ContextConfiguration(locations={"classpath:applicationContext.xml"})

在对应的类上面打上注解

spring

开源的轻量级框架，一站式框架。

spring在三层架构中都提供了相应的技术进行支持

aop:面向切面编程，扩展功能不是改源代码实现

ioc:控制反转 

web层：springMVC

service：用到ioc的部分

dao层用到spring的jdbcTemplate

Ioc

1.将对象的创建直接交给ioc容器

ioc配置文件的操作。注解的操作

bean实例化的三种方式

1使用类的无参构造来实现

就是最常用的方式，要保证无参构造存在

2使用静态工厂创建

类里面创建一个静态的方法，返回我们所需要的实例，成为静态工厂

<bean id="bean2" class="cn.Bean2Factory" factory-method="getBean2">

通过调用静态工厂中的getBean2方法获得bean2的实例，复杂，所以不适用

3使用实例工厂来创建

创建一个不是静态的方法返回这个类的实例

<bean id="工厂类" class="工厂类" >

<bean id="bean3" factory-bean="工厂类" factory-method="getBean3">

2.3基本不会使用，了解认识即可

ioc的底层原理:

xml的配置文件，dom4j解析xml

工厂设计模式,反射

第一步创建配置文件。配置配置对象的类。

<bean id=UserService class="cn.UserService"> 

工厂类

public Class UserFactory{

//返回UserService对象的方法

public static UserService getSetvice(){

解析文件根据id值得到对应的class值

String classValue =  "class的值";

//通过反射获得类对象

Class clazz = Class.forName(classValue);

UserService service = clazz.newInstance();

return service;

}

}

该过程是通过反射进行获得类的实例对象，在类的信息被改变了的情况下，不需要更改与其相关的使用该类的文件。只需要更改对应的配置文件就可以。

spring文件建议放在src目录下使用方便，名字无固定要求

ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

然后通过context.getBean("");方法获取对应的实例，参数填写配置文件中的javaBean的id。

加载总的配置文件

ApplicationContext是一个接口，用于读取Spring配置文件，管理对象的加载，生成，维护Bean对象与Bean对象的依赖关系，负责Bean的生命周期等，是BeanFactory接口的子接口，BeanFactory是SpringIoc的核心。

配置文件会找类中的无参构造进行实例化对象。若果没写无参构造会报错

面向切面编程AOP,是软件发展到一定程度的产物，是对面向对象编程的友谊补充，AOP一般适用于具有横切编辑的场合，如访问控制，事务管理，性能检测

切面，一个模块化的横切逻辑，可能会横切多个对象

连接点，程序执行中的某个具体的执行点。

增强处理，切面在某个特定点上执行的代码逻辑。

切入点，对连接点的特征进行描述，可以使用正则表达式，增强处理的一个切入点表达式相关联，并在与这个切入点匹配的某个连接点上运行。

目标对象，被一个切面，或多个切面增强的对象。

AOP代理，有AOP框架所创建的对象，实现执行增前处理方法等功能。

织入，将增强处理连接到应用程序中的类型或对象上的过程。

增强处理类型，放在前面的成为前值增强，后置增强，还有环绕增强，异常抛出增强，最终增强类型等

bean标签中常用属性的介绍

id    就是起的名字，用于调取时候使用

class id所指向的类的完全限定名，实例化的类

name  不使用，和id属性是一致的。里面可以加符号

scope singleton单例的，默认值 prototype多例的

request创建一个对象放在request域中

session创建一个对象放在session域中

globalSession创建一个对象放在全局Session中。

后面三个有所了解即可

属性的三种注入方式

1 set()方法

2.有参的构造器

3.接口注入，实现接口的方法对属性进行设值。

在service层写入dao层的引用变量，然后写出他的set方法

在配置文件配置注入的关系

配置dao和service的对象，然后在service中的属性，通过ref属性引入dao层的对象，不用value是因为value只能写字符串类型，ref引用的是关联对象

注入复杂类型的属性 数组

<property name="xxx"> array类型的xxx

<list>

<value> 写多个value

<property name="xxx"> list类型的xxx

<list>

<value> 写多个value

<property name="xxx"> map类型的xxx

<map>

<entry key="" value=""></entry>

<property name="xxx"> properties类型的xxx

<props>

<prop key="driver">此处写value值</entry>写驱动。例子

IOC和DI的区别 把对象的创建交给spring进行处理

DI是依赖注入 想类里面的属性中设值

关系，DI是不能单独存在，需要在IOC的基础之上完成操作

每次使用都需要newApplicationContext比较浪费资源，所以写成在服务器启动的时候加载该对象

原理:1.在服务器启动的时候每个项目会有一个ServletContext对象

2.监听器

在服务器启动的时候为每个项目创建一个ServletContext对象，在该对象创建的时候，使用监听器可以监听到servletContext在上么时候创建。

使用监听器，监听到该对象创建的时候，加载spring配置文件，把对象进行创建，会把创建出来的对象放到一个servletContext域对象里面去。带我们需要的时候可以通过getAttruibute()获取我们所需要的对象

IOC的注解方式实现

导入基础jar包

引入新的配置文件约束

注解中的一些特殊的标记，可以直接完成一些相关的功能

​	<!--开启注解的扫描，后面填写包名，包过多时可以写cn -->

<context:component-scan base-package="cn.printer.entity"></context:component-scan>

扫描类上面，方法上面属性上面的注解

在要注册的类上面填写注解就可以实现。@Component(value="printer") value可以省略。

@controller

@service

@Repository

目前这四个功能是一样的。都创建对象。为后续版本做铺垫

@Scope(value="protoType")配置对象是多实例，还是单实例对象

AOP概念

面向切面编程，扩展功能不通过改写代码来实现

从用横向抽取机制取代传统的纵向继承体关系

纵向继承关系中的父类方法中的方法如果改变了。则子类中也要跟着改变

AOP底层使用的是动态代理的方式，再有接口的情况下，创建接口实现类代理对象。这个对象不是真正对象，代理对象，实现和Impl对象相同的功能。

在无接口的情况下，是创建类的子类对其进行增强，达到一个代理的效果.cglib动态代理

AOP中的术语

连接点 类中的哪些方法可以被增强就叫做连接点

切入点 重点，在类里面，可以有很多的方法被增强，我们实际增强的那个方法就被成为切入点

增强，advice 实际增强的逻辑就成为增强，例如日志的功能

分为，前置，后置，异常，最终，环绕增强

后置之后执行为最终通知，在方法之前和之后进行执行，如计算方法运行的总时间

切面 把我们的增强应用到具体的方法上面

引介可以动态的像类中添加属性和方法，一般不用

在spring里面使用aop的操作，需要使用aspectJ,是一个面向切面的框架。

1.基于aspectJ的配置文件方式，

2.通过注解的方式使用aspectJ

使用表达式配置切入点 execution(<访问修饰符><返回类型>（<参数>）<异常>)

常用写法(1)execution(* cn.testaop.Book.add(..))

(2)execution(* cn.testaop.Book.*(..))类中所有方法

(3)execution(* cn.testaop.*.*(..))不常用

(4)execution(* cn.testaop.Booke.save*(..))所有Book中以save开头的方法

环绕增强

public void around1(ProceedingJoinPoint proceedingJoinPoint) throws Throwable{

System.out.println("前置增强");

proceedingJoinPoint.proceed();

System.out.println("后置增强");

}

然后进行配置 method和point-cut配置文件

使用注解进行属性设值不需要写set方法，会自动进行生成

@Autowired 自动装填，UserDao是一个类名，根据类名称找到对应的对象，跟装填类的spring-id无关，用的不多，看起来不清晰，并没有指定对象

@Resource(name="")注入的哪个对象，填写通过注解配置的spring对象的value值，name不能省略

bean标签中也可以填写一个类的构造函数

​		<constructor-arg index="0">

​			<ref bean=""/>

​		</constructor-arg>

方法中的返回值定义为object类型，要在标签中将变量名称写入该变量名称。要写在after-returning中。所以两者区别是一个有返回值，一个没有。

最终增强 after ，武略方法异常抛出还是正常退出，该方法都会得到正常执行，类似于fianlly的作用，一般用于释放资源，使用最终增强，就可以为各功能模块提供统一的，可拔插的处理方式。