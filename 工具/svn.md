#介绍

是apache下开源的版本控制软件

实现资源文件的共享，实现资源版本的控制

将文件存放在中心版本库里，可以记录每一次文件和目录的修改情况，例如，谁修改的什么时间修改的。每一次修改的内容是什么，也就可以回复到对应的版本

# 命令行

```xml
svnadmin --version
svnadmin create		//该命令用于创建svn根仓库
svnadmin create E:\svn\repository\sms
svnserve -d		//用于开启dos下的svn服务，开启svn顶层仓库，开启后客户端就可以对该仓库进行访问
netstat -a		//可以查看当前网络的使用状态
svnserve -d --listen-port=8888 //更换端口
svnserve -d -r E:/svn/repository
sc create SVNServer binpath= "C:/Program Files/VisualSVN Server/bin/svnserve.exe --service -r E:/svn/repository" start=auto depend=Tcpip
sc server create SVNServer 
```



![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/853cf46359da4ece9eec966325d02d08/clipboard.png)



设置svn的默认顶层仓库的名字

将svn注册为开机自启动的windows服务

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/83bf5ae5748b4718b75a6784c8bff393/clipboard.png)



sc server create SVNServer 自启动项目的名称  depend依赖的协议

创建系统服务，该命令需要在管理员下的命令行窗口中运行

且binpath后面必须有一个空格。windows10无问题，但其他的系统不可以必须有空格

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/c784dd2bc5414496a5146e4da9480319/clipboard.png)

net start SVNServer

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/bea71f20a6cb4c56ad5a005159fcfc91/clipboard.png)

net stop SVNServer

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/111e1475e8614d319421e4af0253db67/clipboard.png)

sc delete SVNServer

删除之前要先停止SVNServer服务

SVN客户端命令

svn cheakout检出，创建客户端指定目录与服务器端指定仓库的连接，一个客户端一般情况下，只需要检出一次

基于顶层仓库的checkout

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/792fc4b637f748c9b9499e258c5808bb/clipboard.png)

svn checkout svn://localhost/sms E:/svn/group/aacof

aacof下会多出一个.svn文件夹。不可以删除aacof就是你与服务器端连接的本地仓库

若当前执行命令的目录为客户端指定的目录，学名working copy

，我们称为客户端连接目录，则会将服务器端根目录添加到这里

如果创建的localhost是直到根目录的，则在我们本地的仓库文件夹内直接进行

svn checkout svn://localhost/sms           即可

服务端修改客户端权限

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/b3e074f1c3114069ace05886d5f2d562/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/868c8927b5834d7d83dfea4eca535f57/clipboard.png)

这样所有用户则都可以进行读写操作

每个资源仓库下都有一个svnserve.conf文件，都需要进行配置

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/ff24e56f67d6403a872f2843d2f3b379/clipboard.png)

在客户端(workingcopy)的该文件不在svn管理范围之内，svn并不会对其管理，若要svn对其进行管理必须通过add命令添加到svn管理，被add的文件或者目录，必须存在于workingcopy中。

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/c709e38e425142feb4268aaa48d10e5a/clipboard.png)

svn add E:/svn/group/aacof/测试文件.txt

如果先进入了文件前面的文件夹，则直接添加文件即可，不用写目录，add命令的作用就是将指定文件或者目录交由svn进行管理，所以一个文件或者目录一般情况下就只能执行一次add命令即可，add命令的执行与文件是否被修改过无关

svn commit

commit命令用于将客户端working copy中所有对文件/目录的操作提交到服务端

commit必须携带参数 -m 用于完成日志记录

提交后的文件在服务端是无法直接看到的

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/4a1026528032480085aa9e129756e754/clipboard.png)

svn commit Student.txt -m writed-by-a

如果没写要提交的目标则会将全部的文件进行提交

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/7d94ca58a26a4fb7b365da70620c76c9/clipboard.png)

svn checkout svn://localhost/sms

顶层仓库路径已经到根目录，但是不能直接检出，不明白

然后在别的客户端的库中 svn checkout 即可对文件进行检出

