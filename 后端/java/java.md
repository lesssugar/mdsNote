#基础知识

Graphical User Interface GUI 图形化界面

Command Line Interface CLI 命令行方式

dir md cd rd cd.. cd/ del exit 

JDK1.8各大JVM厂商取消永久区，使用元空间，使用的是物理内存，即电脑内存多大，其可以使用多大，永久区为自己分配，如果满了垃圾回收机制会对其进行回收，使用物理内存使，此处被回收的概率变小

#注释

如果文档注释包含多段内容 用<p>标签分开

@author（只出现在类和接口的文档中） @version（只出现在类和接口的文档中） @param（只出现在方法或构造器的文档中） @return（只出现在方法中） @exception（从java1.2之后也可以使用@thrown替代） @see @since @serial（也可以使用@serialField或@serialData替代） @deprecated

#面向对象

继承：子类继承父类，可以得到父亲的全部属性和方法（除了父类的构造方法）。

接口和抽象类的区别？

抽象类一般用于实现基类又叫做超类，父类。 接口一般实现于业务。再大的项目就具有四个功能增删改查。接口更倾向于业务的实现。接口比抽象类更加抽象。

多态时方法处于重写到哪类就使用该类的属性。定义的同名属性重新定义。取值为方法所在类。可以super设置值。

没有在父类中写的方法。无法通过多态父类直接调用。需要重写或者强转调用

开发全是面向接口的编程 接口是对类实现的规范

接口里只有两个东西 常量和抽象方法 接口是最抽象的 可以抽象出许多类的共同点 可以更规范的对子类进行约束。全面地专业地实现了:规范和具体实现的分离。接口就是规范。定义的是一组规则，体现了现实世界中“如果你是..则必须能...”的思想

接口一样不能实例 只能实例接口的子类 需要使用子类变量需要强制转换

接口主要用于设计和实现分离

每次new的实质就是在堆内存中生成一个对象，将其引用地址发给引用的变量名

#String的常用方法

.delete(int a,int b) 删除对应

insert(int a,String b);

#异常

异常类型;Exception异常结构根类 ArithmeticException算数错误形式，如以0座除数

不要进行小粒度的异常处理，应该讲整个任务包装在一个try中

处理异常不可以代替简单的测试

异常往往在高层处理

java异常的理解

throwable类的常用方法

getMessagr() 获取异常信息

void printStackTrace()打印堆栈信息

void printStackTrace(PrintStream s)

#数组集合方法

System.arrayCopy(a,b,c,d,e);a；原数组 b；被复制起始位置 C：复制到数组 d:插入到哪个位置 e:复制的长度

Arrays.asList(a);将一个数组转换成list

Arrays.sort(a);数组排序

Arrays.binarySearch(a,int b);在a数组中寻找b 必须先经过排序才能使用

Syso（Arrays.toString()）;可以遍历输出数组元素

Arrays.fill(a,x,y,z);将数组a的x到y前一位的值替换为z

Map类 创建并储存键值对

.put("","")添加新的键值对

一个键多次put不会报错。是修改值得作用

.get("");通过key寻找value

.remove（）;

.keySet()显示全部键集

.values()显示全部值

.containsKey();检查集合中是否含有该键值对

.clear()清空键值对

.isEmpty检测是否为空

#多线程

什么是多线程:

系统给每个进程分配了独立的工作空间，内存是独立的；所以即使杀掉单个线程，程序还是可以执行的	

进程和进程之间是互不干扰的，线程与线程之间是公用一个工作空间，所以是会互相干扰的

线程是程序执行的一个最小单位；

并发并不是并行，并行能同时被多个CPU执行，是真正的同时；

.yield（）；调用此方法的线程放弃cpu的执行权；

.join();将调用线程添加进来优先执行完在回归执行其他线程

.isAlve()；当前线程是否还存活;

.sleep(long x);显式的让当前睡眠指定时间 睡眠状态下会有先执行其他线程

同步锁一定要放在共享循环的内侧，线程锁可以由任意的一个对象担当多个线程的所，继承的方式由于锁的对象有多个，所以无法使用syschronized进行线程安全的保证，对于一般方法可使用this当做同步锁，对于静态方法可以使用类.class当做锁，懒汉模式线程安全问题，可以在创建对象的判断外面加锁，为了效率提高，外面再加一层判断，如果对象为null则进入创建过程，不为null直接进行对象的获取

#IO

File("","");前面填写父路径，可以直接用别的路径对象，后面填写要使用的文件；

