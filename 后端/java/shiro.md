#约束

```
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
  <version>1.7.25</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>1.7.25</version>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-all</artifactId>
  <version>1.3.2</version>
</dependency>
```

# 导入ini配置文件

```ini
//获取当前subject
Subject currentUser = SecurityUtils.getSubject();

//获取session
Session session = currentUser.getSession();
session.setAttribute("someKey", "aValue");
String value = (String) session.getAttribute("someKey");
if (value.equals("aValue")) {
    log.info("---> Retrieved the correct value! [" + value + "]");
}                                                                         
```

```java
public class Quickstart {
    private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);
    public static void main(String[] args) {
        //获取一个交互的对象
        Subject currentUser = SecurityUtils.getSubject();
        //获取一个session
        Session session = currentUser.getSession();
        session.setAttribute("someKey", "aValue");
        String value = (String) session.getAttribute("someKey");
        if (value.equals("aValue")) {
            log.info("---> Retrieved the correct value! [" + value + "]");
        }
        //测试用户是否登陆成功
        if (!currentUser.isAuthenticated()) {
            //创建令牌
            UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
            token.setRememberMe(true);
            try {
                //用户的路
                currentUser.login(token);
            }
            catch (UnknownAccountException uae) {
                //未知的账户异常
                log.info("----> There is no user with username of " + token.getPrincipal());
                return; 
            }
            catch (IncorrectCredentialsException ice) {
                //用户存在,但密码不对的异常
                log.info("----> Password for account " + token.getPrincipal() + " was incorrect!");
                return; 
            }
            catch (LockedAccountException lae) {
                //账户被锁定异常
                log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                        "Please contact your administrator to unlock it.");
            }
            catch (AuthenticationException ae) {
                //上面所有异常的父类
            }
        }
        log.info("----> User [" + currentUser.getPrincipal() + "] logged in successfully.");

        //检查用户是否拥有角色
        if (currentUser.hasRole("schwartz")) {
            log.info("----> May the Schwartz be with you!");
        } else {
            log.info("----> Hello, mere mortal.");
            return; 
        }

        //检查用户是否具有某个行为
        if (currentUser.isPermitted("lightsaber:weild")) {
            log.info("----> You may use a lightsaber ring.  Use it wisely.");
        } else {
            log.info("Sorry, lightsaber rings are for schwartz masters only.");
        }
		
        if (currentUser.isPermitted("user:delete:zhangsan")) {
            log.info("----> You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                    "Here are the keys - have fun!");
        } else {
            log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
        }
        System.out.println("---->" + currentUser.isAuthenticated());
        currentUser.logout();        
        System.out.println("---->" + currentUser.isAuthenticated());
        System.exit(0);
    }
}
```

# 整合spring

```xml
	<!--shiro过滤器-->
	<filter>
		<filter-name>shiroFilter</filter-name>
		<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
		<!-- 设置true由servlet容器控制filter的生命周期 -->
		<init-param>
			<param-name>targetFilterLifecycle</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>shiroFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

```

