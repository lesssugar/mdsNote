# 优点

运行稳定，对网络的良好支持，低成本，且可以根据需要进行软件裁剪，内核最小可以达到几百KB等特点

# 入门

## 介绍

免费开源的操作系统，安全高效，稳定，处理高并发强悍，所以企业级的项目都部署到linux和unix上运行

## 主要发行版本

CentOs		Redhat		Ubuntu		Suse		红旗Linux

不同厂商以linux内核为基础，根据需求进行应用程序的添加，生成了不同的发行版

# 安装

## vm

在内存中划分空间，在空间里安装系统，安装好的系统可以移植

##CentOs

##初始化设置

安装好后分配2G内存，双处理器，每个处理器；两核心，

网络适配器:虚拟机的网络连接的三种形式的说明

桥连接:Ip会和电脑的环境在同一网段，当电脑过多有可能会发生网关冲突

NAT模式:windows下会多出一个IP地址	192.168.100.xx不会占用初始的IP地址的冲突，这样的情况下windows无法找到linux，但是linux可以访问外网，不会造成IP冲突(推荐)

主机模式:此时linux是一个独立的主机，不能访问外网

# 使用

## 安装vmtools

将安装包放在/opt文件夹下，解压，安装即可，一路回车，安完重启，就可以将windows中的内容与windows中的互相复制交互

## 文件操作命令

```
tar -zxvf 文件名	//解压缩文件
./xxx.pl   安装pl文件	//安装.pl文件
touch xxx.xxx	//新建一个文件
```

# 文件目录类指令

``` java
pwd	//查看自己当前在什么目录
ls     	//详细展示全部文件及文件夹的信息
    -a	//显示所有文件，包括隐藏的
ll		//只显示各个文件的名称，也有	-a
mkdir xxx	//创建目录
    -p	//可以创建多级目录
rmdir  删除空目录
rm -rf 删除非空目录及文件
touch	xxx	//创建一个文件，也可以一次创建多个文件，空格隔开
cp	//拷贝指令
cp [] source dest	//可以用相对路径或绝对路径
	-r	//递归复制整个文件夹
	\	//cp前写斜杠，强制覆盖
mv source dest	//剪切文件
```

## 查看

```java
cat [] 文件 |more	//查看文件,|more分页显示
	-n //显示行号
more 文件名	//以页显示文件
	ctrl + f	//下一页	f页可以
	ctrl + b	//上一页	b也可以
	q			//退出
	enter		//下一行
less  //也是查看文件，不会吧整个文件统一进行加载，显示速度快
head 文件名	//默认查看文件前5行的内容
    -n 5 文件名	//查看5行
tail 文件名	//默认显示最后5行
    -n 5 文件名	//查看最后5行
    -f	文件名	//实时追踪文件的所有更新
ln -s 源文件或目录 //软连接的名字
```

## >输出重定向	和	>>追加

```java
会覆盖原文件的内容		不会覆盖原文件的内容
ls -l > a.txt		//将查询到的目录信息添加到a.txt中
ls -l >> a.txt		//将查询到的结果追加到后面
cat 文件1 >  文件2
cat xx >> yy
echo "内容" > xx
echo "内容" >> xx
cal > xx
cal >> xx //大致基本相同
```

# 搜索文件

```java
find [搜索范围]	[]
			  -name	//根据文件名称查找，	*.txt 通配符
			  -user //按用户来进行查找
			  -size +20m //查找大小大于20M的文件，-20小于20的，直接写20查找等于20m的，也可以查询k

locate //使用前需要updatedb,更新文件地址的数据库
		文件名
```

# 搜索内容

```java
grep [] 查找内容 源文件
	-n 显示匹配的行号
	-i 忽略字母大小写
cat xxx | grep ...
    将前面获得到到的内容通过|交给grep进行处理
```

# 压缩和解压缩

```java
gzip	文件名	//压缩文件，源文件会消失	.gz
gunzip	文件名	//解压缩文件,压缩包会消失
    
zip [-r] mypackage.zip /home/	//将home下的全部文件递归压缩成mypackage.zip
unzip [] 文件名	//解压缩文件

tar [] xxx.tar.gz 内容 //打包,tar.gz文件
tar -zcvf a.tar.gz a1.txt a2.txt 	//压缩文件，/home/
tar -zxvf a.tar.gz	//解压文件
					-C 解压到的目录
```

## 系统操作相关