File(""); 添加路径，可以代表一个目录或者文件

f.exists();检测路径是否是一个可使用的文件或者路径

f.isFile();是否是一个正常文件

f.isDirectory();是否是一个正常目录

f.canRead();是否可读

f.canWrite();是否可写

f.getPath();获取文件相对路径名

f.getAbsolutePath();获取文件的绝对路径名

f.getName();获取文件或路径的名称

f.delete();删除文件

f.lenth();获取文件长度 如果填写的是文件夹路径，或者文件不存在，则返回0L；

f.isHidden();是否隐藏

f.cteateNewFile（）;创建一个新的文件，或者路径；

f.mkdirs();创建多层目录，不能创建文件

流就是一组流动的字符，以先进先出的方式接受或者发送数据的通道

输出输入流主要包括类

当向一个文件写入数据时，这个流被成为输出流，输入输出是以程序为基准，通过程序向文件书写成为输出，Writer

通过程序阅读文件，是把文件内容输入到程序当中，所以是输入，Reader。

输入类:Reader类和InputStream类 只能向其中输入数据而不能输出数据

输出类:Writer类和OutputStream类 只能向外部输出数据而不能输入数据

上面的类都是抽象类，需要使用他们的子类

Writer是输出类，记清楚

创建类对象将File类的对象放入其中，将文件对象与输入或输出流产生联系

字节流

输入类的方法

FileInputStream类

构造器填写File对象

.read（）；读取一个字节数据

.read(byte[] b)将数据流中读取到数组中

.read（byte[] b,int off,int len）;从off开始读取长度为len的数据到数组b中

.close();关闭输出流　流对象使用完毕后记得关闭

.available();返回输入流读取的估计字符节数

FileOutputStream();

构造器，前面填写对象，后面填写对象，如果填写的是true，则

在源文件后面填写，否则write（）覆盖源文件。

不能通过父类创建对象？

构造器填写File对象

方法

 write(int x); 想文档中输入内容，覆盖原内容？输入int型，自动转化为char型

write(int[] x)；输出一个数组中的全部字节

write(byte[] b,int off,int len);将字节数组中从off开始输出长度为len长度的字符

参数 数组 从哪开始写，写多长

传入或者输出时用数组方便一点

字符流输入法:

Reader类 

子类 FileReader();方法和FileInputStream()；相同

BufferedReader类

buffer缓冲

一次缓存一行，可以按行读取

String lineWords = bfr.readLine();

while(lineWords != -1){

Syso(lineWords);

line = bfr.readLine();

}

BufferedWriter类

多一个newLine（）；方法

多一个flush（）；方法，关闭前，先清除一下缓冲

创建被复制文件路径，创建要复制到的路径，通过要复制到的路径，创建复制文件路径，路径对象.mkdirs();文件对象.creatNewFile(); 文件.read(数组);输出对象.write（）；

使用完一定要关闭

字节流和字符流区别在于流中的传输格式不一样，一个传输的是

byte型，一个传输的是char型

字符流在新文件中写入后一定要flush（）；刷新一下，否则不会出现；

#Log4j

以文件的方式输出异常信息

log4j.appender.file = org.apache.log4j.FileAppender

异常文件的路径

log4j.appender.file.File  = d:/logfile.txt

进行异常文件的布局

log4j.appender.file.layout = org.apache.log4j.PatternLayout

log4j.appender.file.layout.ConversionPattern = [%-5p][%d{yyyy-MM-dd HH:mm:ss}] %c %L %m%n

以滚动文件的方式进行记录

log4j.appender.rollfile = org.apache.log4j.RollingFileAppender

log4j.appender.rollfile.File  = d:/logfile.txt

设置滚动文件的大小

log4j.appender.rollfile.MaxFileSize = 10 KB;

log4j.appender.rollfile.layout = org.apache.log4j.PatternLayout

log4j.appender.rollfile.layout.ConversionPattern = [%-5p][%d{yyyy-MM-dd HH:mm:ss}] %c %L %m%n

4项依次为 输出级别 输出debug及以上的异常 输出到 console，file，rollfile 对象

log4j.rootLogger = debug,console,file,rollfile

文件必须叫log4j才能找到，否则无法运行

出现异常的记录要到catch里 logger.error();里面通常配合 e.getMessage();  e.printStackTrace()返回的不是字符串

填了泛型就不需要集合类型的强制转换了

判断list中是否含有自己要查询的元素用 contains 就可以了不用自己写

HashMap的containsKey("");可以判断是否有相应的key值

#Object类方法

