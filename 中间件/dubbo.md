# RPC

两个不同服务器的服务进行交互，A服务器通过程序和Socket和别的程序进行交互，通过序列化发送数据，B服务反序列化获得参数，返回结果同样通过序列化返回，反序列化得到结果

# 注册中心

当有许多的服务，我们怎么知道哪些服务器上都有哪些服务，可以通过注册中心来注册服务

# 灰度发布

可以通过不同的路由规则，可以让不同的服务器暂时使用不同的服务版本，一步步更新到最新版本

# 角色

- 注册中心
- 服务提供者
- 服务消费者
- dubbo框架容器
- 监控中心，服务的被调用信息每隔1m会被发送到监控中心

# Zookeeper

```java
下载，解压
将配置文件复制一份，改名为zoo.cfg
dataDir=../data	,修改数据存储位置
get /	获取根节点下有没有值
ls	/	检查根节点下的节点
create -e /atguigu 123456	//创建一个值为123456的节点
```

# 运行

```bash
npm install
npm run dev		//运行前台
mvn clean install
mvn --projects dubbo-admin-backend spring-boot:run	//运行后台
```

# 使用