```java
reboot	//重启系统
shutdown -h now //立即关机
		-h 1   //1分钟后关机
		-r now //立即重启
halt	//关机
sync	//将内存的数据同步的硬盘上，防止数据丢失，最好在重启关机前sync一下
setup	//进入设置
ifconfig	//查看本机ip
service iptables stop//关闭防火墙，start开启，status查看状态
cal 显示日历
echo $PATH	//查看系统的环境变量
history	[number]	//查看已经执行过的历史指令,显示多少个
！ xxx	//执行编号为xxx的指令
```

# 目录结构

只有一个/根目录

```
/bin	常用的指令
/dev	管理设备
/etc	存放配置文件
/home	新建用户，会有用户的文件
/lib	库文件	.so
/media	识别USB,DVD
/opt	安装包放置地点
/proc	内核	(尽量不要改动)
/root	root用户的文件
/sbin	高权限用户才可以使用的指令
/selinux安全工具
/srv	服务，服务启动之后要动的数据	(尽量不要改动)
/sys	系统	(尽量不要改动)
/tmp	临时
/user	用户安装的软件和程序（重要），安装了软件user/local下就会多出一个文件
/var	不断变化的文件，比如日志
/mnt	挂载，可以加载别的文件系统，例如和window共享文件夹
```

## VI和VIM

- 正常模式

可以使用快捷键

- 编辑模式

可以输入内容，按i进入

- 命令行模式

可以完成读取，存盘等操作

```java
:wq	//保存退出
:q!	//不保存直接退出
```

快捷键

```java
选择行，正常模式	yy+n复制出5行	p粘贴到
				 n+ yy复制当前和下面的共n行
				 5dd删除5行
				/hello	匹配hello字符，区分大小写		
				：set nu	显示行号
				:set nonu 隐藏行号
				G到末行，gg到首行
				u撤销操作
```

# 用户管理

## 概念

- 组，用户可以放在不同的组当中
- 用户家目录，在/home下有各个创建用户相对的家目录，当用户登录时，会自动的进入到自己的家目录。

## 操作用户

```java
//用户增删改密码
useradd	[]	用户名			//会在home下生成一个useradd的文件夹
	-d 目录，将文件夹放到指定目录	
passwd 用户名	回车，输入两次密码	//设定或者修改密码
useradd 名称	//添加一个用户
userdel -r 名称	//删除一个用户
```

#组的操作

```java
//增加组
groupadd 组名
groupdel 组名

//在创建用户时，将其添加到一个组
useradd -g 组名 用户名	//然后通过id查看用户就被分配到了对应组
//更换用户的组
usermod -g 组名 用户名
```

# 组的管理

## 文件或目录的三要素

```java
所有者	//一般指谁创建了文件，谁就是所有者
    ls -ahl //查看文件所有者
所在组
其他组
```

# 权限

```java
-rw--r--r---. 1 root root   1291 6月  18 05:03 anaconda-ks.cfg
开头 -普通文件
	d目录
	l连接文件，软连接
	c字符设备
	b块文件，鹰牌
rw	文件所有者的读写权限，不代表可以删除该文件，需要由该文件夹的写权限，rwx代表可执行
r-- 文件所在组的人拥有的权限
r-- 文件的其他组的用户拥有的权限
1	文件，代表硬链接
	如果是目录，代表该目录的子目录有多少个
root 用户
root 组
1291 文件大小字节
	 目录统一4096
18 05:03
     文件最后的修改时间
```

# 权限管理

```java
u 所有者	g 所有组	o 其他人	a 所有人(ugo的总和)
chmod u=rwx g=rx o=x	//给所有者rwx，所在组rx，其他人x权限
chmod o+w	//给其他人增加写的权限
chmod a-x	//所有用户都剪掉执行的权限
chmod u=rwx,g=rx,o=x HelloWorld.java
//也可以用数字来进行编写
r = 4	w = 2	x = 1
chmod 751 xxx	和上面的操作相同
chown newowner:newgroup file	//改变用户的所有者和所有组,也可以单写一个
	-r	//递归把整个目录一起进行改变
chgrp [] 组名 /home/abc.txt	//更换文件的组信息
	-R	
```

# 时间日期类

```java
date 	[]	//不加参数显示完整信息
	"+%Y"	显示年份
	"+%Y-%m-%d %H:%M:%S"	显示年月日，时分秒
	-s "2018-10-10 11:22:22"	//设置系统时间
cal 	//显示日历信息
	2020	//显示一整年的日历
```