public Object clone() throws CloneNotSupportedException;

克隆对象需要继承java.lang.CloneAble接口，而且子类只需要继续调用Object类的clone()方法就可以成功克隆该对象

public String toString();

直接输出对象时会默认调用toString方法;

print()调用String.valueOf() valueOf()会调用toString方法

对象比较public boolean equals(Object obj);

保存set集合时，会依靠hashCode()和equals()判断是否为重复对象

hashCode();

每个地址的唯一标识，比较时会先判断编码是否相同，再通过equals();

getClass();

线程等待 wait();直到执行到notify()唤醒第一个等待线程

或者notifyAll()方法后执行

垃圾回收前释放 finalize() throws Throwable;

当使用垃圾回收无用的垃圾时。默认调用。

#UML图

充裕的UML图包括，用例图，类图，组件图，部署图，顺序图，活动图和状态机图等。

用例图，用于描述系统提供的系列功能，而每个用例则代表系统的一个功能模块，用例图的主要目的是帮助开发团队以一种可视化的方式理解系统的供需需求。用例图对系统的实现不做人黑说明，仅仅是系统功能的描述

类图，功能最丰富，使用最广泛的UML图，类图表示系统中应该包含哪些实体，各实体之间应该如何关联。类图除了可以表示自身之外，还可以表示实体之间的相互联系，类之间有三种基本的关系，关联，泛化，依赖。

当类中的某个属性，引用到另一个实体，则变成了关联。

关联关系有两种特例：聚合和组合，他们都有部分和整体的关系，但通常认为组合比聚合更加严格。当某个实体聚合成另一个实体时，该实体还可以同时使是一个实体的部分，聚合用空心四边形，组合用实心四边形

泛化，可以视为继承的关系，通常用空心三角形表示

依赖如果一个类的改动会影响到另一个类，则称俩个类之间存在依赖，改动的类将消息发给另一个类，改动的类以一个类作为参数部分，改动的类是另一个类的操作参数，通常而言，依赖是单向的。

#垃圾回收机制

垃圾回收由JRE进行，一般是在CPU空闲不足或内存不足时自动进行的，它自动释放不再被程序引用的对象。内存碎片是分配给对象的内存块和内存块之间的空闲区域。

垃圾回收机制的工作目标是回收无用对象的内存空间，这些内存空间都是JVM堆内存里的内存空间，垃圾回收只能回收内存资源，对于其他物力资源，如数据库连接、磁盘I/O等资源则无能为力。为了让垃圾回收尽快回收某一个不再使用的对象，可以将该对象的引用变量设置为null，通过该种方式暗示垃圾回收器回收该对象。

基于对象概念也使用了对象，但是没有继承，没有继承多态也就无从谈起，例如javaScript;

在垃圾回收机制回收任何对象之前，总会先调用他的finalize()方法，该方法可能使该对象从新复活，从而导致垃圾回收机制对该对象取消回收。程序只能控制一个对象何时不再被任何引用变量引用，决不能控制他何时被回收。

建议垃圾进行回收。 System.gc();

Runtime.getTuntime.gc();

finalize()永远不要自己调用。该方法的调用具有一定的不确定性，不要将其当成一定会执行的方法。执行该方法可能会使对象从新变为可达状态。当该方法出现异常时，垃圾回收机制不会报告异常，程序继续执行。

System.runFinalization();强制垃圾回收机制调用finalize()方法。回收test类的对象

​	@Override

​	protected void finalize() throws Throwable {

​		System.out.println("垃圾回收");

​		System.out.println(this.toString());

​		test = this;

​	}

#正则表达式

QQ账号 "[1-9][0-9]{4,11}"

符号接受 .任意字符 \\d数字 \\D非数字 \w代表a-z,A-Z,0-9,_

\\W正好相反  \s 空白字符  \t\n\x0B\f\r都属于空白字符 \\S非空白字符 

边界符号. ^行的开头 $行的结束 \\b单词边界

数量词: ? 出现零次或者一次

\* 零次或者多次

+一次或者多次

{3，} 至少进行3次或者以上的匹配

String[] split = qq.split("[ ]+"); 对中间多个空格的两个字符串进行分割

String[] split = qq.split("(\\.)\\1"); ()用于对其中的内容进行封装，使用其编号的转义就可以重新进行使用。编号从1开始。

多参数时可以通过$符号取到前面一个参数的组

Pattern p = Pattern.compile("");将规则编译成对象

Matcher m = p.matcher(""); 和要操作的字符串进行关联，生成 

