# 设置属性时虚拟机参数

```bash
eclipse
run as -> run conf -> arguments -> VM arguments 
-XX:+HeapDumpOnOutMemoryError	//堆存储快照
-Xms20m	//堆内存大小
-Xmx20m	//初始化应用程序使用的内存大小
-XX:+HeapDumpOnOutOfMemoryError -Xms20m -Xmx20m
//项目启动后就在项目内多出了java_pid8748.hprof文件
```

# 内存分析工具

```#java
Memory Analyzer
打开记录文件
```

## 按钮

```java
第三个	//查看内存信息树
Shallow Heap	//当前对象的大小
Retained Heap	//当前对象和引用对象大小的综合，点开可以查看内存占用

```

# 可视化建施工局

```java
/bin	jconsole.exe
```

# 内存详解

## 新生代

```java
Eden 垃圾回收器最常扫描地方，新创建的对象会放在这里，如果检查为可清理对象，会直接进行清除，如果存活，则放入Survivor内存区，如果多次存活则可能将对象放置到持久带中

Survivor
```