## 信息的位置

```xml
/etc/passwd		//用户配置文件
/etc/group		//组配置文件
/etc/shadow		//口令配置文件(密码和登录信息，是加密的,密码存储在这里)
```

### passwd和group文件

```xml
xiaohong:x:503:503::/home/xiaohong/dog:/bin/bash
用户名 加密密码 用户id 组id 家目录 	shell

group文件
用户名:组名
```

#实用指令

## 指定运行级别

0. 关机
1. 单用户(找回丢失密码)
2. 多用户无网络(用的少)
3. 多用户有网络(用的最多)(程序员)
4. 保留(暂无)
5. 图形界面(普通用户)
6. 系统重启

在/etc/inittan下可以指定用户的运行级别

## 切换到运行级别

```xml
init [0,1,2,3,5,6]	4除外
vim /etc/inittab	查看，最底下写着默认级别为5号，图形界面
```

## 如何找回root密码

进入到单用户模式，然后修改root密码，进入到单用户模式root不需要密码

开机刚开始页面，Enter，输入e，到第二行，e，1，进入单用户级别，b，重新引导,passwd root即可

# 定时调度

```java
//定时调度我们的脚本，简单的任务直接写，复杂的写shell进行执行
1. crondtab -e	//进入编辑页面
2. */1 * * * * ls -l /etc/>/tmp/to.txt
3. 保存退出
4. 接下来每分钟会执行一次该代码

crontab -r 终止任务调度
	    -l 列出全部的任务调度
service crond restart 重启全部的任务调度
```

## 参数

```java
*/1 * 1,3,5 2-3 *
第一个1占位代表分钟，可以输入0-59
2	一天中的第几个小时	0-23
3	一月中第几天			1-31
4	一年中的第几个月	1-12
5	一周中的星期几		0-7(0,7都代表星期日)
*代表的是任意时间，及每小时的每分钟都出发该操作
,代表不连续的间隔操作，1,3,5号进行操作
-代表连续的，2-3月都进行该操作
*/10 开头就更改为了每10分钟进行一次操作
```

# 磁盘操作

## mbr分区

- 最多支持4个主分区
- 系统只能安装在主分区上、
- 扩展分区要占一个主分区
- MBR最大支持2TB但是有较好的兼容性

## gpt分区

- 支持无线多个主分区(但操作系统可能有限制，比如windows下最多128个分区)
- 最大支持18EBde容量(EB = 1024 PB , PB = 1024 TB)
- window7 64位以后支持gtp

## linux的分区原理

硬盘可以分成不同的分区，然后可以将不同的分区挂载到不同的路径下，例如/boot对应一个分区，所以linux中的一切都是文件夹

## 硬盘查看

```java
lsblk -f	listblock -f	//老师不离开
    就可以查看对不硬盘的不同分区和挂载情况
```

## 实现挂载

###分区

虚拟机设置添加自己所需要大小的硬盘,查看分区列表就会多出自己的分区，但是还没有挂载

```java
fdisk /dev/sdb
m
n	add a new partition
p
1
默认的回车
默认的回车
w
```

### 格式化

```java
mkfs -t ext-4 /dev/sdb1
```

### 创建文件夹

```java
创建一个文件夹	/home/newdisk
```

### 挂载

```javascript
mount /dev/sdb1 /home/newdisk
umount /dev/sdb1	//取消挂载
```

###设置永久挂载

```java
vim /etc/fstab
添加一行,对齐，这样的话重启后就会自动进行挂载操作
/dev/sdb1			/home/newdisk	ext4	defaults	0	0
```

##磁盘情况查询

```java
df -h	//查询全部磁盘的使用情况
du -h	//查询某个目录磁盘占用情况
du -ach --max-depth=1 /opt	//查询opt一级目录下
    -a //含文件
    -h //带计量单位
    -s //指定目录占用的大小汇总
    
```

# 环境配置

## java

```java
yum install java 		//安装java环境
```

##git

```java
yum install git			//安装git
 git config --global user.name "lesssugar"	//配置
 git config --global user.email "lesssugar@aliyun.com"
 ssh-keygen -t rsa -C "lesssugar@aliyun.com"	//配置秘钥
```

## maven

```java
官网复制	-bin.zip的文件包连接
本地	wget 复制的地址
unzip	解压缩文件包即可
```