m.matches();使用匹配对象方法对字符串进行操作

String a3 = a2.replaceAll("(.)\\1+", "$1");

String replaceAll = a2.replaceAll("(\\d{3})(\\d{4})(\\d{4})", "$1****$3");

#哈希与HashMap原理

存储的地址叫做哈希地址，通过计算可以让关键字和存储地址间直接形成一个映射的关系就叫做哈希函数

哈希表的基本概念，设定哈希函数及处理冲突的方法将查找表中的元素存储在一段有限的连续空间中，就得到了哈希表

最常见的是除留余数法，通过对某一个数字进行取余数，将得到的余数进行不同的地址映射，这个数一定要是一个质数，否则可能会极大的减小哈希表的范围，如果发生冲突，则使用二次处理的方式进行新的哈希地址的寻找

例如：

哈希函数: H(key) = key % 7;

发生冲突时:Hi = (H(key)+di) % 7 其中di =1,2,3,4,5

H(key)是取余获得出来的地址，如果冲突则对其+1在取余

哈希表在底层存的是 key,value, 以及next指针。

放置地址是hashcode%表的程度。

hashmap底层存储的是一个哈希表，实际上是一个entry的数组，存储的数组，包括key,value以及next指针的信息。如果元素得到相同的数组下标，就对该链表下的全部元素进行equals比较 ，如果equals相等，这不添加，否则吧原先的元素挤出去，并用链式连接，用next指向。没有指向的时候应该next里是没有东西的，在后面有连接的元素之后，next的值则存储的是后面一个元素的地址，所以HashMap不允许放入相同的元素。如果链表过长效率一样会低下，所以会在负载因子达到0.75的时候扩容，但是极端的情况下，有可能仅存在一个位置，于是在jdk1.8

在数组+链表的基础上添加了红黑树，当某一个位置的碰撞大于8且容器的总容量大于64时，会将其转为红黑树，除了添加以外效率都高，红黑树是通过hashcode比较，大于的放在一侧，小于的放在一侧，可以少进行很多次的比较

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/9973d744758e42799da051372cce7a79/clipboard.png)

哈希表中的每个元素是一个entry对象，包含key,value以及next指针。

迭代器遍历期间，如果其他线程对该集合进行了修改操作，则会触发fail-fast机制，也就是说不可以在迭代期间通过集合本身对集合进行修改

集合.removeIf(Perdicate) 函数式接口，进行判断并删除，类型是object的如果需要使用对应类的对应方法需要进行强制转换

set集合里面的元素的无序的，添加新元素时，会调用其equals(),和hashCode()方法，如果返回true，和两者的哈希地址一样时，才判断可以添加元素

当通过类中的某个元素进行重写equals()和hashCode()方法时，当数据已经存入，则不要修改它的值，会使Set无法进行正常访问。

访问HashSet类的时候，遍历是按照无序的顺序进行的。

LinkedHashSet也是使用哈希值获取元素的位置，但同时也使用链表维护元素的次序，也就是说我们可以按照顺序进行元素的访问。

list与set的根本区别在于元素不允许重复

​		File file = new File("D:/我的文档1.txt");

​		int data;

​		InputStreamReader isr = new InputStreamReader(new FileInputStream(file), "GBK");

​		char[] words = new char[1024];

​		while(isr.read() != -1){

​			isr.read(words);

​			System.out.println(words);

​		}

File file = new File("F:/新建文本文档.txt");

BufferedReader in = new BufferedReader(new InputStreamReader(new FileInputStream(file),"utf-8"));  

String string = in.readLine();

System.out.println(string);

#单例模式

构造器私有，提供对外的构造器，这种模式就是最简单的单例模式，但是有很严重的弊端，如线程不安全，可能会有多个该对象，线程不安全

懒汉模式

对产生实例的方法加上同步锁，需要实例的时候在进行初始化，因为加了同步锁，所以就不会产生多个对象，不会产生线程同步问题，这样就可以很好的并发情况下进行开发，但会导致效率的低下，所以不推荐使用

饿汉模式

将需要产生实例以引用的方式写在类自身内部，写成static变量，基于classLoader机制进行初始化，这样在类加载的时候我们所需要的实例就已经进行了初始化就不存在线程安全问题，不会出现多个该实例对象，但是类加载的时候就对该对象进行初始化会比较浪费资源，可以在一个静态内部类中，写一个静态方法创建常量保存该对象，等到有人调取getInstance()方法通过该常量进行初始化