如果没有做任何修改第二次提交则无效（根据的是文件最后的修改时间）

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/10980e9a34694f04841de241c9963569/clipboard.png)

svn update Student.txt

可以直接对该文件进行更新。在别人进行提交后就可以更新。

不写对象则全部更新

文件不能提交是因为没有进行add添加到svn管理里面

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/75791500df4c448f993fccdfd5f6b6e7/clipboard.png)

删除的是本地的文件，对服务器无影响，只有我们commit后才会同步到服务器端

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/c4a2fc4e6df641fc82477564645d6dd3/clipboard.png)

svn revert Student.txt

用于恢复删除后并没有被commit的文件，一旦在客户端进行了删除则不能revert恢复

svn list

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/f69a6d475fad4eabbb7c9f12c38dc18d/clipboard.png)

列出当前working copy中全部的文件或者目录

svn info

列出有关当前working copy服务端与客户端的相关信息

TortoiseSVN

下载与安装

需要下载两个安装程序，包含svn客户端程序与svn语言包程序

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/b688b18340654a458b53fb76783544e8/clipboard.png)

先安客户端，在安语言包，安装过程一直next即可

windows支持Overlay icon(覆盖图标)最多15个，本身应用已经将这15个图标占用，所用svn安装成功后图标并没有变化，可以通过修改注册表进行修改

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/5624bb06c50a48078b5d076db1b57512/clipboard.png)

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/eb2f92f745ae4d7bbf0743a7c4f67223/clipboard.png)

360第一个。因为前面都是空格。

将360的前面的空格去掉，重启

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/db412e8693664f3aa298b6c6cec123e3/clipboard.png)

图标就会被更新

你所要连接的文件夹右键checkout 地址写入svn://localhost即可连接对应仓库，对要提交的文件右键add，完后进行commit，其他的客户端，update就会更新

如果在一个文件夹选中后对其提交，只会将它里面的文件提交到服务端，而不会提交它本身，如果对空白提交，则会把当前位置的全部文件进行提交，对不是working copy内的文件只能通过torto -> import的方式进行添加该文件

对客户端内的文件也可以选择导出，

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/e0131d8b429f4b75a3131cb3805d78e7/clipboard.png)

第一个会将该svn内不归svn管理的文件一块导出

第二个忽略该工程引用的外部工程

导入，导出都是将点击的文件的子文件操作而不会对文件本身进行操作

删除某个文件最好使用的软件自带的delete命令进行删除

删除后没提交之前可以使用svn revert进行文件的恢复 

uodate to reversion 可以选择到指定的版本

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/41213d84e2a44854bac812054deebb21/clipboard.png)

通过show log就可以看到各个版本间的变化，选择对应的版本

当一个文件在多个客户端都被修改了，第一个客户端进行他提交时，会提交成功，但第二个提交时会不能提交，冲突问题

根据冲突引发的具体原因可以将冲突分为两类

1.异行修改冲突

多个客户端对同一版本做出的修改，可以使任意的添加，修改，删除操作，只要他们修改的不是同一行数据，则他们的修改都会成功，对修改进行合并

2.同行修改冲突

只要多个客户端对同一版本文件的同一行数据进行了。添加修改删除，则称为同行修改冲突，由于不同客户端对于同一版本文件的修改内容是不同的，svn无法对各个客户端的修改进行取舍，无法自动给出冲突解决方案，此时只能有人工进行冲突内容的选择，即人工进行取舍

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/1a1fa84024a14ae0bc78bd31ae471b8f/clipboard.png)

当提交已经被修改了的文件时，

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/6594625475374368ad16b2fbf3c15ec0/clipboard.png)

第一个文件内的内容会变成该样式，.mine为我自己要修改的内容，.r5为5版本的内容，即未和别人发生冲突的版本的内容，.r6是别人已经修改后的文件，自己修改好修改的文件后，删除掉其余多余的文件，然后对修改好的文件再次进行提交即可，如果没删除掉多余的文件则还是不可以进行提交的

也可以通过自带的Edit Conflicts工具进行修改

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/0314ce50272a4073b4f635d469762093/clipboard.png)

