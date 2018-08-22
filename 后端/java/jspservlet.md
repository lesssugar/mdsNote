装网页的容器 tomcat

JSTL标签

<%@  taglib  uri="http://java.sun.com/jsp/jstl/core"  prefix="c"  %>

url 网址 统一资源定位符 uniform resource locator 

url的组成   <http://localhost:8080/news/index.html>

协议  主机地址 端口号 网页位置

注释<！--  -->

<%-- --%>

测试tomcat是否启动成功时 .bat文件要打开不能关闭

tomcat 目录

bin 存放运行和停止服务的脚本

conf configurration 配置 存放服务器的各种配置文件

lib 运行所需要的jar文件

logs 日志文件

temp 运行时用于存放临时文件

webapps 应用的发布目录

work 把JSP生成的Serverlet存放在此目录下

网页采用动态还是静态主要取决于网站的功能需求和网站内用的多少，如果网站功能较简单，内容更新量不大采用静态较好，反之，用动态网页

B/S架构维护和升级简单 B/S架构的应用程序业务逻辑完全在服务器端进行实现，B/S界面没有C/S友好，在速度和安全性上要花费巨大的成本，B/S架构的交互式请求/响应，所以信息一旦更新，必须刷新页面才能看到

tomcat的配置方法

preference servers 配置环境 点图标添加项目 选取项目 运行根目录放置 jsp html js css图片等

web - inf 包含web.xml 各种使用的资源 客户端无法访问

web - info/classes 存放各种 class文件

web - info/lib 存放应用使用的jar文件

web.xml可以配置网页的起始网页

<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>

更改jsp的默认编码格式设置:preference ，files and editor，jsp右侧就可以设置

引用多个类文件时用逗号隔开

方法的定义方法:<%!  返回值(参数){

方法体}%>  ！前面不能有空格

JSP的正常写法是将脚本中的代码写入Servlet中的Service()方法中，而用!写的是写到Servlet中的类当中，就相当于类的定义方法，所以可以多次调用

JSP

文件的执行过程:翻译，编译，执行

服务器将接收到的JSP文件翻译成可以执行的java源代码

让后将java源码编译成字节码文件，然后服务器进行执行

编译好的字节码文件保存在服务器内存中，当客户端再次请求相同的JSP时，就可以重新用这个编译好的字节码文件，可以大大提高执行效率

错误:未启动tomcat 无法显示该网页

未部署web项目 404

url输入错误 404

文件放错文件夹 404 

不能放在web-info中

jsp的内置对象 是不要做任何声明就可以使用的对象

如：out.print();

常用的jsp内置对象

所谓的内置对象就是由Web容器加载的一组Serverlet Api的实例

常用对象包括： out request reponse session application

requset对象用于处理其他页面提交过来的信息

方法包括:getParameter("参数的name值") 表单内的name属性值

返回值为 String  

getParameterValues("参数的name值") 返回值为一个String型的数组

当采用的post传递方式时可以用

setCharacterEncodind("UTF-8");方法 设置请求数据解码为utf-8

tomcat解码默认是 ISO-8859-1字符集进行解码

当用get方式传递时可以用

String name = request.getParameter("name");

name = new String(name.getBytes("ISO-8859-1"),"UTF-8");

这种获取方法需要对每一个获取的对象都进行重新设置，不推荐使用

可以在tomcat配置文件中 Connector 中添加URIEncoding属性

URIEcoding="UTF-8"  进行统一设置

getRequestDispatcher(String path);返回一个 对象 该对象的forward方法用于转发请求  转发也可以获得信息

用同一个对象将获得信息转发到别的页面

 form表单有4中method 参数 post 用于修改 input 用于添加

get 用于查询 delete 用于删除

页面传递 错误500 先检查各个页面是否有语法错误

有可能是被传递的页面语法错误  对比字符串一定要用 equals

传递数据  request.getRequestDispatcher.forward(request,response);

a标签的不同跳转

<a href="x.jsp?color=a&id=1">

<a href="x.jsp?color=b&id=2">

此时color就相当于一个name值 可以在传递的页面获取该值

该方式可以用于get表单 

session 会话 用户打开浏览器与服务器端建立连接直到连接断开或者失效就是一次会话的过程

方法:

String getId(); 获取本次会话的id 值

void setMaxInactiveInterval(int interval);设置本次会话时间

getMaxInactinveInterval();获取本次会话的时间；

invalidate() 使对象失效

对象失效后不能在设置session的属性的操作

setAttribute("",""); 以键值对的方式进行存储对象

Object getAttribute("");通过String键值获取对应的对象 

