# Spring Security

## 组成

安全拦截器分配给不同的组件

认证管理器			访问决策管理器			运行身份管理器

### 常用认证模式

HttpBasic密码使用base64编码进行传输，安全性低，http是无状态协议。每次登陆都需要重新登陆

Digest会对用户名密码http请求方法被请求的uri等信息组合后进行md5运算，发送给服务器

X.509证书认证，包含X.509的版本号，公钥，证书的序列号是由CA给予的编号，主题信息，证书的有效期，证书的发布机构，证书的数字签名，签名的算法等等。

LDAP轻量级的文件访问协议，统一身份认证，使需要认证的软件都使用LDAP服务器进行认证，每个用户只需要一个密码

Form通过表单验证用户的账户密码

## 介绍

### SecurityContextPersistenceFilter

```java
在其他的请求之前，检查用户的上下文信息，如果有就拿出来用，如果没有，创建一个放到SecurityContextHolder中
2.会在所有请求完毕后清除context信息
```

### ThredLocal

```java
 private T setInitialValue() {
        T value = initialValue();
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
        return value;
    }
每个线程都对应一个ThreadLocalMap的本地map变量，get获取的map也是线程本地的map,主要用于解决并发问题，当我们使用ThredLocal中的信息，当我们的请求结束后一定要清除掉其中的信息
```

### LogoutFilter

```java
SecurityContextHolder，处理注销请求，默认处理请求j_spring_security_logout请求，消除用户的session，清空SecurityContextHolder，然后重定向到注销成功页面，可以与remeber等功能配合，清除用户的cookie，
```

### AbstractAuthenticationProcessingFilter

```java
处理form登录的过滤器，默认处理j_spring_security_check请求，通过用户名和密码判断用户登录是否有效
```

### DefaultLoginPageGeneratingFilter

```java
用来生成一个默认的登录页面，默认地址为spring_security_login
```

### BasicAuthenticationFilter

```java
用basic方式进行验证，类似AbstractAuthenticationProcessingFilter
```

### SecurityContextHolderAwareRequestFilter

```java
包装客户的请求，在原来的请求的基础上，对后面的请求提供额外的数据，比如，get,removeUser时，返回user的用户名之类
```

### RememberMeAuthenticationFilter

```java
当cookie带有rememberMe标记时，会根据标记自动实现登录并创建SecurityContext授予对应的权限，用户使用remeberMe实现登录时，就会为用户生成一个唯一标识，用户登录
```

### AnonymousAuthenticationFilter

```java
当用户没有登录时。默认为用户分配匿名用户的权限
```

### ExceptionTranslationFilter

```java
处理抛出的异常FilterSecurityException中抛出的异常然后将请求重定向到对应页面
```

### SessionManagementFilter

```java
防御会话级的攻击，用户登陆成功后销毁他的session并重新生成一个session
```

###　FilterSecurityInterceptor

```java
如果用户没有登录抛出用户未登录的异常，没有访问权限，会抛出访问权限异常，否则则放行
```

### FilterChainProxy

```java
会按照一定的顺序调用一组filter使自己的filter技能完成授权认证的本质工作，又能享有springIoc的功能
```

# 数据库管理

## UserDetailService

```java
public interface UserDetailsService {
    UserDetails loadUserByUsername(String var1) throws UsernameNotFoundException;
}

public interface UserDetails extends Serializable {
    Collection<? extends GrantedAuthority> getAuthorities();	//权限集合

    String getPassword();	//密码

    String getUsername();	//用户名

    boolean isAccountNonExpired();	//账户是否过期

    boolean isAccountNonLocked();	//账户是否被锁定

    boolean isCredentialsNonExpired();	//证书是否过期

    boolean isEnabled();	//账户是否有效
}
//如果任何一个为false用户的凭证就会失效
```