public class MySingleton {

//创建用于单例模式的实例

​	private static MySingleton mySingleton;

​	//用一个静态内部类存储实例，这样该类未被调用的时候不对象就不会被装载

​	public static class SingletonHelper{

​		private static final MySingleton INSTANSE = new MySingleton();

​	}

​	//通过获取实例进行了初始化

​	public static MySingleton getInstanse(){

​		mySingleton = SingletonHelper.INSTANSE;

​		return mySingleton;

​	}

​	//测试是否是单例模式

​	public static MySingleton test(){

​		return mySingleton;

​	}

}

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/f0e7f0ec24de45c9b7d7de72da567c75/clipboard.png)

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/8b092d344290423e8c565f74f85b8b2d/clipboard.png)

静态内部内在使用的时候才被加载



#异常链

异常链，在一个异常处理中抛出另外一个异常，可以通过

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/145162f35d5d4829a8500d2092cc6e0d/clipboard.png)

设置引发新异常的原因异常是什么，在调用者处

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/d6ffd73bbfe44568974c1bc69e5ecbf8/clipboard.png)

即可获得引发异常的异常原因

throws是在 方法的()后面使用

Arrays.asList 获得的List不能添加删除元素

一般来说用ArrayList()性能较好，迭代ArrayList()时使用get方法好，而使用LinkedList()时，通过迭代器迭代较好

当使用iterator进行集合的迭代时，并不能对值进行修改。

静态的元素里不能使用泛型，因为泛型并不是单独的类，不能占用类的变量，使用了不同泛型的类也是同一个类

LinkedHashSet底层存储元素时是无序的的但是元素本身形式是双向链表用于维护元素的顺序

元注解

#基础注解

@Retention(RetentionPolicy.SOURCE)

可选值有三个SOURCE 	编译器直接丢弃这种策略的注解

CLASS	编译器把注解记录在class文件中，在运行的时候不会保留注解

RUNTIME	编译器会把注解记录在class文件中，当运行java时，java会保留注解，程序可以通过注释获取注解信息

@Target	修饰的注解声明该注解可以用于哪些程序元素

TYPE class,interface,enum	FIELD 属性	METHOD 方法		PARAMETER 参数

CONSTRUCTOR	构造器	LOCAL_VARIBEL	变量		ANOTATION_TYPE注解类型		PACKAGE	包

@Documented	该注解将被javadoc提取为文档   必须是RetentionPolicy.RUNTIME

@Inherited 说明被修饰的注解具有继承性

#lambda

lambda可以代替只有一个抽象函数的接口实现，告别内部类，同时还提升对集合的迭代，遍历，过滤的操作。

函数式编程 参数类型自动推断 代码量少，简洁

更容易使用并行

只有一个抽象方法的接口就是函数式接口，默认方法，静态方法，以及Object中的方法都除外。

BiConsumer; 代表两个输入

Consumer; 代 表一个输入

Supplier; 代表一个输出

Function<T, R> T是输入 R是输出 一般输入输出类型是不一样的

UnaryOperator<T> 继承自function输入和输出都是相同类型的

BiFunction<T, U, R> 两个输入一个输出 一般类型是不一样的

BinaryOperator<T> 将两个输入一个输出的类型搞成一样的

lambda表达式是一个函数是接口的实例	

()  -> {} 无参无返回值

基本语法: 

Runable r = () -> {System.out.print();};

r.run();方法可以启动一个表达式

() -> {return 100;} 有返回值 可以返回null

(int x)-> {return x+1;} 参数类型可以省略，表达式会自动进行推断

多个参数用"，"隔开，类型全部省略，或者全部不省略，只能选一种

不能使用 final 修饰符，修饰变量

不允许使用可能会抛出的一个异常

表达式参数与返回值应该对应，没有返回值的方法也可以执行有返回值的方法，但不可以进行接收

方法的引用，如果某一个功能使用别的方法正好可以完成该功能则可以引用，如果还需要进行其他的功能，则不可以使用方法的引用。

 = x :: method; x静态方法所在的类名 method方法名，不写括号

实例方法的引用:

= () -> new x.y(); 引用一个对象的方法  = new x::y;

对象方法引用:

没有输入参数的不能使用对象方法引用，第一个参数是引用该方法的类型，其余的参数都是该方法所使用的参数，才可以使用，直接使用第一个参数调用后面的参数，也就是说方法的第一个参数是哪个类执行了该方法

类名::方法名 接口中的参数正好是引用的方法的所在类，且多余的参数都是该方法所需要的才可以使用。

