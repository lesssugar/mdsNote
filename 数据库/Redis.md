# 需求

数据库在数据量很大的时候，内存消耗多，建立大量索引，性能急剧下降

方法:

- 缓存	+	MySql 	+	垂直拆分(例如买家，卖家)
- MySql主从，父数据库写一条，子数据库也写入一条
- 读写分离(读写用不同的数据库)
- 分库分表，水平拆分，MySql集群，将紧耦合的访问程度相差不多的放在一个库
- MyISAM使用的是表级锁，一个访问的情况下其他的不允许访问，高并发下效率低 

# NOSQL

not only sql,这些数据类型的数据不需要固定的模式，无需多余操作就可以横向扩展·

## 特点

- 易于扩展
- 键值对
- 缓存
- 持久化

## 大数据时代的3V和3高

3V

- 海量Volume
- 多样Variety
- 实时Velocity

3高

- 高并发
- 高可扩
- 高性能

## 当下的应用时sql和nosql一起使用

我们主要处理的问题是多数据源，多数据类型的存储问题

- 商品基本信息，名称价格，出厂日期，生产厂商等趋冷的数据

关系型数据库，mySql/oracle

- 商品描述，详情，评价信息(多文字类)MongoDB
- 商品图片展现类，（分布式的文件系统中，Hadoop的HDFS，谷歌的GFS，淘宝的TFS）
- 商品的关键字(ISearch,淘宝内用)
- 商品的波段性的热点高频词汇:玫瑰，巧克力（放在Redis等缓存中）
- 商品的交易，价格计算，积分累计(外部接口，支付宝等)

### 难点

- 数据类型多种多样
- 数据源多样性和变化重构
- 数据库改造而数据服务平台不需要大面积重构

### 解决方法

统一数据平台服务层UDSL

- 映射
- API
- 热点缓存

## 去IOE化

ioe就是在It建设中，去除IBM小型机、Oracle数据库及EMC存储设备



# 注意点
- 高并发的操作不建议有关联查询
- 互联网公司可以用冗余数据来避免关联查询
- 分布式是不能支撑太多的并发的

# 数据表现形式
- bson 
- KV键值对  Tokyo,BDB,Redis..
- 列族  以竖的方式表现表，成为KV对的形式.存储文档型数据，mongoDb，CouchDb
- 图形  neo4j.InfoGrid

# CAP + BASE
NOSQL 中叫CAP
consistency 强一致性
Avaiabilty 可用性
Partition tolerance 分区容错性
三选二，不可能三者同时满足
- CA 单点集群，满足一致性，可重用性的系统，通常在可扩展性上不太强
- CP 满足一致性，可重用性的系统，通常在可扩展性上不强。
- AP 满足可用性，分区容忍性的系统，通常可能对一致性要求低一点。



## BASE

降低数据库某一时刻的一致性要求来换取系统整体衣服索性和性能上的改变。



# Redis的优点

- 支撑持久化
- 支持更多的数据类型
- 支持数据的备份，即master-slave模式的数据备份

# 安装

- 下载
- tar  -zxvf解压
- make 

## gcc未找到

```java
make安装的时候有可能没有gcc环境
gcc是linux下的一个编译程序，是c程序的编译程序
yum install gcc
然后	make distclean
然后再次make安装成功
```

make install

# 使用

- 使用前先将redis的配置文件 redis.conf 备份到一个文件夹中
- redis配置文件

```java
redis-server /redis/redis.conf	//启动服务
ps -ef|grep redis		//查看服务
redis-cli	//启动服务
      -p //端口号
SHUTDOWN	//关闭，然后exit退出
DBSIZE //查看存储的键值对总数
keys * //查看全部key
```

## 多个数据库

```java
redis一个库共包含16个数据库，通过select 0-15进行切换
0号库中set了一个数据,其他库中没有，可以对不同的库分片
```

# 增删改查key

