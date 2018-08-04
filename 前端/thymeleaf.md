# 模板引擎核心组件

## 资源管理



- 模板封装
- 模板解析
- 国际化
- 渲染上下文
- 表达式引擎
- 渲染引擎

mvc过程用户发出请求，前端控制器通过handlerMapping和handlerAdaptor选择对应的控制器，返回modelAndView给前端控制器，前端控制器根据不同的视图解析器寻找对应的模板jsp,volicity,freemaker等，渲染数据，最后将页面返回给用户

```html
<html lang="zh-CN" xmlns:th="http://www.thymeleaf.org" >	命名空间，语法提示
```

##页面的渲染

```html
<div th:text="${hello}"></div>
```

```
<div th:text="${hello}" th:id="${hello}" th:class="${hello}"></div>
//可以对任意属性进行渲染
```

1）、th:text；改变当前元素里面的文本内容；

​	th：任意html属性；来替换原生属性的值

![](H:/springboot%E5%B0%9A%E7%A1%85%E8%B0%B7/%E5%B0%9A%E7%A1%85%E8%B0%B7SpringBoot%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E7%AF%87%EF%BC%88%E4%B8%8A%EF%BC%89/%E6%BA%90%E7%A0%81%E3%80%81%E8%B5%84%E6%96%99%E3%80%81%E8%AF%BE%E4%BB%B6/%E6%96%87%E6%A1%A3/Spring%20Boot%20%E7%AC%94%E8%AE%B0/images/2018-02-04_123955.png)



2）、表达式？

```properties
Simple expressions:（表达式语法）
    Variable Expressions: ${...}：获取变量值；OGNL；
    		1）、获取对象的属性、调用方法
    		2）、使用内置的基本对象：
    			#ctx : the context object.
    			#vars: the context variables.
                #locale : the context locale.
                #request : (only in Web Contexts) the HttpServletRequest object.
                #response : (only in Web Contexts) the HttpServletResponse object.
                #session : (only in Web Contexts) the HttpSession object.
                #servletContext : (only in Web Contexts) the ServletContext object.
                
                ${session.foo}
            3）、内置的一些工具对象：
#execInfo : information about the template being processed.
#messages : methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
#uris : methods for escaping parts of URLs/URIs
#conversions : methods for executing the configured conversion service (if any).
#dates : methods for java.util.Date objects: formatting, component extraction, etc.
#calendars : analogous to #dates , but for java.util.Calendar objects.
#numbers : methods for formatting numeric objects.
#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
#objects : methods for objects in general.
#bools : methods for boolean evaluation.
#arrays : methods for arrays.
#lists : methods for lists.
#sets : methods for sets.
#maps : methods for maps.
#aggregates : methods for creating aggregates on arrays or collections.
#ids : methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).

    Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
    	补充：配合 th:object="${session.user}：
   <div th:object="${session.user}">
    <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
    <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
    <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
    </div>
    
    Message Expressions: #{...}：获取国际化内容
    Link URL Expressions: @{...}：定义URL；
    		@{/order/process(execId=${execId},execType='FAST')}
    Fragment Expressions: ~{...}：片段引用表达式
    		<div th:insert="~{commons :: main}">...</div>
    		
Literals（字面量）
      Text literals: 'one text' , 'Another one!' ,…
      Number literals: 0 , 34 , 3.0 , 12.3 ,…
      Boolean literals: true , false
      Null literal: null
      Literal tokens: one , sometext , main ,…
Text operations:（文本操作）
    String concatenation: +
    Literal substitutions: |The name is ${name}|
Arithmetic operations:（数学运算）
    Binary operators: + , - , * , / , %
    Minus sign (unary operator): -
Boolean operations:（布尔运算）
    Binary operators: and , or
    Boolean negation (unary operator): ! , not
Comparisons and equality:（比较运算）
    Comparators: > , < , >= , <= ( gt , lt , ge , le )
    Equality operators: == , != ( eq , ne )
Conditional operators:条件运算（三元运算符）
    If-then: (if) ? (then)
    If-then-else: (if) ? (then) : (else)
    Default: (value) ?: (defaultvalue)
Special tokens:
    No-Operation: _ 
```

```
//遍历数据
<div th:each="prod : ${prod}">
    <div th:text="${text}"></div>
</div>
<div th:each="user : ${users}">[[${user}]]</div>
```

# 使用注意

模板自带缓存，应该在开发的时候禁用缓存

```properties
spring.thymeleaf.cache=false
```

## 模板的创建与使用

###抽取公共片段

<div th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</div>

###引入公共片段

<div th:insert="~{footer :: copy}"></div>
~{templatename::selector}：模板名::选择器
~{templatename::fragmentname}:模板名::片段名

###默认效果：

添加成功，但是添加的片段会被放在一个div里面，可能会对页面造成一定的影响，insert的公共片段在div标签中
如果使用th:insert等属性进行引入，可以不用写~{}：
行内写法可以加上：[[~{}]];[(~{})]；

三种引入公共片段的th属性：

**th:insert**：将公共片段整个插入到声明引入的元素中

**th:replace**：将声明引入的元素替换为公共片段	所以最好用这个

**th:include**：将被引入的片段的内容包含进这个标签中



```html
<footer th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

引入方式
<div th:insert="footer :: copy"></div>	footer是页面名
<div th:replace="footer :: copy"></div>
<div th:include="footer :: copy"></div>

效果
<div>
    <footer>
    &copy; 2011 The Good Thymes Virtual Grocery
    </footer>
</div>

<footer>
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

<div>
&copy; 2011 The Good Thymes Virtual Grocery
</div>
```

也可以直接用id进行引入

```
<div th:include="footer :: #"></div>
```

引入模板的时候可以为模板上添加变量，我们可以在总模板中定义不同变量的样式

```html
th:class="${active=='login'?'active':''}"//添加对应的样式
th:replace="~{footer::#tooBar(active='login')}"//这样就完成了模板参数的传递
```

# 工具类

##时间

```html
#dates.format(emp.birth,'yyyy/MM/dd HH:mm')	//格式化日期
```

```properties
//提交表单的时候的日期解析器
#配置日期解析器
spring.mvc.date-format=yyyy-MM-dd
```

