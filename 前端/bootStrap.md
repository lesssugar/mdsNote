el和jstl

<%@  taglib  uri="http://java.sun.com/jsp/jstl/core"  prefix="c"  %>

expression language 表现语言

主要用于替换JSP页面中的脚本表达式

执行运算

获取web常用开发对象

调用java方法

${data} = ${pageContext.getAttribute("data")};

pageContext包含四个作用域对象

void setAttribute(String name, Object value, int scope)	

如果查询的属性没有找到则返回空字符串，返回null会对页面产生影响

person.name 调取的是javabean中的getName()方法

访问list用list名[]访问对应数据

改动java代码要重新进行编译，而修改jsp中数据则有可能不用

empty()函数可以检测一个键值对是否是null或者为空

${user ！= null?user.name:"游客";}

el表达式不支持字符串的连接，+两边只能是数字

数据回显 <input type="text" name="username" 

value="${empty username?username:"";}">

将数据封装到javabean中发回。然后填上

<input type="radio" name="gender" value="male" ${gender=="male"?checked : '';}>

使用三元运算符:两边要加空格。不知道为啥。。。。，empty后面加空格在填获取的属性就可以判断该属性是否是null或者为空集合

可以直接在servlet forward方法后从param中获取到数据进行数据回显

<c:out value="" ecspaXml="true"> 可以填写html代码，填写false则会解析

<c:if test=""></c> 写条件

  <c:choose>

  	<c:when test=""></c:when>

  	<c:otherwise></c:otherwise>

  </c:choose>

选择标签

迭代标签

<c:forEach var="循环的变量名" items=""循环的集合 varStatus="x">

x中包含的信息如当前循环到的下标等信息