构造函数的调用:

如果某个接口的实现可以用某个类的无参构造方法来进行实现，则可以进行无参构造函数的调用，没有无参函数的不能进行调用

类名::new；来进行调用，可以调用有参数的构造器，调用哪个根据接口所输入的参数进行自动选择;

##Stream

Stream API，主要用于处理数组集合等的API

1，不是数据结构，没有内部存储

2，不支持索引访问

3，延迟计算

4，支持并行

5，很容易生成数组或者集合

6，支持过滤，查找，转换，汇总，聚合等操作

Stream分为 源，中间，终止操作。

如果未进行终止操作则不会进行，遍历等操作

流的源可以是一个数组，集合，生成器方法，一个I/O通道等

一个流可以有一个或者多个中间操作，每一个中间操作都会返回一个崭新的流，供下一个操作使用。一个流只会有一个终止操作获得stream的方式

Stream.of(arr); 里面可以填写一个数组

list.stream();可以获得一个集合的流

Stream<Integer> generate = Stream.generate(() -> 1);通过该方法填写一个 supplier<T> s来获得一个输出1的流，如果foreach()会无限循环

Stream<Integer> iterate = Stream.iterate(1, x -> x+1);

里面填入一个 UnaryOperator<T> 继承自function但一般输入与输出的类型不同 foreach();会无限自加

终断方法 limit写在foreach之前，可以规定执行的次数

通过其他的方式获取流，例如str.chars();

会返回一个IntStream

filter方法，Predicate<? super T>参数，进行一个过滤作用

filter(x -> {System.out.println(2);return x.equals("kof");})

# 多线程

## 死锁

```java

public class TestDieLock {
    static StringBuffer sb1= new StringBuffer();
    static StringBuffer sb2= new StringBuffer();

    public static void main(String[] args) {
        new Thread(){
            @Override
            public void run() {
                synchronized (sb1){
                    sb1.append("A");
                    synchronized (sb2){
                        sb2.append("B");
                    }
                }
                System.out.println(sb1);
                System.out.println(sb2);
            }
        }.start();

        new Thread(){
            @Override
            public void run() {
                synchronized (sb2){
                    sb1.append("C");
                    synchronized (sb1){
                        sb2.append("D");
                    }
                }
                System.out.println(sb1);
                System.out.println(sb2);
            }
        }.start();
    }
}
```

如上，两条线程都在等待对方执行对方释放锁，这种情况就成为死锁，如果几次实验未出效果，可以给第一步睡眠10ms

## 线程的通信

wait()

notify()唤醒当前线程

notifyAll()唤醒全部的线程

三个必须使用在同步代码块当中

# 反射

动态语言的关键，反射机制允许程序在执行期间借助于Reflection API取得类的内部信息，并能直接操作任意对象的内部属性方法

## 反射所包含的功能

- 运行时判断一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时判断任意一个类所具有的成员变量和方法
- 在运行时调用任意一个对象的成员变量和方法
- 生成动态代理

#反射的方法

```java
Class<Person> clazz = Person.class;
Person person = clazz.newInstance();//获取一个类的对象实例
Field age = clazz.getField("age");
age.set(person,100);//获取类的属性，并为对象赋值，private不能获取，只有public能获取
```

```java
Field name = clazz.getDeclaredField("name");
name.setAccessible(true);//private属性的获取
```

```java
Method method = clazz.getMethod("showNation",String.class);
method.invoke(person,"chinese");//获取类中的方法
```

```java
Class clazz = person.getClass();//通过对象获取Class
System.out.println("clazz = " + clazz);
clazz = class com.wyq.Person	//获得其运行时类
```

.class文件加载到内存区后，就是一个运行时类，存在在缓存区，那么这个运行时类本身就是一个Class的实例！一个运行时类只能加载一次 .class意思就是指向内存中的class文件

```java
Class clazz = Class.forName("com.wyq.Person");//通过完全限定名获取类的反射
```

```java
ClassLoader classLoader = this.getClass().getClassLoader();
Class person = classLoader.loadClass("com.wyq.Person");//通过类加载器获得类的对象
```

## 类的加载器

- java编译器将java文件编译为.class文件
- 通过类装载器将class文件装载到内存中
  - 加载器生成一个类的Class对象
  - 将类的二进制数据合并到Jre中
  - JVM负责对类进行初始化

- 通过字节码校验器对文件进行校验
- 通过解释器解释成对应不同系统的指令集文件
- 在操作平台上运行

通过反射获取一个类的全部信息

