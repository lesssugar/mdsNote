B/S架构原理剖析

接口的意义在于提前指定各个组件的规格，是各个组件可以达到很好的无缝对接的效果，可以使系统具有更高的可插拔性，使系统的松耦合性提高，具备更高的扩展能力

不同版本的浏览器都可以访问一个网页，是因为他们都是遵守W3C的HTTP规范的

我们发出请求是像服务器(容器)发出了请求，请求帮我们找到该资源，服务器找到后将资源返回给客户端

服务器与应用程序之间通过servlet接口规范进行解耦合

参与的对象共有 浏览器，服务器，应用程序，数据库分别拥有

HTTP协议，Servlet规范，JDBC接口分别实现解耦合

HTTP协议是计算机A像计算机B发送数据时提前规定好的一种数据格式，超文本传输协议，不仅仅能传输文本，还能传输流媒体

Server.xml

服务，现在只有一个服务叫Catalina

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/27f6b8b672ff441b99cc1c3854d8b194/clipboard.png)

如果想要使用该服务，则需要通过连接器 Connector 进行连接

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/9f55cc5c2bed4349ad1d2a4f77be1460/clipboard.png)

protocol 协议

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/79f5cf39d4674d56880f99a7b2ba9328/clipboard.png)

其他浏览器的连接器，协议为AJP/1.3

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/59e708a8d329471084454ddb269b8738/clipboard.png)

引擎，内部有安全管理 Realm，默认引擎localhost,如果有多个主机则访问localhost

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/aca46b1a8fda4336b856acdd94f5f2b8/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/5ad881b0ac024aa5bdf60a17719a0e19/clipboard.png)

主机 war包打包地址，是否会自动解压war包，是否自动发布

war包放置到webapp文件夹会自动解压自动发布

引擎提供服务，服务提供多个连接器，每个连接器对应一个浏览器，多个主机调用一个引擎，相当于一台阿里服务器上有多个企业的域名

如果想要更换web项目存储的位置在其下添加一个标签

<context path="\xxx" local="d:\course\mywebapp">

就可以将d盘下文件夹改为webapp的webapp   path url中的路径

不过该种方式需要重启服务器进行实现，一个服务器同时会运行多个公司的项目，重启不可能实现

可以在catalina引擎下

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/201d7c3524404a4c9c14157c0c1c365c/clipboard.png)

新建一个ooo.xml文件

<?xml version="1.0" ?>

<Context docBase="d:/course/myweb2">

path就是ooo 如果访问ooo就可以访问该项目

一个引擎中可以包含多个虚拟主机

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/265e62fe0b3d47468c58cf299bcc0ab4/clipboard.png)

这样就可以创建一个虚拟主机

如果通过 www.myweb.com 代替 localhost作为域名访问项目，但是还不能访问，域名解析DNS是现在本地域名解析器解析，找不到到供应商，再到全球进行解析

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/1026e58913644309a3c3a9c41301ad92/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/2295950ba2154d0abe7f455fb5324bdc/clipboard.png)

该文件不能直接进行修改

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/b2f5935645174e06beef743f45fd0e6c/clipboard.png)

将文件复制到别的地方，添加一个自己的域名解析器，然后复制回去进行覆盖即可

catalina中引擎中的文件夹就代表一个虚拟主机

浏览器的默认端口号是80端口，所以如果不想写:8080则需要将连接器中的端口号改为8080

tomcat的默认应用是ROOT 如果将自己的项目名字改为ROOT将会成为默认应用

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/f79e1d06a414497284c3c6495c784b92/clipboard.png)

修改该文件

<role rolename="manager-gui"/>

<user username="1" password="1" roles="manager-gui"/>

添加一个

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/7b30a12e2a7a41158546888cde363228/concrete-bindings.png)

账户密码为1的用户就可以使用tomcat的项目管理