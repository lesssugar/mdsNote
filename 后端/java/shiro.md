# 基本使用                                                                

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
                //用户登录
                currentUser.login(token);
            }
            catch (UnknownAccountException uae) {
                //未知的账户异常
                return; 
            }
            catch (IncorrectCredentialsException ice) {
                //用户存在,但密码不对的异常
                return; 
            }
            catch (LockedAccountException lae) {
                //账户被锁定异常
            }
            catch (AuthenticationException ae) {
                //上面所有异常的父类
            }
        }

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

## 自定义Realm

```java
public class MyRealm1 implements Realm {
    public String getName() {
        return "myRealm1";
    }
	//判断该realm是否支持该次比对
    public boolean supports(AuthenticationToken authenticationToken) {
        return authenticationToken instanceof UsernamePasswordToken;
    }
	//进行登录测试
    public AuthenticationInfo getAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        String userName = (String) authenticationToken.getPrincipal();
        String passWord = new String((char[]) authenticationToken.getCredentials());
        if (!"zhang".equals(userName)) {
            throw new UnknownAccountException("该用户不存在");
        }
        if (!"123".equals(passWord)) {
            throw new IncorrectCredentialsException("密码错误");
        }
        return new SimpleAuthenticationInfo(userName,passWord,getName());
    }
}
```
## 自定义realm的整合

```ini
myRealm1=com.crud.filter.shiro.realms.MyRealm1
myRealm2=com.crud.filter.shiro.realms.MyRealm2
securityManager.realms=$myRealm1，$myRealm2
//SecurityManager会按照制定的顺序进行检查，如果没写realms会按注册顺序，写了一个其他的会被忽略
```

# Realm

