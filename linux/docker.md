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
```