```java
clazz.getFields();		//获取本类及父类声明为public的属性
clazz.getDeclaredFields();	//获取运行时类全部声明的属性
```

```java
int i = f.getModifiers();	//获取权限修饰符的数值
String modifier = Modifier.toString(i);	//将值转化为对应的修饰符
f.getName();	//获取对应的属性名
Class type = f.getType();	//获取属性的类型
```

```java
Method[] methods = clazz.getMethods();	//获取运行时类的方法,获取运行时类及其父类中所有声明为public的方法
clazz.getDeclaredMethods();	//获取运行时类所有的声明的方法
```

可以获取方法的如注解，修饰符，返回值，方法名，形参列表，抛出的异常都可以进行获取

```java
Constructor[] c = clazz.getConstructors();	//获取运行时类的方法,获取运行时类及其父类中所有声明为public的构造方法
clazz.getDeclaredConstructors();	//获取运行时类所有的声明的构造方法
```

```java
Class superClass = clazz.getSuperClass();	//获取不带泛型的父类
```

```java
Type type = clazz.getGenericSuperclass();	//获取带泛型的父类
ParameterizedType param = param.getActualTypeArguments();	//获取泛型的子接口
Type[] args = param.getActualTypeArgument();	//获取泛型的类型
(Class)args[0].getName();	//泛型的类名
```

```java
Class[] interfaces = clazz.getInterfaces();	//获取类的接口
```

```java
Package p = clazz.getPackage();	//获取包
```

```java
Annotation[] annotations = clazz.getAnnotations();	//获取生命周期为RunTime的注解
```

##对属性方法进行操作
```java
Field name = clazz.getField("name");
Person p = clazz.newInstance();
name.set(p,"王玉强");
name.setAccessicle(true);	//对于私有属性无法无法访问，设置其可访问性为true即可
```

# 动态代理

静态代理，在编译期代理目标对象和类，不利于程序的扩展，同时，每一个代理类只能为一个接口服务，这样程序开发中必然会产生多个代理

最好通过一个代理类完成全部的代理功能

```java
interface Human {
    void info();

    void fly();
}

//被代理类
class SuperMan implements Human {

    @Override
    public void info() {
        System.out.println("我是超人，我怕谁！");
    }

    @Override
    public void fly() {
        System.out.println("I believe i can fly!");
    }
}
//插入方法
class HumanUtil {
    public void method1() {
        System.out.println("===========方法一===========");
    }

    public void method2() {
        System.out.println("=========== 方法二===========");
    }
}
//动态代理类
class MyInvocationHanlder implements InvocationHandler {
    Object obj;

    public void setObj(Object obj) {
        this.obj = obj;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        HumanUtil humanUtil = new HumanUtil();
        humanUtil.method1();
        Object returnVal = method.invoke(obj, args);
        humanUtil.method2();
        return returnVal;
    }
}
//获取一个被代理的对象
class MyProxy {
    //创建一个被代理类的对象
    public static Object getProxyInstance(Object obj){
        MyInvocationHanlder hanlder = new MyInvocationHanlder();
        hanlder.setObj(obj);
        return Proxy.newProxyInstance(obj.getClass().getClassLoader(),
                obj.getClass().getInterfaces(),hanlder);
    }
}

public class TestInvoke {
    public static void main(String[] args) {
        SuperMan man = new SuperMan();
        Object proxyInstance = MyProxy.getProxyInstance(man);
        Human human = (Human)proxyInstance;
        human.info();
        human.fly();
    }
}
```

# 网络编程

- 网络编程概述
- 通讯要素
  - IP和端口号
  - 网络协议
- InetAddress
- TCP/IP网络通信
- UDP通信协议
- URL编程

## 计算机网络

将分布在世界的计算机用这门的设备和通信线路进行连接，形成一个规模大，功能强的网络系统，从而使众多的计算机可以方便的互相传递信息，共享硬件，软件，数据信息等资源

### InetAddress类

IP地址	访问自己127.0.0.1

```java
InetAddress inetAddress = InetAddress.getByName("www.baidu.com");//通过类获取百度域名/IP地址
inetAddress.getHostName();	//获取百度的域名
inetAddress.getHostAddress();	//获取百度的IP地址
InetAddress localHost = InetAddress.getLocalHost();	//获取本机信息
```

通过IP地址寻找电脑，通过端口号指定发送消息的程序

TCP/IP协议簇		不会数据丢失，可以传递较为重要的数据