![realm](https://7n.w3cschool.cn/attachments/image/wk/shiro/5.png)

一般继承**AuthorizingRealm**（身份验证）进行使用，继承自**CachingRealm**，所以自带缓存

- IniRealm,PropertiesRealm，通过配置文件的方式
- JdbcReamlm通过sql查询对应的信息

## JdbcRealm

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.25</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>0.2.23</version>
</dependency>
```

```ini
//shiro.ini的数据源整合
jdbcRealm=org.apache.shiro.realm.jdbc.JdbcRealm
dataSource=com.alibaba.druid.pool.DruidDataSource
dataSource.driverClassName=com.mysql.jdbc.Driver
dataSource.url=jdbc:mysql://localhost:3306/shiro
dataSource.username=root
dataSource.password=root
jdbcRealm.dataSource=$dataSource
securityManager.realms=$jdbcRealm
//shiro会用自己默认的sql语句到数据库中进行比对，可以使用shiro.sql类似创建数据库表

//指定检验方式的默认实现
[main]
#指定securityManager的authenticator实现
authenticator=org.apache.shiro.authc.pam.ModularRealmAuthenticator
securityManager.authenticator=$authenticator
#指定securityManager.authenticator的authenticationStrategy
allSuccessfulStrategy=org.apache.shiro.authc.pam.AllSuccessfulStrategy
securityManager.authenticator.authenticationStrategy=$allSuccessfulStrategy
```

#Authenticator  

```java
//测试的核心，包含ModularRealmAuthenticator实现类，包含三种不同的验证规则，默认使用2
FirstSuccessfulStrategy		//只要有一个验证成功即返回信息，其他的忽略
AtLeastOneSuccessfulStrategy//返回所有成功认证的信息
AllSuccessfulStrategy		//所有Realm认证成功才算成功，有一个失败就失败
//也可以继承AbstractAuthenticationStrategy来自己定义策略
```

# 授权

- 主体 subject，即访问的用户
- 资源 用户需要获得授权才可以对资源进行访问或者修改
- 权限  原子授权单位，可以给用户分配操作某个资源的权利

```java
//确定权限三种方式
if(subject.hasRole(“admin”)) {
    //有权限
} else {
    //无权限
}
@RequiresRoles("admin")
public void hello() {
    //有权限
};
<shiro:hasRole name="admin">
<!— 有权限 —>
</shiro:hasRole>
```

```ini
[users]		//密码，角色1，角色2
zhang=123,role1,role2
wang=123,role1&nbsp;
```

```java
//通过角色粗粒度管理权限，如果代码更改，角色权限更改，需要在代码中多处进行更改
@Test
public void testHasRole() {
    login("classpath:shiro-role.ini", "zhang", "123");
    //判断拥有角色：role1
    Assert.assertTrue(subject().hasRole("role1"));
    //判断拥有角色：role1 and role2 and !role3
    boolean[] result = subject().hasRoles(Arrays.asList("role1", "role2", "role3"));
    Assert.assertEquals(true, result[0]);
    Assert.assertEquals(true, result[1]);
    Assert.assertEquals(false, result[2]);
};

@Test	//check的方法如果没有通过会抛出异常
public void testCheckRole() {
    login("classpath:shiro-role.ini", "zhang", "123");
    //断言拥有角色：role1
    subject().checkRole("role1");
    //断言拥有角色：role1 and role3 失败抛出异常
    subject().checkRoles("role1", "role3");
};
```

```java
//基于资源的访问控制（显示角色）
[users]
zhang=123,role1,role2
wang=123,role1
[roles]
role1=user:create,user:update
role2=user:create,user:delete

role3="system:user:create,update,delete,view"//对资源的多个权限
subject().checkPermissions("system:user:create,delete,update,view");
role4=system:user:*	//对user的全部权限
subject().checkPermissions("system:user:*");
role5=*:view
subject().checkPermissions("user:view")
role6=*:*:view

role72="user:update,delete:1"	//对user的1实例拥有update,delete权限
```

```java
@Test
public void testIsPermitted() {
    login("classpath:shiro-permission.ini", "zhang", "123");
    //判断拥有权限：user:create
    Assert.assertTrue(subject().isPermitted("user:create"));
    //判断拥有权限：user:update and user:delete
    Assert.assertTrue(subject().isPermittedAll("user:update", "user:delete"));
    //判断没有权限：user:view
    Assert.assertFalse(subject().isPermitted("user:view"));
};

@Test
public void testIsPermitted() {
    login("classpath:shiro-permission.ini", "zhang", "123");
    //判断拥有权限：user:create
    Assert.assertTrue(subject().isPermitted("user:create"));
    //判断拥有权限：user:update and user:delete
    Assert.assertTrue(subject().isPermittedAll("user:update", "user:delete"));
    //判断没有权限：user:view
    Assert.assertFalse(subject().isPermitted("user:view"));
};
```

# Remeber me

关闭浏览器后下次打开还可以记住你是谁，下次无需登录即可访问

```xml
<bean id="sessionIdCookie" class="org.apache.shiro.web.servlet.SimpleCookie">
    <constructor-arg value="sid"/>
    <property name="httpOnly" value="true"/>
    <property name="maxAge" value="-1"/>
</bean>
<bean id="rememberMeCookie" class="org.apache.shiro.web.servlet.SimpleCookie">
    <constructor-arg value="rememberMe"/>
    <property name="httpOnly" value="true"/>
    <property name="maxAge" value="2592000"/><!-- 30天 -->
</bean>

http://172.18.40.147:9090/gjzs2.0/pages/calculator.html?type=xejkhtjf
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
    
    <!--  
    2. 配置 CacheManager. 
    2.1 需要加入 ehcache 的 jar 包及配置文件. 
    -->     
    <bean id="cacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
        <property name="cacheManagerConfigFile" value="classpath:ehcache.xml"/> 
    </bean>
    
    <bean id="authenticator" 
    	class="org.apache.shiro.authc.pam.ModularRealmAuthenticator">
    	<property name="authenticationStrategy">
    		<bean class="org.apache.shiro.authc.pam.AtLeastOneSuccessfulStrategy"></bean>
    	</property>
    </bean>
    
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
    
    <!--  
    4. 配置 LifecycleBeanPostProcessor. 可以自定的来调用配置在 Spring IOC 容器中 shiro bean 的生命周期方法. 
    -->       
    <bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>
    
    <!--  
    5. 启用 IOC 容器中使用 shiro 的注解. 但必须在配置了 LifecycleBeanPostProcessor 之后才可以使用. 
    -->     
    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"
          depends-on="lifecycleBeanPostProcessor"/>
    <bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
        <property name="securityManager" ref="securityManager"/>
    </bean>
    
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
  <!--shiro-->
    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <property name="realms">
            <list>
                <ref bean="jdbcRealm"/>
            </list>
        </property>
    </bean>

    <bean id="jdbcRealm" class="com.crud.filter.realms.ShiroRealm">
    
    </bean>

    <bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>
    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"
          depends-on="lifecycleBeanPostProcessor"/>
    <bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
        <property name="securityManager" ref="securityManager"/>
    </bean>

    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <property name="securityManager" ref="securityManager"/>
        <!-- 项目首页  -->
        <property name="loginUrl" value="/index.jsp"/>
        <!-- 登录认证成功后的首页 -->
        <property name="successUrl" value="/success.jsp"/>
        <!-- 登录成功后访问某些页面但是没权限时跳转到的页面 -->
        <property name="unauthorizedUrl" value="/unauthorized.jsp"/>
        <!-- 配置相应jsp页面访问权限 -->
        <property name="filterChainDefinitions">
            <value>
                /login.jsp = anon
                /shiro/login = anon
                /shiro/logout = logout
                //想要表单提交可以被访问，需要添加
			   /shiro/shiroLogin = anon
                /user.jsp = roles[user]
                /admin.jsp = roles[admin]

                # everything else requires authentication:
                /** = authc
            </value>
        </property>
    </bean>
```

## 拦截器的配置

```xml
/xxx/xx = xx	//路径,方式	? * **	一个字符，一个路径，随意路径
anon	//不需要认证就可以访问
authc	//必须要认证才可以进行访问
路径匹配，第一次匹配的优先
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

# 基本使用

```java
    <form action="shiro/shiroLogin" method="post">
        userName: <input type="text" name="userName">
        passWord: <input type="password" name="passWord">
        <input type="submit" value="Submit">
    </form>
```

## 登录

```java
@Controller
@RequestMapping("/shiro")
public class ShiroHandler {
    @RequestMapping("/login")
    public String login(@RequestParam("userName") String userName, @RequestParam("passWord") String passWord) {
        //获取一个交互的对象
        Subject currentUser = SecurityUtils.getSubject();
        //测试用户是否登陆成功
        if (!currentUser.isAuthenticated()) {
            //创建令牌
            UsernamePasswordToken token = new UsernamePasswordToken(userName, passWord);
            token.setRememberMe(true);
            try {
                currentUser.login(token);
            } catch (AuthenticationException e) {
                System.out.println("登陆失败" + e.getMessage());
            }
        }
        return "success";
    }
    //退出登录
    subject.logout();
}
```

## Realm

```java
//login方法的比对会到该出进行比对，不用配置文件
public class ShiroRealm extends AuthenticatingRealm {

    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        //将AuthenticationToken强制转换为UsernamePasswordToken
        UsernamePasswordToken token = (UsernamePasswordToken) authenticationToken;
        String username = token.getUsername();
        //从数据库中获取userName和userName所对应的信息
        if("unknow".equals(username)){
            throw new UnknownAccountException("用户不存在");
        }
        //根据用户情况创建
        Object principal = username;
        //密码
        Object credentials = "123456";
        //realm的name
        String realmName = getName();
        SimpleAuthenticationInfo info = new SimpleAuthenticationInfo(principal,credentials,realmName);
        return info;
    }
}
```