获取的是Object类需要进行强制类型转换 一定要进行强制类型转换

removeAttribute("String");通过String删除对应的键值对

可以在web.xml 中添加标签对

<session-config>

<session-timeout>10</session-timeout>

<session-config>

常用

设置会话的失效时间 10代表10分钟

也可以在tomcat的web.xml中进行同样的设置

指令include

<%@ include list=“x” %>文件路径 可以直接引用别的网页。可以引用网页头，网页尾，验证登录等可以复用的代码

把验证是否已经登录的写入一个Jsp中，直接调用就可以判断是跳转到登录页面还是使用页面了

application 和 session 类似，在客户端程序的一次运行中都有效，有添加属性和删除属性的方法

计算网站浏览的人数统计代码

int count = 0;

  	if(application.getAttribute("count") != null){

  	count = (int)application.getAttribute("count")+1;

  	}else{

  	count = 0;

  	}

  	application.setAttribute("count", count);

  	 %>

  	 <%=count %>

cookie:

可以创建Cookie的对象 存储形式一样是 字符串键值对存储

方法： setMaxAge(int x) 设置cookie的有效期 单位为秒

填写大于0的数字代表cookie的存活时间

等于0 代表删除该cookie

填写负数代表该cookie 会在窗口关闭后失效

   setValue("");为cookie设置新值

getName() getValue() getMaxAge();

可以用response.addCookie(new Cookie("","'));

添加cookie cookie只能通过遍历查询

可以用request.getCookies();获得全部的cookie的数组

然后遍历每个元素的 getName().equals(“”)；判断元素名称

session的name和id也是以 cookie的形式存储的

session是在服务器端保存客户信息，cookie是在客户端保存客户信息

session保存的数据类型是Object类型，使用时候需要进行强制转型，cookie保存的是String类型

JNDI的配置:          JNDI:java naming and directory interface

java命名与目录接口

<context>里写

<Environment name="tjndi" value="hello JNDI" type="java.lang.String" />

type固定

获取该属性使用

Context ctx = new InitialContext();

String jndi = (String)ctx,lookup("java/comp/env/tjndi");

获取name属性为tjndi的value值

数据库连接池再初始化时将创建一定数量的数据库链接放到连接池中，这些数据连接的数量是由最小数据库连接数设定的，最大数据库连接数决定最多的连接数，当应用程序的访问数大于最大连接数量时，这些请求将计入到等待队列中

连接池中不使用的调用.close()方法。会把连接归还到连接池中。

配置:数据源需要在context.xml 里的Context里面配置Resource元素进行连接池的配置

javax.sql.DataSource接口的实现类，负责管理与数据库的连接，以连接池的形式对数据库连接进行管理

Tomcat支持将DataSource实现发布为JNDI资源

Web应用通过JNDI获得DataSource引用

<Context>

   <Resource name="jdbc/news" auth="Container" type="javax.sql.DataSource"

​      maxActive="100" maxIdle="30" maxWait="10000" username="newsu"

​      password="123456" driverClassName="com.mysql.jdbc.Driver"

​      url="jdbc:mysql://127.0.0.1:3306/newsmanagersystem?

​              useUnicode=true&amp;characterEncoding=utf-8" />

</Context>

在Myeclipse2014中新建一个web项目，出现一个warning：

Description Resource Path Location Type

Build path specifies execution environment JavaSE-1.6. There are no JREs installed in the workspace that are strictly compatible with this environment. WebServiceProject_learn1 Build path JRE System Library Problem

解决方案：

右键项目→Build path,选择config build path,选择JRS System Library[JavaSE-1.6]，Edit，选择Workspace default JRE(.....) 【ps：就是选择和电脑装的JRE环境一致】，确定 tomcat不编译问题

EL expression language

语法 ${} 可以快速获取某作用域中的对象

作用域 requestScope responseScope sessionScope appliacationScpoe

获取属性键值对 .键值 list[0] 

关系操作符 无区别

逻辑操作符，无区别

${ request.user.username }

填写list会将list值全部打印 数组[]可以填写下标

param.username 获取前面页面的name="username" 属性的input标签

paramValues. 相当于 getParaNames();

JSTL java server pages standard tag  library

empty a el表达式中检测a是不是null 如果null返回true

pageContext 可以配置内置对象 如 out 

用== 或者 eq 不用 equals()

<c:set var="username" value=""></c>创建一个变量

使用这个值需要 用el表达式读取

可以通过response的getWriter()方法获取对当前页面的PrinterWriter对象

输出完之后要用 location.href跳转页面。不能用response进行跳转

jsp

jsp本质其实就是servlet，jsp是专门用于数据展示的servlet，而普通的servlet适用于逻辑处理的，都是单例多线程的。

！声明的是定义在类当中的，而没有用的是定义在service方法中的

表达式就是out.print( x )的输出流

jsp翻译成的servlet中含有对应的内置对象，所以我们可以直接进行使用对应的session,request,response对象

指令为当前的页面进行设置

分为page指令，include指令和taglib指令

errorPage属性，指定如果出现了异常则跳转到的页面

对应的指令跳转到的页面的page指令的内部添加isErrorPage属性，解析成的servlet中会添加一个内置的exception对象

Include的文件之间变量互相共享，虽然eclipse会报错，需要注意的是因为共性所以两个文件中不要定义相同名字的变量

commons-fileupload

需要导入

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/c39e57c131c44fc5a775479f59f92d22/clipboard.png)

new DiskFileItemFactory();

本地磁盘工厂，构造器可以有两个参数，第一个是上传文件的大小，第二个是本地文件的仓库

//本地文件工厂

DiskFileItemFactory factory = new DiskFileItemFactory();

//通过工厂生成上传组件

ServletFileUpload upload = new ServletFileUpload(factory);

//判断是否是Multipart类型表单

if (upload.isMultipartContent(req)) {

​	//获取上传存储的文件夹地址

​	String path = req.getServletContext().getRealPath("/upload");

​	try {

​		//将request中的请求信息解析成FileItem的集合

​		List<FileItem> fileItems = upload.parseRequest(req);

​		//获得集合的迭代器

​		Iterator<FileItem> iter = fileItems.iterator();

​		//遍历迭代器

​		while (iter.hasNext()) {

​			FileItem next = iter.next();

​			//判读是否是普通表单项

​			if (next.isFormField()) {

​				System.out.println(next.getFieldName() + "=" + next.getString());

​				//文件表单项

​			} else {

​				System.out.println("上传了文件" + next.getName());

​				//为文件重新生成UUID+文件格式的名字

​				String inFileName = UUID.randomUUID().toString().replaceAll("-", "")

​								+ next.getName().substring(next.getName().lastIndexOf("."));

​				//将文件写入到规定好的文件夹当中

​				next.write(new File(path, inFileName));

​			}

​		}

​	} catch (FileUploadException e) {

​		e.printStackTrace();

​	} catch (Exception e) {

​		e.printStackTrace();

​	}

}

}