在底下可以进行代码的修改，右键也有选项，如保留自己的代码，保留他人的代码，将自己和他人的一起保存，顺序不同等，可以选择几行用自己的几行用别人的也可以

异行冲突，按提示进行下一步更新working copy即可

混合冲突的处理

如何为用户添加权限

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/06a0cc4fa0664dabb516b7e2c8db4963/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/293803a302554683ac2885811ade8137/clipboard.png)

将上免得进行注释，将下面的注释打开，让授权用户具有读写权限，anon a no name 匿名用户权限无

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/f32e1d610c684189b4eb4d1e4f0c7ceb/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/377991f73b6745789084c95c74bde530/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/f7274d5ce3de437ba52b8a1eb121395e/clipboard.png)

为具体的用户配置权限文件，上面三步是注册密码配置文件，配置权限配置文件，指定要用的根仓库

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/8da3ade3ce774f3e8620b556754423e7/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/a2e9a673bcff468f8dc5631dd61a091c/clipboard.png)

这样aa,bb,cc,dd,ee就是我们注册的用户密码

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/54d9c1f391ec40c08399ba680ae75f79/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/f31e9f3032e84179abf153a9c8b60720/clipboard.png)

对于根仓库[/]，abc可读可写，de可读，其他用户不具有权限* = 

权限的取值只有这三个

[/com/bjpowernode/beans] 这是指定的目录，则可以对某个文件夹去进行限制

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/3aa9310c796b4fbc823b603ffea6ac77/clipboard.png)

可以队成员进行分组

对文件进行上锁，工具中有一个properties，

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/e2eb566eda374e2cba7f6e253964d120/clipboard.png)

New 点开有一个 needs lock选项，选择后则该文件被锁，想要操作该文件则必须先对其获取锁，一个人获取锁之后如果未对文件进行commit则其他成员无法对该文件进行修改，删除则选中对应的锁点击remove即可，不方便很少用

Eclipse中如何使用svn

svn插件下载与安装

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/bf17484e1e5a464e9aed431e1ff8109c/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/4b683449c656424bab686efb2e87424d/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/5b7edfb217654dd397e0a29690a73738/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/62856dcb03c94946a2421be2c56c349d/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/a474cc99d1e344fa887cecd250471cb9/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/f89474e731e141f2bbe7bd345a941365/clipboard.png)

将下载好的插件内解压出来的

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/dcac6baa78eb4146b863bc4195691821/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/deb280dba7d34b6180ef3b95da1e95be/clipboard.png)

内的全部文件复制到eclipse中对应的文件夹中，然后重启eclipse

到这里是将subversion插件安装完毕，如果安装成功，则右键项目->team->shareProject则可以看见svn的选项

还需要安装subversion的connector连接器

help->选择安装软件的选项

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/c45435c3c6e1453ab18b3c57054951cc/clipboard.png)

选择该项

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/52759f1733b346a7917f21e88a285020/clipboard.png)

next进行安装

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/46cf7ab194744775b61cd3ec98005a47/clipboard.png)

next ,next,i accept ,finish,等待右下角进行安装完毕

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/19c6c9e63158461380e0d937127f9fc1/clipboard.png)

填写url和管理人的账户密码

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/7ce3318cf0c3418abfe5d2568b80ca03/clipboard.png)

通过

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/e4e9845eda254ccaa1f7d2701db96687/clipboard.png)

可以查看根仓库内容

检出

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/e52120418bfb47c081b2b26f4966f317/clipboard.png)

打开该视图，连接仓库，输入自己的账号密码，然后在对应的仓库上checkout，切换回正常视图则可以看到项目

team工具栏中的各项功能

update,commit,revert,add to version control,

show history 查看该文件的全部历史，show local history

查看自己修改的历史记录

图标?表示该文件还没有被svn锁管理

同行冲突解决，提交不成功，更新，然后mark as merged标记进行合并过，然后再次进行提交即可，加锁文件只有加锁的用户可以修改，加锁操作会自动提交到svn服务器端，可以通过 locks scan对文件锁进行扫描

svn协议仅可以在局域网内使用，如果需要进行外网的访问，则需要进行服务器的配置