## application.xml

    <?xml version="1.0" encoding="UTF-8"?>
    
    <beans xmlns="http://www.springframework.org/schema/beans"
    
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    
    	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- =========================================================
         Shiro Core Components - Not Spring Specific
         ========================================================= -->
    <!-- Shiro's main business-tier object for web-enabled applications
         (use DefaultSecurityManager instead when there is no web environment)-->
    <!--  
    1. 配置 SecurityManager!
    -->     
    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <property name="cacheManager" ref="cacheManager"/>
        <property name="authenticator" ref="authenticator"></property>
        
        <property name="realms">
        	<list>
    			<ref bean="jdbcRealm"/>
    			<ref bean="secondRealm"/>
    		</list>
        </property>
        
        <property name="rememberMeManager.cookie.maxAge" value="10"></property>
    </bean>
    
    <!-- Let's use some enterprise caching support for better performance.  You can replace this with any enterprise
         caching framework implementation that you like (Terracotta+Ehcache, Coherence, GigaSpaces, etc -->
    <!--  
    2. 配置 CacheManager. 
    2.1 需要加入 ehcache 的 jar 包及配置文件. 
    -->     
    <bean id="cacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
        <!-- Set a net.sf.ehcache.CacheManager instance here if you already have one.  If not, a new one
             will be creaed with a default config:
             <property name="cacheManager" ref="ehCacheManager"/> -->
        <!-- If you don't have a pre-built net.sf.ehcache.CacheManager instance to inject, but you want
             a specific Ehcache configuration to be used, specify that here.  If you don't, a default
             will be used.: -->
        <property name="cacheManagerConfigFile" value="classpath:ehcache.xml"/> 
    </bean>
    
    <bean id="authenticator" 
    	class="org.apache.shiro.authc.pam.ModularRealmAuthenticator">
    	<property name="authenticationStrategy">
    		<bean class="org.apache.shiro.authc.pam.AtLeastOneSuccessfulStrategy"></bean>
    	</property>
    </bean>
    
    <!-- Used by the SecurityManager to access security data (users, roles, etc).
         Many other realm implementations can be used too (PropertiesRealm,
         LdapRealm, etc. -->
    <!-- 
    	3. 配置 Realm 
    	3.1 直接配置实现了 org.apache.shiro.realm.Realm 接口的 bean
    -->     
    <bean id="jdbcRealm" class="com.atguigu.shiro.realms.ShiroRealm">
    	<property name="credentialsMatcher">
    		<bean class="org.apache.shiro.authc.credential.HashedCredentialsMatcher">
    			<property name="hashAlgorithmName" value="MD5"></property>
    			<property name="hashIterations" value="1024"></property>
    		</bean>
    	</property>
    </bean>
    
    <bean id="secondRealm" class="com.atguigu.shiro.realms.SecondRealm">
    	<property name="credentialsMatcher">
    		<bean class="org.apache.shiro.authc.credential.HashedCredentialsMatcher">
    			<property name="hashAlgorithmName" value="SHA1"></property>
    			<property name="hashIterations" value="1024"></property>
    		</bean>
    	</property>
    </bean>
    
    <!-- =========================================================
         Shiro Spring-specific integration
         ========================================================= -->
    <!-- Post processor that automatically invokes init() and destroy() methods
         for Spring-configured Shiro objects so you don't have to
         1) specify an init-method and destroy-method attributes for every bean
            definition and
         2) even know which Shiro objects require these methods to be
            called. -->
    <!--  
    4. 配置 LifecycleBeanPostProcessor. 可以自定的来调用配置在 Spring IOC 容器中 shiro bean 的生命周期方法. 
    -->       
    <bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>
    
    <!-- Enable Shiro Annotations for Spring-configured beans.  Only run after
         the lifecycleBeanProcessor has run: -->
    <!--  
    5. 启用 IOC 容器中使用 shiro 的注解. 但必须在配置了 LifecycleBeanPostProcessor 之后才可以使用. 
    -->     
    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"
          depends-on="lifecycleBeanPostProcessor"/>
    <bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
        <property name="securityManager" ref="securityManager"/>
    </bean>
    
    <!-- Define the Shiro Filter here (as a FactoryBean) instead of directly in web.xml -
         web.xml uses the DelegatingFilterProxy to access this bean.  This allows us
         to wire things with more control as well utilize nice Spring things such as
         PropertiesPlaceholderConfigurer and abstract beans or anything else we might need: -->
    <!--  
    6. 配置 ShiroFilter. 
    6.1 id 必须和 web.xml 文件中配置的 DelegatingFilterProxy 的 <filter-name> 一致.
                      若不一致, 则会抛出: NoSuchBeanDefinitionException. 因为 Shiro 会来 IOC 容器中查找和 <filter-name> 名字对应的 filter bean.
    -->     
    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <property name="securityManager" ref="securityManager"/>
        <property name="loginUrl" value="/login.jsp"/>
        <property name="successUrl" value="/list.jsp"/>
        <property name="unauthorizedUrl" value="/unauthorized.jsp"/>
        
        <property name="filterChainDefinitionMap" ref="filterChainDefinitionMap"></property>
        
        <!--  
        	配置哪些页面需要受保护. 
        	以及访问这些页面需要的权限. 
        	1). anon 可以被匿名访问
        	2). authc 必须认证(即登录)后才可能访问的页面. 
        	3). logout 登出.
        	4). roles 角色过滤器
        -->
        <!--  
        <property name="filterChainDefinitions">
            <value>
                /login.jsp = anon
                /shiro/login = anon
                /shiro/logout = logout
                
                /user.jsp = roles[user]
                /admin.jsp = roles[admin]
                
                # everything else requires authentication:
                /** = authc
            </value>
        </property>
        -->
    </bean>
    
    <!-- 配置一个 bean, 该 bean 实际上是一个 Map. 通过实例工厂方法的方式 -->
    <bean id="filterChainDefinitionMap" 
    	factory-bean="filterChainDefinitionMapBuilder" factory-method="buildFilterChainDefinitionMap"></bean>
    
    <bean id="filterChainDefinitionMapBuilder"
    	class="com.atguigu.shiro.factory.FilterChainDefinitionMapBuilder"></bean>
    
    <bean id="shiroService"
    	class="com.atguigu.shiro.services.ShiroService">
    </bean>

```xml
基本配置
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">

    <!-- 业务层ioc,扫描除了控制器之外的全部组件 -->
    <context:component-scan base-package="com.crud">
        <context:exclude-filter type="annotation"
                                expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    <context:component-scan base-package="com.crud.service"/>

    <!-- 引入properties文件 -->
    <context:property-placeholder location="classpath:database.properties"/>

    <!-- 配置DataSource -->
    <bean id="dataSource" destroy-method="close"
          class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!-- 配置一个可以执行多条语句的sqlSession -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>
        <constructor-arg name="executorType" value="BATCH"></constructor-arg>
    </bean>

    <!-- 配置SqlSessionFactoryBean -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 引用数据源组件 -->
        <property name="dataSource" ref="dataSource"/>
        <!-- 引用MyBatis配置文件中的配置 -->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations">
            <list>
                <value>classpath:mapper/*.xml</value>
            </list>
        </property>
    </bean>

    <!-- 扫描mapper文件的接口 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="cn.crud.dao"/>
    </bean>

    <!-- 导入springMvc的配置文件 -->
    <import resource="springmvc-servlet.xml"/>

    <!-- aop注解的扫描 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

    <!-- 定义事务管理器,名字在tx内部有默认联系 -->
    <bean id="transactionManager"
          class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <tx:advice id="txAdvice">
        <tx:attributes>
            <tx:method name="get*" read-only="true" propagation="SUPPORTS"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <!-- 定义事物的切面 -->
    <aop:config>
        <aop:pointcut id="serviceMethod" expression="execution(* com.crud.service..*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="serviceMethod"/>
    </aop:config>

    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <property name="cacheManager" ref="cacheManager"/>
        <property name="realms" value="jdbcRealm"></property>
    </bean>
    <!--配置shiro的缓存管理器-->
    <bean id="cacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
        <property name="cacheManagerConfigFile" value="classpath:ehcache.xml"/>
    </bean>

    <bean id="jdbcRealm" class="com.crud.filter.realms.ShiroRealm"></bean>
    <!--shiro生命周期的bean的管理器,可以自动的配置在spring ioc 容器中shiro bean的生命周期方法-->
    <bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>

    <!--  启用 IOC 容器中使用 shiro 的注解. 但必须在配置了 LifecycleBeanPostProcessor 之后才可以使用 -->
    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"
          depends-on="lifecycleBeanPostProcessor"/>
    <bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
        <property name="securityManager" ref="securityManager"/>
    </bean>

    <!--shiro过滤器id 必须和 web.xml 文件中配置的 DelegatingFilterProxy 的 <filter-name> 一致.-->
    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <property name="securityManager" ref="securityManager"/>
        <property name="loginUrl" value="/login.jsp"/>
        <property name="successUrl" value="/list.jsp"/>
        <property name="unauthorizedUrl" value="/unauthorized.jsp"/>

        <!--
            配置哪些页面需要受保护.
            以及访问这些页面需要的权限.
            1). anon 可以被匿名访问
            2). authc 必须认证(即登录)后才可能访问的页面.
            3). logout 登出.
            4). roles 角色过滤器
        -->
        <property name="filterChainDefinitions">
            <value>
                /login.jsp = anon
                /shiro/login = anon
                /shiro/logout = logout

                /user.jsp = roles[user]
                /admin.jsp = roles[admin]

                # everything else requires authentication:
                /** = authc
            </value>
        </property>
    </bean>
</beans>
```

```java
public class ShiroRealm implements Realm{
    public String getName() {
        return null;
    }

    public boolean supports(AuthenticationToken authenticationToken) {
        return false;
    }

    public AuthenticationInfo getAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        return null;
    }
}
```

# 工作过程

```java
shiro filter //拦截全部的请求
```