```java
set	x	y
setnx x y //如果x不存在则添加
get x
move key index	//将某个key移动到某个库
ttl key	//查询某个key剩余过期时间,-1永不过期
expire key seconds //设置某个key的过期时间为多少s
setex k1 10s v4 //创建的同时定义key的生命周期
FLUSHDB	//清除一个库中的全部数据
FLUSHALL //清除16个库的全部数据
del key //删除某个key
mset k1 1 k2 2 k3 3	//同时设置多个值
mget k1 k2 k3	//同时获取多个值
msetnx k1 1 k2 2 k3 3 k4 4 //对多个值判断判断，如果有一个没有则全部都不存
```

# String

```java
append k1 //连接字符串
length k1 //长度
incr k2 //每次自增1
decr k2 //每次自减1
incrby k2 3 //增加3
decrby k2 3 //递减3
getrange k3 0 -1 //subString(); -1取全部
setrange k3 0 xxx //重0开始，将前三位更改为xxx
```

# List

```java
LPUSH list 1 2 3 4 5
LRANGE list 0 -1	//取元素，-1全部
RPUSH 1 2 3 4 5
LRANGE 0 -1 	//右侧插入，顺就和自己插入的顺序一样
LPOP list 		//左侧出栈
RPOP list		//右出栈
LINDEX list 2	//2号为元素
LREM list 2 3	//在所有元素中删除两个3
LTRIM list 1 3	//保留下标1到3的元素
RPOPLPUSH list1 list2 	//将list1的 尾部接到list2的头部
LINSERT list before x java	//在x前面插入java
```

# Set

元素不允许重复

```java
SADD set v1
SREM set xx	//删除元素
SRANDMEMBER	set x	//从集合中随机获取x个元素
SMOVE set1 set2 5	//将set1中的5元素移到set2中
SDIFF set1 set2		//查询第一个有第二个没有的
SUNION set1 set2	//查询两个集合的并集
```

# Hash

类似于map

```java
HSET user id 11
HGET user id
HMSERT user id 11 name wang
HMGET user id name
HMGETALL user
HDEL user name
HLEN user
HEXISTS user id
HKEYS user
HVALS user
HSETNX user email xxx@qq.com	//没有的键添加
```

# 有序set

```java
ZADD set 30 v1 30 v2 40 v3
ZRANGE set 0 -1 withscore
```

# 配置文件

## include

可以包含其他redis的配置文件

## network

```java
tcp-backlog 511
timeout 0
tcp-keepalive 300	//建议设置成60，集群之间发送ping进行检测网络连接是否良好
```

## GENERAL

```java
loglevel	//开发环境可以使用debug,生产环境可以使用notice或者warning，默认级别verbose
logfile下三项
# syslog-enabled no		//是否记录系统日志
# syslog-ident redis	//日志实体文件 默认叫redis
# syslog-facility local0	//日志的用户默认0
```

## SECURITY

```java
config get requirepass
config set requirepass "123456"
auth 123456
密码设置为	""	就是没密码
```

## MEMORY MANAGEMENT或LIMITS

```java
The default is:
#
# maxmemory-policy noeviction	//默认永不过期
//LRU算法，对使用少的删除，这个是对过期的键删除
 volatile-lru -> Evict using approximated LRU among the keys with an expire set.
//对所有的key进行LRU算法
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU among the keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
//随机删除过期键
# volatile-random -> Remove a random key among the ones with an expire set.
//随机移除键
# allkeys-random -> Remove a random key, any key.
//移除TTL值最小的key
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
//不移除信息
# noeviction -> Don't evict anything, just return an error on write operations
    
maxmemory-samples 5
```

## SNAPSHTTING

save 900 1

save 300 10

save  60 10000		//秒	修改次数，会在修改位置多出一个dump.rdb

```java
save	//非异步的产生备份文件
bgsave	//异步的产生备份文件
//恢复数仅需将rdb文件移动到目录下即可，最后一次的数据有可能备份不进去，备份的时候2倍的数据膨胀需要注意
```

## RDB(先加载AOF)

```java
dbfilename dump.rdb		//加载的时候默认加载这个文件，文件被删除后数据库也进行了提交
stop-writes-on-bgsave-error yes	//后台出了错误，前台停止写入
rdbcompression	//是否开启压缩，可以节约一定量的CPU不建议开启
```

