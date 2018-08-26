# SpringBoot优缺点与微服务

## 优点

1. 快速创建独立运行的spring项目，并与主流框架继承 
2. 使用嵌入的sertvlet容器，应用无需打成war包 
3. starts启动器自动依赖与版本控制 
4. 大量的自动配置，简化开发，也可修改默认值 
5. 无需配置xml文件，通过api配置，无代码生成，开箱即用 
6. 准生产环境的运行时应用监控 
7. 与云计算的天然集成 
8. 是一个简化spring开发的一个框架，整合spring的技术栈 
## 微服务的特点

一个应用应该是一组小型服务，可以通过http的方式进行沟通，

单体应用	all in one	将一个服务复制多份，放在多台服务器上跑，通过负载均衡，即可提交服务的并发能力，优点，开发测试简单，都在一个应用，编译简单

一体应用通过对项目的水平复制提高项目的负载能力，耦合性高，有可能因为较小的改动需要重启整个服务，而微服务强调的是将项目视为功能元素的组合，每个服务都是可独立替换，可独立升级的功能单元，部署和运维难度较高

# SpringBoot的启动与打包

## 建立maven项目，导入依赖 

```java
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.2.RELEASE</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```
## 程序的启动入口

@SpringBootApplication 

主函数内	SpringApplication.run(HelloWorld.class,args);进行服务的启动 
# SpringBoot的原理 

```java
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.0.2.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

spring-boot-dependencies管理的就是我们项目所需要的依赖，依赖的版本都在里面进行了管理，如果没有在里面规定版本的依赖，我们则需要写版本号 

## 项目启动由不同的场景启动器启动

spring-boot-starter-web 

spring-boot-starter 场景启动器进行启动，我们需要web的模块 ，则引入有关于web的有关依赖	父工程负责依赖版本的控制，starter负责规定引入哪些jar包

https://docs.spring.io/spring-boot/docs/1.5.14.BUILD-SNAPSHOT/reference/htmlsingle/#using-boot-dependency-management

搜索spring-boot-starter-activemq 

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```
## AutoConfigurationPackage

@AutoConfigurationPackage	下有导入注解

@Import({Registrar.class})

Registrar类下有registerBeanDefinitions方法，对其打断点

AnnotationMetadata参数获取的是主入口的元注解@SpringBootApplication	

new AutoConfigurationPackages.PackageImport(metadata)).getPackageName()

计算为主入口的所在包，也就是说将主入口包下的全部注册为组件，一个组件如果不在该包之下则无法进行注册

## @SpringBootConfiguration

表明一个类是一个配置类

@Import({AutoConfigurationImportSelector.class})

### @EnableAutoConfiguration