- 使用TCP/IP协议前，需要进行TCP/IP连接，形成数据传输的通道
- 传输前，采取三次握手方式，是可靠的，客户端向服务器端发信息检测，服务器接收到后给客户端返回信号，客户端向服务器端进行数据的传输
- TCP协议进行通信的两个应用进程:客户端服务端
- 在连接中可进行大量数据的传输
- 传输完毕，需释放已经建立的连接，效率低

UDP协议           数据可能会丢失一些，但是速度快，可以传输视频等文件

- 将数据、源、目的封装成数据包，不需要建立连接
- 每个数据限制在64K内
- 无需连接，不可靠
- 发送数据结束无需释放资源，速度快

##使用Socket(套接字)进行网络编程

- 端口号与IP地址的组合就叫做一个套接字，网络通信就是Scoket间的通信。
- Socket运行程序把网络连接当做一个流，数据在两个Socket间进行IO传输

```java
public class TestInvoke {
    //客户端
    @Test
    public void client() {
        Socket socket = null;
        OutputStream os = null;
        try {
            //指定一个服务端的IP及端口号的socket
            socket = new Socket(InetAddress.getByName("127.0.0.1"), 9090);
            //通过输出流向服务端输入信息
            os = socket.getOutputStream();
            os.write("我是客户端，请多关照".getBytes());
            //告诉客户端信息传输完毕
            socket.shutdownOutput();
            InputStream is = socket.getInputStream();
            byte[] b = new byte[20];
            int len;
            while ((len = is.read(b)) != -1) {
                String str = new String(b,0,len);
                System.out.println("str = " + str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //资源关闭
            try {
                if (os != null) {
                    os.close();
                }
                if (socket != null) {
                    socket.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    //服务端
    @Test
    public void server() {
        InputStream inputStream = null;
        Socket accept = null;
        ServerSocket serverSocket = null;
        try {
            //获取发送信息的套接字信息
            //服务端ServerSocket对象，并指明服务的端口号
            serverSocket = new ServerSocket(9090);
            //获得socket对象
            accept = serverSocket.accept();
            String hostName = accept.getInetAddress().getHostName();
            //获取从客户端发送来的输入流
            inputStream = accept.getInputStream();
            byte[] words = new byte[20];
            int len;
            while ((len = inputStream.read(words)) != -1) {
                String str = new String(words, 0, len);
                System.out.println("str = " + str);
            }
            //接收信息完成后返回信息
            OutputStream os = accept.getOutputStream();
            os.write("我已经接收到了你的信息".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //资源关闭
            try {
                if (inputStream != null) {
                    inputStream.close();
                }
                if (accept != null) {
                    accept.close();
                }
                if (serverSocket != null) {
                    serverSocket.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

因为inputStream的read方法是阻塞式的，他无法判断什么时候用户的信息传输完毕，所以需要

socket.shutdownOutput();

## UDP协议

```
public class TestInvoke {
    @Test
    public void send() {
        DatagramSocket datagramSocket = null;
        try {
            datagramSocket = new DatagramSocket();
            byte[] str = "我是要发送的数据".getBytes();
            //创建数据报
            DatagramPacket datagramPacket = new DatagramPacket(str, 0, str.length, InetAddress.getByName("127.0.0.1"), 9090);
            //发送数据报
            datagramSocket.send(datagramPacket);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (datagramSocket != null) {
                datagramSocket.close();
            }
        }
    }

    @Test
    public void recive() {
        DatagramSocket datagramSocket = null;
        try {
            datagramSocket = new DatagramSocket(9090);
            byte[] bytes = new byte[1024];
            DatagramPacket datagramPacket = new DatagramPacket(bytes, 0, bytes.length);
            datagramSocket.receive(datagramPacket);
            String str = new String(datagramPacket.getData(), 0, datagramPacket.getLength());
            System.out.println("str = " + str);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (datagramSocket != null) {
                datagramSocket.close();
            }
        }
    }
}
```

## url

一个URL对应互联网上的一个资源，我们可以通过URL的对象对应一个资源

```java
new URL("");	//一个资源的路径
```

```java
InputStream is = url.openStream();	//获取一个输入流
```

如果也想进行写出操作

```java
UrlConnection uc = url.getConnection();	//获取与资源的连接
InputStream is = uc.getInputStream();	//可以获取输入输出流
```

# api开放

## 危险

- 数据窃取，用户的密码等信息被窃取
- 数据篡改，提交的数据抓包在进行提交
- 数据泄露，爬虫

# 对应解决方式

- 加密
- MD5做数据混淆
- TOKEN令牌