## 不确定位置项

```java
maxclietns 128	//客户端最大连接数
```

# 持久化

## rdb（redis database）

一定时间段内就将内存中的数据写入到磁盘,redis会单独fork一个进程来进行持久化，会先将数据写入到一个临时文件中，待持久化的过程结束了，再用这个临时文件替换持久化好久的文件，如果需要进行大规模数据的恢复，而且对于恢复的完整性不是非常敏感，RDB方式要比AOF方式更加的高效，缺点是最后一次的数据可能会消失，保存的是dump.rdb文件

## FORK

复制一个与当前进程一样的进程，新进程的 所有数据和原进程一样，但是是一个新进程，而且是原进程的子进程

## aof

以日志的形式记录每个写操作，将redis执行过的所有写指令记录下来，读操作不记录，只许追加文件但不可以改写文件，redis启动之出会读取该文件重新构建数据，

以日志的形式记录全部的

## APPEND ONLY MODE

先加载AOF所以如果AOF文件错误，会拒绝连接

```java
appendonly no		//是否开启,默认不开启
appendfilename "appendonly.aof"		//默认文件名称，启动时默认按该文件进行数据库的恢复，FLUSHALL也会被写入
appendfsync everysec	//默认每秒记录，一秒钟宕机数据有丢失，
    		Always		//同步持久化每次发生改变数据变更立即记录到磁盘，性能差但数据完整性好
no-appendfsync-on-rewrite no	//对aof进行重写压缩，会在aof文件大小是上次重写的一倍且大小在64m以上
auto-aof-rewrite-min-size 64mb	
```

### 修复redis.aof和redis.dump

```java
redis-check-aof --fix appendonly.aof
//同理一样的另一个文件
```

# 事物

```java
MUTIL		//begin
EXEC		//commit
WATCH 		//监视在事务执行开启前有没有别的客户端对值进行了修改
watch	balance
UNWATCH		//取消所有key的监控
DISCARD		//rollback
```

# 主从复制，读写分离

```java
复制多份配置文件
修改项		
端口号
pid
logfile “6380.log”
dbfilename dump6380.rdb
```

```java
info replication	//查看多个数据库的信息
role	//角色，默认都是master
子数据库中	SLAVEOF	127.0.0.1 主机端口号，信息会全部进行复制
重点，子数据库不能写，只能读取数据
```

## 数据库宕机

```java
默认配置下，如果主机宕机，子数据库会保持原有状态，主机回复链接，回复原有状态
子数据库宕机后，连回来后需要重新SLAVEOF进行连接
```

## 子数据库可以作为数据源

```java
可以使用一个slave作为其他slave的数据源，这样的情况下，主机分配给几个，几个继续向下复制，可以减少主机压力
```

## 子数据库成为父数据库

```java
SLAVE no one
//主机连回来后依旧是master但是不连着子数据库，可以成为slave
```

# 复制原理

- slave启动后成功连接到master会发送一个sync
- master接到命令后启动后台的存盘进程,同时搜集所有的用于修改数据集的命令集，在后台进程执行完毕后，master将整个数据库文件发送到slave完成一次同步
- 首次连接到master全量复制master数据
- 以后同步数据采用增量同步的方式
- 只要重新连接到master，就会进行全量同步同步数据

# 哨兵模式

在后台监视主机是否故障，在故障后通过投票的方式选择一个从库成为主库

```java
//在redis下创建
touch sentinel.conf	//文件
vim sentinel.conf
sentinel monitor 自己起被监控的数据库的名称 127.0.0.1 6379 1
//1代表票数，谁的票数多余1票以上谁就成为新的主机
在bin文件夹下启动	redis-sentinel /envConfig/redis/centinel.conf
//原来master回来后成为slave
```

# Jedis

## 连接出现connection refuse

```java
chkconfig iptables off	//关闭防火墙
修改redis.conf配置文件，将端口号127.0.0.1注释掉
protected-mode yes 改为no
Jedis jedis = new Jedis("192.168.10.56",6379);
```