public @interface EnableAutoConfiguration {

### @Import({AutoConfigurationImportSelector.class})

selectImports方法，会给容器中导入非常多的自动配置类	xxxAutoConfiguration，给容器这个场景需要的所有组件，并配置好这些组件

有了这些自动配置类，免去了我们手动编写配置注入功能组件的工作

### configurations 

SpringFactoriesLoader.loadFactoryNames方法获取第一个参数

EnableAutoConfiguration.class	第二个参数为class.loader

try下第一行

Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");

对资源文件进行获取

包含全部的配置项，如果有需要更改的可以到这里进行更改

# 配置文件

## @ConfigurationProperties(prefix = "person") 

导入文件配置处理器

```java
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-configuration-processor</artifactId>
   <optional>true</optional>
</dependency>
```

对开头为person的全局属性进行注册

​    \- hongloumeng

​    \- xiyouji	列表

## @Value("${person.name}") 

如果我们就是希望调取某个配置文件中的值用一下，那么使用@value

##两者的区别

批量注入与单个注入。	@ConfigurationProperties支持松散语法	first_name,

firstName,first-name都可以，@Value则不支持

@Value支持SpEL，@ConfigurationProperties不支持

@ConfigurationProperties支持Jsr303校验，@Value不支持

在类上写	@Validated	在对应的属性上写对应的校验规则

@Value不会进行JSR303的校验 @Value仅支持简单属性的封装，但是不支持复杂类型的封装

# 引入局部文件属性

**@PropertySource**	我们不想将全部的属性写在全局配置文件中，

@PropertySource("classpath:person.properties")或可以写数组

@PropertySource(value = {"classpath:person.properties"})

写在对应的装配类上，配合@ConfigurationProperties

引入外部的自定义属性文件

Spring Boot中 spring的配置文件<bean id="">不识别

**@ImportResource(locations = {"classpath:bean.xml"})**在程序的主入口写该注解注册自己的spring配置文件

# SpringBoot推荐的添加组件方式

写一个配置类

```java
@Configuration
public class MyAppConfig {
    @Bean
    public HelloService helloService(){
        System.out.println("给容器中添加组件了");
        return new HelloService();
    }
}
```

通过**@bean**的方法注册一个组件，返回值就是要注册的类，方法名就是要注册成的bean的名字

# 属性文件使用占位符添加属性

```java
name: wang${random.value}
age: ${random.int}
name : wangwang${person.hello:hi.name}
```

前两个使用随机数作为填充，最后一个取hello的值，如果没有用**hi**进行填充

# profile

用于配置对不同环境的不同支持

xxxx-{profile}.properties/yml

application-dev.properties	开发环境

application-prod.properties 生产环境

# 更换使用的环境

```
spring.profiles.active=dev
```

## 或者使用yml文件多文档块

```
server:
  port: 9999
spring:
  profiles:
    active: dev
---

server:
  port: 8083
spring:
  profiles: dev
---
server:
  port: 9987
spring:
  profiles: prod
```

第一个控制使用的环境，后面的文档块编写配置环境

## 启动jar包的时候配置环境

或者在运行jar包的时候添加 --spring.profiles.active=dev

## 优先级

命令行优先，文档块其次，总配置文件最低

# 配置文件可以放的地方

```java
file:./config/  直接写在项目路径下
file:./	直接写在项目路径下
classpath:/config		resoureces/config/application.properties
classpath:/	application.properties
```

上面的顺序优先级由高到低，高优先级的会覆盖低优先级的配置，可以形成配置互补

## 添加外置配置文件的地址

```
spring.config.location=D:application.properties
```

启动jar包的时候添加--spring.config.location=D:application.properties

# 配置项

## 搜索Appendices application文件配置

```
#配置项目的访问端口
server.port=9999
#配置项目的环境
spring.profiles.active=prod
#配置项目的入口	//项目的入口老版本 server.context-path=/demo 
server.servlet.context-path=/demo 	//登录的时候	http://localhost:9999/demo/hello才可以
```

# 外部配置文件的顺序
```java
1.命令行参数	java -jar initboot-0.0.1-SNAPSHOT.jar --server.port=10000
2.来自java:com/env的JNDI属性	
3.java系统属性
4.操作系统环境变量
5.RandomValueProperty配置的random.* 属性值
6.打出来的jar包外applicatio-(profile)带环境的文件
7.打出来jar包内applicatio-(profile)带环境的文件、
8.打出来jar包外applicatio不带环境的文件
9.打出来jar包内applicatio不带环境的文件
10.@Configuration类注解上的@PropertySource
11.通过SpringApplication.setDefaultProperties指定的默认属性
6.78较常用
```

# 自动配置原理

- springboot启动的时候加载主配置类，开启了自动配置功能，@EnableAutoConfiguration
- @EnableAutoConfiguration的作用
- AutoConfigurationImportSelector给容器中倒入一些组件
- List<String> configurations = getCandidateConfigurations(annotationMetadata,      attributes);

获取候选配置

