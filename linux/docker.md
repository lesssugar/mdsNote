# 地址

http://docker-cn.com/

centos 7版本的使用可以参照文档

# 概念

- 镜像 镜像就相当于java中的类，为只读模板
- 容器 独立运行的一个或一组应用，就是通过镜像所生成的实例，可以进行启动停止删除的操作。每个都是相互隔离的安全环境
- 仓库是集中存放镜像的地方

# 安装

```bash
yum install -y epel-release		//docker的依赖库
```

# 配置文件

```bash
vi /etc/sysconfig/docker	//配置文件地址
```

# 使用

```bash
service docker start
docker run hello-world	//会先运行本地的镜像，如果没有会自动从仓库中拉取
docker info //详细信息
docker --help
```

## 阿里云镜像

```bash
https://dev.aliyun.com/search.html
6.8
vim /etc/sysconfig/docker
other_args="--registry-mirror=https://abs6m4jv.mirror.aliyuncs.com"

7.0++
vim /etc/docker/daemon.json
{
  "registry-mirrors": ["https://abs6m4jv.mirror.aliyuncs.com"]
}
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# 镜像的命令

```bash
docker images	//显示所有的镜像信息
				-a	//镜像是多层的。显示所有层
				-q	//只显示当前镜像的id	//-aq可以查询出该镜像的全部id，可以批量进行删除
				--digests	//镜像的摘要信息
				--no-trunc	//显示id的完整信息
docker search	//按名称查询需要的镜像名字，类似npm search
				-s 30	//把点赞数超过30的镜像查询出来
docker pull tomcat
docker rmi hello-world
docker rmi ${docker images -qa}	//删除某个镜像的全部镜像
docker commit //提交容器副本成为一个新的镜像
docker commit -a="wyq" -m="del docs tomcat" bac73cf399c4  wyq/tomcat:0.1	//将正在运行的镜像打包成一个新的镜像
```

# 容器的操作

```bash
docker run [] IMAGE 镜像名 []
						-i	//以交互的形式启动容器，通常it一起使用
						-t	//重新分配一个伪输入终端
						--name	//给容器分配的名称
						-d		//不进行交互，以后台守护进程的方式运行容器，dockee ps查询不到，没有前台进行docker就自动对齐进行了关闭
//可以通过shell编程的方式。没两秒进行一次操作，后台就可以查询到持续运行的容器
docker run -d centos 	/bin.sh -c "while true;do echo xxx;sleep 2;done"
docker ps	 []	//查看那些容器正在运行
			-a	//查看现在运行的和历史运行过的
			-l	//查看上次运行的容器
			-n 3	//最近三次使用过的容器	
			-q	//只显示容器编号，批量删除容器
exit	//退出，会关闭容器并退出
ctrl + p + q	//
docker start 容器id	//重新回到
docker restart id	//讲一个运行中的容器进行重启
docker stop id		//慢慢重启
docker kill id		//马上关闭
docker rm 	id		//删除一个已经停止的容器
docker rm -f $(docker ps -aq)	或者	docker ps -aq|args docker rm
docker top	id	//查看某个docker中的进程
docker inspect id	//以json的形式描述整个docker
docker attach  id	//重新进入某个docker容器
docker exec -t id ls /l /tmp	//在宿主机也就是容器外部对某个docker容器进行操作
docker cp id 文件路径	主机路径	//复制docker的文件到主机中
```

## 例子

```bash
docker run -it centos //进入了一个新的centos系统
docker logs [] 容器id	//显示一个容器的日志
			-t //时间戳
			-f //不停的追加
			--tail	//最后几条
docker run -it -p 8888:8080 tomcat //主机外部端口，对应内部8080端口
			  -P tomcat		//端口号随机分配，可以通过docker ps查看
docker exec -it bac73cf399c4 /bin/bash	//启动tomcat后进入容器的路径
```

# 镜像

UnionFS	联合文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂在到同一个虚拟文件系统下，特性，一次可以加载多个文件系统，但在外层只能看到一个文件系统，比如一个tomcat，最内层kernel,centos,java,tomcat

## bootfs

主要包含bootloader和kernel,bootloader主要作用是加载kernel,Linux刚加载的时候会加载bootfs文件系统，在Docker的最底层是bootfs，当boot加载整个内核到内存中后，将内存的所有权交给内核，此时卸载bootfs

## rootfs

在bootfs之上就是rootfs，包含典型Linux系统中的/dev，/proc,/bin，/etc等标准文件和目录，rootfs就是各种	不同的操作系统发行版，如Ubantu,centos,精简的OS,rootfs可以很小，只需要包含基本的命令，工具程序库即可，因为底层直接用Host的kernel,自己只需要提供rootfs即可，由此可见对于不同的系统，bootfs基本一致，rootfs千差万别，因此不同的系统可以共用一个bootfs