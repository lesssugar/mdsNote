servlet

响应的字符集设置

resp.setContentType("text/html;charset=UTF-8");

PrintWriter out = resp.getWriter();

out.print("<script type='text/javascript'>"					+ "alert('输入的账号或密码错误，请重新输入！');" + "</script>");			req.getRequestDispatcher("index.jsp").forward(req, resp);

request.setCharacterEncoding("UTF-8");

name = new String(name.getBytes("ISO8859-1"), "UTF-8");

运行在服务器上(tomcat)，的服务器端小程序

实质是一个JAVA类

开发动态的WEB资源 是基于request-response编程模型

请求响应模型

生命周期是一个实例在特定的时间点我们可控的部分，包括实例化，初始化，服务，销毁

创建一个类继承自HttpServlet

重写一个doxxx方法 基本就doGet();或doPost();方法

servlet配置 为了让服务器可以找到java文件 在web.xml中配置

<servlet>

<servlet-name>H</servlet-name>

<servlet-class>cn.HelloServlet</servlet-class>

</servlet>

<servlet-mapping>

<servlet-name>H</servlet-name>

<url-pattern>/hello</url-pattern>

<servlet-mapping>

http://local/days04/hello

运行url时会寻找mapping中url-pattern中/hello的标签对

寻找servlet-name根据name到servlet中寻找对应名字，找对应类

再根据你请求的方法是get，还是post相应的执行类中的doGet或者doPost方法

需要导入 servlet-api.jar 在tomcat的lib文件夹下

Servlet是单类多线程的，一个实例只会执行一次无参构造器和

因为是单类多线程的所以一般不定义变量。因为所有的线程都可以修改这个变量

init方法

当第一个请求到来的时候Servelet实例化，容器启动的时候，Servlet不会被实例化，实例化会调用无参构造器，初始化方法，和service()方法

更换浏览器不会重新创建无参构造器和初始化方法，每提交一次请求会使用一次service()方法，所以只能执行一次destory方法；该种方法第一个用户第一次响应时间较长，有可能会给第一个用户带来不好的感受。

重要的servlet可以在容器启动的时候进行创建，

在<servlet>标签里面添加<load-on-startup>对标签

里面可以添加数字数字越小的优先级越高 0最高，如果填写负数则不创建，容器启动的时候不创建

在服务器中存在有两个map 第一个map的key放的是 url-pattern value是servlet引用 第二个当中放的引用与类的地址的对应然后通过servlet到第二map中寻找对应的类的地址然后将查出来的类的实例根据反射机制放到第一个map里对应的value里

servletInfo()放的是对当前servlet的说明文字比如servlet的版本，作者，sevlet所在的应用，了解即可

servletConfig() 是容器像初始对象传递信息用的

就是web.xml中的配置标签对

可以通过初始化方法获得的config对象获取标签中的信息

如servlet名

config = 包名类型对象名

getServletName()

还可以获取初始化参数。自己在servlet标签对里自己写

<init-param><param-name><param-value>

config.getInitParameter([param-name]);

也可以使用枚举类进项遍历

Enumeration<String> names = config.getInitParameterNames();

while(names.hasMoreElements()){

String name = namse.nextElement()；

String value = config.getInitParameter(name);

Syso(name +"="+ value);

}

上下文就是整个servlet的整个环境，一个项目只有一个servlet上下文 包含初始化的参数，还可以包含属性

存放属性的地方称为域 则属性也可以叫域属性

context

写在<context-param>标签对里

<context-name><context-value>标签对里和上面遍历方法一样 可以获取上下文标签中的配置信息所有的Servlet中都可以读取通过实例化

ServletContext 对象 = config.getServletContext();

对象.setAttribute("","");

可以设置

通过getAttribute可以在其他的Servlet中获取

可以在任意的servlet中重新设置属性

也可以删除属性

contextPath()获取到的是应用的名称

realPath("/文件夹") 获取文件夹在本地的绝对路径

tomcat中默认的欢迎页面是 index.html .htm .jsp

<url-pattern>中间可以写成多级 tomcat后自己进行匹配