 - SpringFactoriesLoader.loadFactoryNames()
 - .loadFactoryNames()从类路径下获得一个资源

```
Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");
把扫描到的文件的内容包装成properties对象
从properties中获取EnableConfiguration.class（类名）对应的值，然后把它们添加到容器中
```

## HttpEncodingAutoConfiguration类举例

```
@Configuration	//表示这是一个配置类
@EnableConfigurationProperties(HttpEncodingProperties.class)//启用某各类中的配置
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)//根据不同的条件，如果满足条件，整个配置类才会生效，当前这个注解判断是否是web应用，不是则不生效
@ConditionalOnClass(CharacterEncodingFilter.class)//判断当前项目有没有这个类，该类是springMvc解决乱码的类
@ConditionalOnProperty(prefix = "spring.http.encoding", value = "enabled", matchIfMissing = true)//判断配置文件中是否存在spring.http.encoding.enabled配置，如果不存在也会生效
public class HttpEncodingAutoConfiguration {
```
	@EnableConfigurationProperties(HttpEncodingProperties.class)
	
	@ConfigurationProperties(prefix = "spring.http.encoding")
	public class HttpEncodingProperties {
	由此可以看出我们需要配置的属性都在一个个HttpEncodingProperties.class的类中
如果该配置类通过检查，则通过@Bean为容器中添加组件

```
@ConfigurationProperties(prefix = "spring.http.encoding")
public class HttpEncodingProperties {
```

如果想进行配置对应的属性就spring.http.encoding.force等等即可

```
private final HttpEncodingProperties properties;
该类只有一个有参构造器
public HttpEncodingAutoConfiguration(HttpEncodingProperties properties) {
		this.properties = properties;
	}
这种情况下构造器执行就会从spring容器中拿HttpEncodingProperties，而该类是通过
//@EnableConfigurationProperties(HttpEncodingProperties类并进行绑定.class)注册HttpEncodingProperties类并进行绑定

```

```
@Bean
@ConditionalOnMissingBean
public CharacterEncodingFilter characterEncodingFilter() {
   CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
   filter.setEncoding(this.properties.getCharset().name());
   filter.setForceRequestEncoding(this.properties.shouldForce(Type.REQUEST));
   filter.setForceResponseEncoding(this.properties.shouldForce(Type.RESPONSE));
   return filter;
}
这些都是进行的配置，而this.properties.getCharset().name()取值就从配置文件中取
```

## 所以我们能配置的属性都是来自properties类

## 如何查看哪些配置类生效了

配置文件中配置	debug=true

启动控制台出现

```java
============================
CONDITIONS EVALUATION REPORT
============================

Positive matches:
```

下面的内容就是配置报告，这样我们就以查看哪些自动配置类生效了

# 日志

| 抽象层            | 实现层                    |
| ----------------- | ------------------------- |
| JCL,slf4j,logging | logback,log4j,log4j2，JUL |

推荐使用slf4j，logback

## SLF4J使用

开发的实话，日志记录的调用，不应该直接调用实现类，而应该调用日志抽象层中的方法

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

log4j开发时并未考虑到抽象层的问题，所以如果需要已给中间层slf4j-log412的jar包

![concrete-bindings](https://www.slf4j.org/images/concrete-bindings.png)

## 遗留问题

a:(slf4j+logback):spring(commons-logging),hibernate(jboss-logging)Mybatis.xx

同一日志

![concrete-bindings](https://www.slf4j.org/images/legacy.png)

如果相对不同的框架更换实现，在中间添加一个中间层 如commons-logging用jcl-over-slf4j.jar进行替换，所以我们再引入新框架的时候应该移除新框架的日志依赖

## 关于日志的配置

spring默认使用的是info级别的

```java
#日志级别
logging.level.com.wyq.initboot=trace
#日志输出地址
logging.path=log	//log指的是项目下的文件夹，日志文件默认名字叫spring.log
logging.pattern.console=
```

 SpringBoot默认帮我们配置好了日志；在

下的logging下的defaults可以进行查看

## 指定配置

给类路径下放上每个日志框架自己的配置文件即可；SpringBoot就不使用他默认配置的了

| Logging System          | Customization                                                |
| ----------------------- | ------------------------------------------------------------ |
| Logback                 | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml` or `logback.groovy` |
| Log4j2                  | `log4j2-spring.xml` or `log4j2.xml`                          |
| JDK (Java Util Logging) | `logging.properties`                                         |

logback.xml：直接就被日志框架识别了；

**logback-spring.xml**：日志框架就不直接加载日志的配置项，由SpringBoot解析日志配置，可以使用SpringBoot的高级Profile功能

```xml
<springProfile name="staging">
    <!-- configuration to be enabled when the "staging" profile is active -->
  	可以指定某段配置只在某个环境下生效
</springProfile>
```

如：

```xml
<appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!--
        日志输出格式：
			%d表示日期时间，
			%thread表示线程名，
			%-5level：级别从左显示5个字符宽度
			%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
			%msg：日志消息，
			%n是换行符
        -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <springProfile name="dev">	//在这里进行分别不同的级别
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ----> [%thread] ---> %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
            <springProfile name="!dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ==== [%thread] ==== %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
        </layout>
    </appender>
```

## 切换日志框架

切换为log4j2

```xml
//排除掉原来的  spring-boot-starter-logging
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>spring-boot-starter-logging</artifactId>
                    <groupId>org.springframework.boot</groupId>
                </exclusion>
            </exclusions>
   </dependency>

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```
# SpingBoot与Web开发

## 静态资源文件的处理	mvc的自动配置类下

```
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
   if (!this.resourceProperties.isAddMappings()) {
      logger.debug("Default resource handling disabled");
      return;
   }
   Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
   CacheControl cacheControl = this.resourceProperties.getCache()
         .getCachecontrol().toHttpCacheControl();
   if (!registry.hasMappingForPattern("/webjars/**")) {
      customizeResourceHandlerRegistration(registry
            .addResourceHandler("/webjars/**")
            .addResourceLocations("classpath:/META-INF/resources/webjars/")
            .setCachePeriod(getSeconds(cachePeriod))
            .setCacheControl(cacheControl));
   }
   String staticPathPattern = this.mvcProperties.getStaticPathPattern();
   if (!registry.hasMappingForPattern(staticPathPattern)) {
      customizeResourceHandlerRegistration(
            registry.addResourceHandler(staticPathPattern)
                  .addResourceLocations(getResourceLocations(
                        this.resourceProperties.getStaticLocations()))
                  .setCachePeriod(getSeconds(cachePeriod))
                  .setCacheControl(cacheControl));
   }
}
```

所有/webjars/**都去classpath:/META-INF/resources/webjars/找资源

webjars的方式引入静态资源

![1528119456386](C:\Users\ADMINI~1\AppData\Local\Temp\1528119456386.png)

以此类推如果我们需要对应的静态资源只需要引入对应的maven约束即可

如果我们发送了localhost:8080/webjars/abc则会到/META-INF/resources/webjars/下找

![1528119856613](C:\Users\ADMINI~1\AppData\Local\Temp\1528119856613.png)

###我们自己的静态资源，下半部分的资源

```
private String staticPathPattern = "/**";
```

任何路径，如果没有控制器进行处理，则到

###静态资源文件放置地址

```
private static final String[] CLASSPATH_RESOURCE_LOCATIONS = {
      "classpath:/META-INF/resources/", "classpath:/resources/",
      "classpath:/static/", "classpath:/public/" };
```

下进行寻找，这些文件夹就是静态资源文件夹,main下面的文件都是类路径的开始，可以再下面建上面的应用

###欢迎页

```
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(
      ApplicationContext applicationContext) {
   return new WelcomePageHandlerMapping(
         new TemplateAvailabilityProviders(applicationContext),
         applicationContext, getWelcomePage(),
         this.mvcProperties.getStaticPathPattern());
}
```

```java
getWelcomePage()
return this.resourceLoader.getResource(location + "index.html");
```

```java
this.mvcProperties.getStaticPathPattern());
被这个进行映射	/**寻找默认静态资源文件夹下的index.html
```

###配置喜欢的图标，标题栏内的

```
@Configuration
@ConditionalOnProperty(value = "spring.mvc.favicon.enabled", matchIfMissing = true)
public static class FaviconConfiguration implements ResourceLoaderAware {

   private final ResourceProperties resourceProperties;

   private ResourceLoader resourceLoader;

   public FaviconConfiguration(ResourceProperties resourceProperties) {
      this.resourceProperties = resourceProperties;
   }

   @Override
   public void setResourceLoader(ResourceLoader resourceLoader) {
      this.resourceLoader = resourceLoader;
   }

   @Bean
   public SimpleUrlHandlerMapping faviconHandlerMapping() {
      SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
      mapping.setOrder(Ordered.HIGHEST_PRECEDENCE + 1);
      mapping.setUrlMap(Collections.singletonMap("**/favicon.ico",
            faviconRequestHandler()));
      return mapping;
   }
