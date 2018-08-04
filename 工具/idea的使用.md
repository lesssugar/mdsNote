#  模板的修改

<https://blog.csdn.net/u011961421/article/details/78773665>

#基础配置

-  在File->Settings->Editor->File Types下的”Ignore files and folders”一栏添加 *.idea;*.iml;添加不显示的文件格式
-  安装时的 .java .grovy .kt不点，功能是当打开这三种文件的时候，用idea打开
-  General 第二个选项可以用鼠标更改字体大小
-  General -> appearance 第五项 可以在方法和方法之间添加下划线
-  更改编码 Editor -> File Encodings project encoding 及properties files及右侧的选项框选上
-  Editor -> Code Style -> java. Blank Lines 前三项都改成1，当格式化代码时如果有多个空行会被合并成为一个空行
-  General -> Code Completion 第一个下拉框改成none，这样代码提示会忽略你写的大小写
-  General -> auto import  当从一个文件复制代码到另一个文件时，idea不会自动导包，而是提示是否要导包 第一个下拉框改成 all ，然后把接下来竖着排列的两个复选框选上
-  General 下 other 倒数第三个鼠标放在一个类上会给出类信息的展示
-  Preferences -> Inspections -> Serialization issues -> Serialization class without 'serialVersionUID'  自动提示添加序列化ID
-  alt + v 去掉 Navigation bar 文件导航栏，想看的时候alt + home

##插件的添加

- Plugins 最下面共有三种添加方式 左到右 idea官方插件 第三方插件 本地插件

###jdk的配置

- configure -> project defaults -> project structure 

- Appearance & Behavior System Settings 第一项取消勾选，则不会进入上次退出的项目

- view 中打开 toolbar 和 toolbar buttons

- idea jbm参数

- help ->  Edit Custom VM Options 

前三个改成 1024 2048 500

- BUG模式 F5 F6 F7  进入方法内部 不进入方法内部 执行到下一个断点

- ![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/69a9616ba0a84acca8eb23d04343e3f9/clipboard.png)

- maven菜单项

mavenProject菜单第一个刷新，第三个下载源码，第5个可以填写要执行的生命周期，倒数第三个显示项目的依赖关系图

- 集成svn

vcs -> 倒数第三个 -> 第四个 指定远程服务器的地址，点绿色加号，添加远程地址

上传文件到远程仓库有些文件不应该被上传，例如 .iml 文件 及idea文件夹、

- settings -> version controller -> ignored Files 

点击右侧绿色按钮添加，添加第一项为选中文件，第二个为选中文件夹

连接成功后

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/53f7d4e9364844fda2a5a04a14995fc4/clipboard.png)

- idea连接数据库，如果需要做性能优化还是用客户端

##插件
- maven helper

- GSONFormater 可以将json转化为java类 快捷键 alt+S

- markdown support可以编写markdown文本 .md

- indBugs  <https://blog.csdn.net/feibendexiaoma/article/details/72821781>


ECTranslation

CheckStyle-IDEA

<https://blog.csdn.net/l1ub0w3n/article/details/74920842> 规范详解

快捷键的使用

shift + shift 全局搜索  alt + ↓ 可以在浏览代码的时候在方法之间跳跃。ctrl + w选中，再摁扩大范围选中	ctrl + shift +w不断缩小范围选中	

int a = 10; a.sout = System.out.println(a)		

a.for = 

 (int i = 0;i<a;a++){

}

a > 10 .if 	

if(a>10){

}

变量或对象.var等于eclipse的直接生成对象

f2快速定位错误	alt + enter快速解决问题

对象.null = if(对象 == null){}

thr = throw new

快捷键生成构造器，get,set	alt insert 	选择对应的 shift进行选中

ctrl + j可以查看本处可用的代码模板

手动输入新建一个文件 	ctrl + alt + insert

ctrl + shift + f12 最大化编辑框

crtl + f12 展示一个类的结构信息

ctrl + f1 展示代码的错误信息

移动过程中 +ctrl 一次移动一个单词

拉住光标alt可以选中多行编辑

ctrl + shift + ↑ 或下将一行位移

shift + f9 debug模式运行	shift + f10以运行模式运行

ctrl + f5 重新运行 ctrl + f2 中断运行

Alt + f9可以运行到光标指向处 	想要打印运算时的变量，用alt+f8	f6可以移动方法或者类的位置 		修改类的名称用shift + f6	格式化可以格式化一个包中的全部文件 alt + shift + l

ctrl + f 查询	f3 下一个，shift + f3前一个位置，

ctrl + r进行替换	进入一个类 f4	ctrl + h查看一个类的继承关系
#配置文件地址
可以通过导出配置然后在别的电脑进行使用