/xx/yy/zz 精确路径模式

一个servlet可以写多个url-pattern与之匹配

通配符形式

/xxx/*意识是url模式可以写/xxx/后面随便

全路径模式 /*    什么路径都能匹配 所有请求同匹配，包括html，jsp，即使是jpg.mp3

被该模式servlet拦截的页面不会显示

/也是全路径模式  只会拦截静态资源请求，对于动态资源请求是不会进行拦截的

*.xxx 拦截所有已.xxx为结尾的请求 一般写do或者action

用于框架

加后缀与路径模式不能一起使用。否则会报错

PrinterWriter 类使用完需要使用期flush方法进行刷新

然后记得关闭

路径优先后缀匹配原则 精确路径优先匹配原则

最长路径优先匹配原则 同一个网页长路径闭短路径更精确一点

Servlet方法只有service方法是我们主要关心的。

所以出现了Servlet的子类

Generic类

放空四个方法单独实现Service方法

适配器设计模式  缺省适配器设计模式

父类中的方法可以直接用 this.getServletConfig()获取

config对象

模板方法设计模式

Servlet初始化时会执行每一个的init()方法，所以在父类中写一个空参空方法让子类重写会因为多态而执行子类重写的方法

，写在父类的init()方法中成为模板方法设计模式

可以检测用户的提交方式。如果用户提交方式为get说明是非法提交，表单提交才是post

GenericServlet类集成Servlet类实现了Service方法并实现了ServletConfig接口实现了，获取上下文对象context，项目名，

获取初始参数名和获得全部参数名的方法

post提交方式现阶段仅能见到表单提交和ajax

HttpServletRequset是ServletRequest的子类除了具有父类可以正常获取表单等传递的信息外。还包括getMethod();获取表单提交方式

当常亮和字符串变量进行比较时，应该将字符串放在前面，这样不会报空指针异常，因为字符串常亮不可能为空

HttpServletRequest是为了处理Http请求生成的

还有HttpServletResponse

通过request获得的网页提交方法，则可以进行分发，post进入自己写的doPost()方法中，get进入doGet()方法中；传入request与response对象这样后面的类继承这个类就可以直接写入操作

doGet()里可以直接写get方式进行的处理方法

doPost()里可以直接写post提交方式的处理

这样就形成了自定义的简易版HttpServlet

可以直接创建servlet类用于直接注册

一次请求对应一个请求对象。

调取doGet,doPost时候开始并没做任何事情，浏览器默认调取一次service()方法才会进行请求的分发

HttpServletRequset 接口的实现类是 HttpServletFacade

包含一次请求全部的请求信息

当一次请求结束时则该对象被销毁

注册servlet可以用注解@webservlet("/url") 注解不写分号

请求函数是存储在Map中的，这个Map的key是请求参数的名称为String类型，Map的value是请求该参数的所有值，为String[]类型。

request.getContextPath();获取当前web应用的根路径，已经自己带了斜杠，所以自己不用写斜杠

getRequesutUrl()；获取请求的url 

getRequesutUri()；去除协议及主机及端口号测内容

getServletPath();获取url匹配的精确部分

getPathInfo();获取url匹配的非精确部分

如果没有非精确部分返回null

Http的底层协议是基于tcp/ip协议的以字节流传递的协议。在接收请求和响应的时候回产生乱码。Tomcat默认字符编码集是ISO-8859-1这种编码不支持汉字，所以需要设置字符编码

tomcat9.0对于get方式的提交解决了中文乱码的问题

其他版本还没有解决，所以需要使用在tomcat文件中进行配置

这种方法不太好，可以使用，

String name = request.getParameter("name");

byte[] bytes = name.getBytes("ISO8859-1");

name = new String(bytes , "UTF-8");

将接收到的字符解码，然后编码成utf格式

这种方式对于get方式，post方式都有效

不过比较麻烦，最好用于get方式

PrinterWriter 的out对象尽量不要自己关闭 应该由response被销毁后服务器自己销毁

重定向可以防止表单的重提交，防止恶意的提交

重一个servlet到另一个servlet最好使用的是重定向

若一个请求的servlet会占用服务器的大量资源，则该servlet处理完毕后，也应该用servlet进行重定向

使用forward()方法时候输出流还未开启

使用include()方法时候回包含当前的输出流，带到下一个servlet

所以使用forward方法时，该servlet中的response中不应该写入输出流数据。若要写入数据，应该使用include进行。

访问路径问题

url   资源路径与资源名称

web项目中的绝对路径即url

绝对路径 = 参照路径 + 相对路径

浏览器会为不同的相对路径添加不同的参照路径，使其成为绝对路径

以 / 开头的相对路径 

前台路径 由浏览器所进行解析解析的路径成为前台路径

如 html css js中的路径 及jsp中的静态的路径 静态部分就是

如<img src = " " />等 都是以/开头的，其实也是属于html的那一部分

这写路径的参照路径是 web服务器的根路径，即 <http://127.0.0.1:8080>

举例为显示img标签需要 /路径需要写上项目名称才能正确加载图片

表单也属于前台路径

有服务器所执行的文件中所执行的代码及文件中的路径。例如Java代码中的路径，jsp的动态部分(java代码块中的路径)，xml等配置文件是要被java代码解析后读取到内存的，其中的路径会出现在java代码中

后台参照路径的是web应用的根路径 <http://127.0.0.1:8080>/项目名称

以路径名称开头的绝对路径

后端路径

比如说xml中的配置文件路径直接写包名

因为参照路径是当前web应用的根目录，所以可以访问

前台路径的作用是查找，后台作用的作用是标识

表单提交路径写 /xxx 就是查找该项目路径下的/xxx路径

response.sendRedirect("");

对于该方法完成的重定向，若它是/开头的，则不是后台路径

这个方法不仅可以完成在当前项目中资源的跳转，还可以跳转到其他项目中的资源。/所以服务器不会给他加项目路径，因为有可能跳转的是其他项目。

可以在前面添加项目路径名，不过不太好，最好添加request.getContextPath()方法获取当前项目名

只有这一种重定向是特例 用/时候才有这种情况 不用斜杠则可以完成正常的路径定向

不以/开头的路径格式

以路径名称开头的路径，其参照路径是当前访问路径的|"资源路径"

xml中的配置文件不可以去除/因为他的作用是用于标识，不会被访问，所以不会产生对应的绝对路径。所以必须以/开头

所以如果/可加可不加的路径，建议最好加上/

servlet是单例多线程下运行的，所以可能存在线程安全问题

存在多线程并发访问

存在可修改的共享数据

当多个线程同时修改一个共享数据时，后修改的数据会将先修改的数据覆盖，对数据先进行修改的用户进行查询时读取到的不是自己所修改的数据，这就是线程安全问题

栈内存是多例的，即JVM会为每个线程创建一个栈。所以其中的数据不是共享的，另外，方法中的局部变量存放在stack的栈帧中，方法执行完毕，栈帧弹栈，局部变量消失。局部变量是局部的不是共享的，所以栈内存的数据不存在线程安全问题

一个JVM中只有一个堆内存，堆内存是共享，存放在堆内存中的数据，实际上就是对象成员变量的集合，及成员变量是存放在堆内存中的，堆内存中的数据时多线程共享的，也就是说对内存中的数据存在线程安全问题

方法区

一个JVM中只包含一个方法区，静态变量和常量都存放在方法区，还有方法的代码片段，方法区中的常量不可以修改，静态变量可以修改

对于一般类，尽量不要定义成单例类，除非项目有特殊需求，或者该类为，重量级对象，创建该对象需要好用较多的系统资源

无论类是否为单例类，尽量不要使用静态变量

若需要定义为单例类，则单例类中尽量不创建成员变量

若单例类中必须使用成员变量

则对该成员变量的的操作，添加串行化锁synchornized线程锁，实现线程同步，添加了线程锁的操作会大大减小执行效率

API  application programming interface 

应用程序变成接口  就是一套提供了对外接口的类库