DiskFileItemFactory factory = new DiskFileItemFactory();

ServletFileUpload upload = new ServletFileUpload(factory);

通过工厂获得一个servlet上传下载的组件，通过获得的组件的parseRequest()方法，解析request请求，获得一组FileItem对象的集合，该集合可以获得一系列的信息，例如，

fileItem.getFieldName() 获得对应表单对象中name属性的值

fileItem.isFormField() 判断该fileItem是否是普通表单项

fileItem.getName() 获取对应的文件的名字

req.setCharacterEncoding("UTF-8");

servlet的路径是相对于web的根路径的，web-root或者web-app路径，普通表单项的值直接通过FileItem.getString();即可获得对应的值，但是每一个都需要先equals()判断其name属性是那一栏

UUID可以为文件命名不重复，如果对一个同位置的文件进行更换时要记得把源文件进行删除

upload.setFileSizeMax(1);

设置上传文件的大小，如果超过范围会抛出异常

验证文件上传类型 创建一个数组其中包括你想要的类型，

.jpg,gif.......

然后将获得文件名，通过截取获得文件格式 .xxx

然后通过集合的.contains()方法判断集合里是否有同名格式的成员

List<String> types = Arrays.asList(".txt",".jpg");

String type = next.getName().substring(next.getName().indexOf("."));

if (types.contains(type)) {

​		System.out.println("包含txt文件");

​		}

FileUploadBase.FileSizeLimitExceededException

文件过大用该异常进行检测

浏览器通过请求报文像服务器发送请求

请求报文 请求行，内容为，请求方式，请求地址，

请求头 浏览器的信息，接收的语言格式等一些浏览器的信息以及想要发送给服务器的信息

请求参数 发送给服务器的数据，内容

响应报文 

状态行 包含响应状态码，是否成功 404页面不存在 304页面被缓存了

响应头 服务器信息，以及服务器想要告诉浏览器的一些信息

响应主体，用户看到的内容

请求 包括请求头，请求行，请求主体，

包括浏览器信息，请求的方式，发送给服务器的数据

状态报文 状态行，响应头，响应主体

包括 服务器的信息，响应是否成功，浏览器中可以看到的信息

这种方式就叫做HTTP协议