```

favicon.ico放在静态资源文件夹下即可

##thymeleaf

如果想切换jar包的版本直接在pom文件中进行修改即可

```
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

   private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;

   public static final String DEFAULT_PREFIX = "classpath:/templates/";

   public static final String DEFAULT_SUFFIX = ".html";
```

thymeleaf的properties文件，说明静态页面只要放在templates文件下，就会被该视图解析器解析

## SpringMVC自动配置

https://docs.spring.io/spring-boot/docs/1.5.10.RELEASE/reference/htmlsingle/#boot-features-developing-web-applications

### 1. Spring MVC auto-configuration

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.

  - 自动配置了ViewResolver（视图解析器：根据方法的返回值得到视图对象（View），视图对象决定如何渲染（转发？重定向？））
  - ContentNegotiatingViewResolver：组合所有的视图解析器的；

  ```java
  public View resolveViewName(String viewName, Locale locale) throws Exception {//解析视图
  //获取全部的视图对象
  List<View> candidateViews = this.getCandidateViews(viewName, locale, requestedMediaTypes);
  //获取最适合的视图对象
  View bestView = this.getBestView(candidateViews, requestedMediaTypes, attrs);
  ```

  - ==如何定制：我们可以自己给容器中添加一个视图解析器；自动的将其组合进来；==

  ```java
  @Component
  private static class MyViewResolver implements ViewResolver{
      @Override
      public View resolveViewName(String s, Locale locale) throws Exception {
          return null;
      }
  }//这样就会被注册为一个视图解析器,可以在前端控制器查询到
  ```

- Support for serving static resources, including support for WebJars (see below).静态资源文件夹路径,webjars

- Static `index.html` support. 静态首页访问

- Custom `Favicon` support (see below).  favicon.ico

  

- 自动注册了 of `Converter`, `GenericConverter`, `Formatter` beans.

  - Converter：转换器；  public String hello(User user)：类型转换使用Converter
  - `Formatter`  格式化器；  2017.12.17===Date；

  	@Bean
  	@ConditionalOnProperty(prefix = "spring.mvc", name = "date-format")//在文件中配置日期格式化的规则
  	public Formatter<Date> dateFormatter() {
  		return new DateFormatter(this.mvcProperties.getDateFormat());//日期格式化组件
  	}
  ==自己添加的格式化器转换器，我们只需要放在容器中即可==

- Support for `HttpMessageConverters` (see below).

  - HttpMessageConverter：SpringMVC用来转换Http请求和响应的；User---Json；

  - `HttpMessageConverters` 是从容器中确定；获取所有的HttpMessageConverter；

    ==自己给容器中添加HttpMessageConverter，只需要将自己的组件注册容器中（@Bean,@Component）==

    

- Automatic registration of `MessageCodesResolver` (see below).定义错误代码生成规则

- Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).

  ==我们可以配置一个ConfigurableWebBindingInitializer来替换默认的；（添加到容器）==

  ```
  初始化WebDataBinder；
  请求数据=====JavaBean；
  
  ```

**org.springframework.boot.autoconfigure.web：web的所有自动场景；**

## 如果修改默认配置

先查看容器中有没有用户自己配置的组件，如果有用户配置的就先用用户配置的，如果没有，则使用其默认的

## 扩展springmvc

```xml
    <mvc:view-controller path="/hello" view-name="success"/>
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/hello"/>
            <bean></bean>
        </mvc:interceptor>
    </mvc:interceptors>
```

编写一个配置类（@Configuration），实现WebMvcConfigurer接口重写方法

```java
@Configuration
public class MyConfiguration implements WebMvcConfigurer{
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("nihao").setViewName("sucess");
    }
}//直接注册了一个映射
```

#使用篇

##初始页面的设置

```java
@Configuration	//2.0
public class WebViewMvcAdapter implements WebMvcConfigurer{
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
    }
}
```

```
@Bean   	//1.5版本的方法，其中接口已经被废弃
public WebMvcConfigurerAdapter webMvcConfigurerAdapter() {
    WebMvcConfigurerAdapter adapter = new WebMvcConfigurerAdapter() {
        @Override
        public void addViewControllers(ViewControllerRegistry registry) {
            registry.addViewController("/").setViewName("index");
            registry.addViewController("/index.html").setViewName("index");
        }
    };
    return adapter;
}
```

## 添加对应的文件

```html
@{/webjars/bootstrap/4.1.0/css/bootstrap.css}
```

```
@{/asserts/css/signin.css}		//不加static文件夹，加webjars的到引入的webjar中找，其余的请求先寻找控制器，找不到到几个精通资源文件夹中寻找
```

## 国际化

编写国际化配置文件

![1528268068824](C:\Users\ADMINI~1\AppData\Local\Temp\1528268068824.png)

通过文件的resource bundle进行编辑

```properties
spring.messages.basename=i18n.index   //其中最后需要对应 index才可以
```

```java
@ConfigurationProperties(prefix = "spring.messages")
public class MessageSourceAutoConfiguration {
/**
 * Comma-separated list of basenames (essentially a fully-qualified classpath
 * location), each following the ResourceBundle convention with relaxed support for
 * slash based locations. If it doesn't contain a package qualifier (such as
 * "org.mypackage"), it will be resolved from the classpath root.
 */
private String basename = "messages";  
//我们的配置文件可以直接放在类路径下叫messages.properties；

@Bean
public MessageSource messageSource() {
	ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
	if (StringUtils.hasText(this.basename)) {
        //设置国际化资源文件的基础名（去掉语言国家代码的）
		messageSource.setBasenames(StringUtils.commaDelimitedListToStringArray(
				StringUtils.trimAllWhitespace(this.basename)));
	}
	if (this.encoding != null) {
		messageSource.setDefaultEncoding(this.encoding.name());
	}
	messageSource.setFallbackToSystemLocale(this.fallbackToSystemLocale);
	messageSource.setCacheSeconds(this.cacheSeconds);
	messageSource.setAlwaysUseMessageFormat(this.alwaysUseMessageFormat);
	return messageSource;
}
```
###实现传参国际化

```java
//重写一个自己的本地化解析器,这样就会解析request中的locale参数
public class MyLocaleResolver implements LocaleResolver {

    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        String localeStr = httpServletRequest.getParameter("locale");
		//对参数进行分解为语言和国家
        Locale locale = Locale.getDefault();
        if (!StringUtils.isEmpty(localeStr)){
            String[] locs = localeStr.split("_");
            //生成locale对象并返回
            locale = new Locale(locs[0], locs[1]);
        }
        return locale;
    }
```

```java
@Bean//然后将其在一个配置类注册为组件
public LocaleResolver localeResolver(){
    return new MyLocaleResolver();
}
```

## RestFul风格注解

@PostMapping	以默认post的方式发送请求，一次类推四种注解

@RestController	@Controller及@ReponseBody的结合体

##重定向

```
redirect:/main.html	
```

## 拦截器

```java
HandlerInterceptor接口
```

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {
    //
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object loginUser = request.getSession().getAttribute("loginUser");
        if (loginUser==null){
            request.setAttribute("msg","没有登录，请先进行登录");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }else{
            return true;
        }
    }
   //定义拦截器
```

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
    //添加自定义拦截器，拦截全部请求，除了初始登录页面，及初次跳转页面
    registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**").excludePathPatterns("/","/index.html","/user/login");
}//在WebMvcConfigurer中重写该方法，将拦截器注册组件
```

## 提交表单日期格式

默认解析日期是按照	/	的方式可以配置解析器

```
#配置日期解析器
spring.mvc.date-format=yyyy-MM-dd	来修改日期
```

##事物

```java
@EnableTransactionManagement	//启动类添加该注解，开启事物的支持
在对应的service层方法上添加@Transactional
```



#错误处理

原理

```
ErrorMvcAutoConfiguration类进行管理
```

给容器添加一下组件

```
DefaultErrorAttributes
```

```
BasicErrorController
	@RequestMapping({"${server.error.path:${error.path:/error}}"})
public class BasicErrorController extends AbstractErrorController {
```

```java
ErrorPageCustomizer
    @Value("${error.path:/error}")
    private String path = "/error";
		//处理请求之后产生html类型的数据
        @RequestMapping(
        produces = {"text/html"}
    )
    public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
        HttpStatus status = this.getStatus(request);
        Map<String, Object> model = Collections.unmodifiableMap(this.getErrorAttributes(request, this.isIncludeStackTrace(request, MediaType.TEXT_HTML)));
        response.setStatus(status.value());
        ModelAndView modelAndView = this.resolveErrorView(request, response, status, model);
        return modelAndView != null ? modelAndView : new ModelAndView("error", model);
    }
	//处理并产生json类型的数据
    @RequestMapping
    @ResponseBody
    public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
        Map<String, Object> body = this.getErrorAttributes(request, this.isIncludeStackTrace(request, MediaType.ALL));
        HttpStatus status = this.getStatus(request);
        return new ResponseEntity(body, status);
    }
```

```
DefaultErrorViewResolver
```

一旦系统错误ErrorPageCustomizer生效定制错误的响应规则系统错误来到error请求进行处理，就会来到/error请求，BasicErrorController就会在用户没有配置的情况下处理/error请求，浏览器发送请求的请求头包含

Accept:"text/html",而客户端访问的accept中 accept:"*/*"，所以会用不同的方法进行处理，原理暂停
##使用方法
可以在模板下放置error/404.html文件，或各个错误状态码的文件，可以用4xx.html包括所有的400错误，5xx.html涵盖所有的500错误，可以做一个404，其余的用4xx概括

## 页面可以获取的信息

​	timestamp:时间戳

​	status:状态码

​	error：错误提示

​	exception:异常对象

​	message:异常消息

​	errors:JSR303数据校验的错误，可以用模板表达式进行获取

# 集成

## MyBatis

```
        <!--数据库驱动-->	//////////////更换
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
         <!--myBatis启动器-->
        <dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.1</version>
		</dependency>
		
		//指定xml文件的位置
		mybatis.mapper-locations=classpath:com/wyq/boot/dao/mapper/*.xml

#数据源的配置
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/ssm-crud?useUnicode=true&characterEncoding=utf-8
spring.datasource.username=root
spring.datasource.password=123456

@MapperScan("com.wyq.boot.dao")	//在启动类上写，可以直接注册全部的mapper接口
或者为每个mapper接口添加	@Mapper注解

@Service层接口实现类写注解，自动注入写接口
```

## xml文件无法加载的问题

```xml
//java包下的xml文件找不到，在build下添加这些配置即可
		<resources>
			<resource>
				<directory>src/main/resources</directory>
				<includes>
					<include>**/*.*</include>
				</includes>
			</resource>
			<resource>
				<directory>src/main/webapp</directory>
				<includes>
					<include>**/*.*</include>
				</includes>
			</resource>
			<resource>
				<directory>src/main/java</directory>
				<includes>
					<include>**/*.xml</include>
				</includes>
			</resource>
		</resources>
```
## devtools热部署插件

```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-devtools</artifactId>
   <optional>true</optional>
</dependency>	//保存后会自动重启项目
```

该插件会默认禁止spring-boot如模板等的进行缓存，防止进行的修改不生效，在类路径下的文件被监控，eclipse保存，idea重新build会重启服务器，restart加载器会被丢弃，新建一个加载器和原俩加载器一同工作

## 设置不需要重启的文件

```xml
/META-INF/maven ， /META-INF/resources ， /resources ， /static ， /public或/templates
spring.devtools.restart.exclude = static / **，public / ** 
```

