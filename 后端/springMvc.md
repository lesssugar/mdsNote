springmvc

@Controller

@RequestMapping

@Resource和@Autowired

@PathVariable

@CookieValue

@RequestParam

@SessionAttributes

@ModelAttribute

@ResponseBody

@DateTimeFormat

为展示层提供的基于mvc设计理念的优秀web框架，是现在最主流的框架之一

springmvc3.0之后，全面超越了struts2成为最优秀的mvc框架，通过一套mvc注解，让pojo成为处理请求的控制器，而无需实现任何借口。

支持rest风格的url请求

采用松散的耦合的可插拔组件结构，比其他mvc框架更具扩展性和灵活性

类上面也可以加requestMapping注解作用是为每个方法的url前面加上类所写的url

还可以指定请求方式value="url",method=RequestMethod.post

请求参数与请求头的对应，不常用了解即可

params={"username","age!=10"}或者一个素组的形式

!=左右两侧不能添加空格

headers用法类似

ant风格资源地址支持 了解即可

?匹配文件中的一个字符

*匹配文件中的任意字符

**匹配多层路径

/test/*/bac  匹配一层路径

/test/**/abc 匹配多层路径

/test/a?/abc 支持a任意字符的路径@RequestMapping("/hello/{id}")

public String hello(@PathVariable("id") Integer id)

pathVariable注解，可以获得url中对应的部分的值

rest，资源表现层状态转化，是目前最流行的一种互联网软件架构，结构清晰，符合标准，易于理解，扩展方便。

资源，就是网络上的一个实体，可以使一段文本，音乐，图片，可以用一个url对其进行指向，把资源具体呈现出来的形式，叫做他的表现层。比如文本可以用txt格式展现，可以用html,甚至可以用二进制方式进行展现。

状态转化，每发出一次请求，就代表了客户端和服务器的一次交互过程，http协议，是一个无状态协议，即所有的状态都保存在服务器端，因此如果客户端想要操作服务器，必须通过某种手段，让服务器发生"状态转化"，而这种转化石建立在变现层之上的，具体来说，就是http中表示操作方式的四个动词，get,post,put,delete分别对应，获取，添加，更新，删除

HiddenHttpMethodFilter

可以将post请求转换为delete或put请求

<filter>

​	<filter-name>HiddenHttpMethodFilter</filter-name>

​	<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>

</filter>

<filter-mapping>

<filter-name>HiddenHttpMethodFilter</filter-name>

​	<url-pattern>/*</url-pattern>

</filter-mapping>

配置HiddenHttpMethodFilter类

  <form action="/springmvc/testRest" method="post">

  	<input type="hidden" name="_method" value="DELETE">

  </form>

截取到name为_method的提交表单，如果method是post则会根据自己填写的value值对该请求进行一个转化，该例中转化为的是DELETE请求，总来的说就是为该表单携带一个name=_method的隐藏域

该方式想要获取参数则要配合前面的@PathVariable注解进行参数的获取

@RequestParam可以通过问号传参获取对应的参数值

@RequestMapping("/hello") 路径

@RequestParam(value = "name"，required=false) String name获取对应的参数	

如果某个参数可能没有值，则需要填写required否则会报错，因为默认该参数必须需要，某个参数可以没有填写

基本数据类型不能为空，封装类可以返回null而，基本数据类型需要添加defaultValue=""，则在该基本数据类型没有的时候使用该值进行传递

@RequestHeader

请求头的获取，用法基本同@RequestParam映射请求头信息。了解即可

@CookieValue("JSESSIONID") String sessionId

获取一个cookie的value用法和前面大致相同

可以直接放入某种pojo类作为参数，例如注册账号时，会自动按照表单的name属性值对属性进行封装，级联属性在name属性处写级联样式,例如:adress.city 大小写不会对属性的封装造成影响

该方式会有乱码。需要对获取到的字符串进行解码在编码的过程

参数中使用ServletApi

例如ServletRequest,ServletResponse,HttpSession,Writer

ModleAndView

包含有模型和视图信息，即使查询出来的数据，会添加到requestScoper作用域当中，而view则是要进行解析的页面

springmvc会把ModelAndView中的数据放到request域对象中

参数还可以传入一个map类型的数据，也会被封装到域中，实际上可以是Model类型，或者ModelMap类型，如果要添加对象需要使用addObject()方法，可以添加一个Object对象到request域中

如果想要将得到的数据模型的model放入到session域中。可以在类上面写@SessionAttruibute注解，不能写在方法上面，属性俩个 value={""} 填写key的数组，types={String.class} .填写类.class

则会将该属性放置到session域中

修改属性的值得时候如果有一个值不能修改，因为是新创建的对象，如果直接对数据库进行操作，可能会造成某些字段的空值，@ModelAttribute

public void before(@RequestParam(value="name",required=false) String name,Map<String, Object> map){

​		System.out.println(name);

​		if (name != null) {

​			System.out.println("name != null");

​			User user = new User("tom", 12, "805197832@qq.com", null);

​			map.put("user", user);

​		}

​	}

通过@ModelAttribute可以冲数据库拿一个对象用于使没有的属性用数据库中的值进行填写

map的key名字需要和调用的地方一样，由该注解标记的方法

执行@ModelAtturibute 注解修饰的方法，把数据库获取的user放入到map当中，键为user

springmvc从map中取出user对象，并把表单的请求，赋给该user属性，springmvc把上述对象传入目标方法的参数，然后在调取的方法中对该对象进行赋值，如果是null的值则用数据库对象中的值，如果有值则用表单的值，即使是空字符串也会使用，不会使用数据库中的数据

修饰的方法中，放入到map时的键需要和目标方法入参类型的首字母小写形式即可

该注解也可以来修饰方法POJO的入参，也可以用来修饰入参方法的参数，可以映射map其他ModelAtturibute注解map中存入值得key值，这样即使不同也可以映射

但是存储对象的键值袁辉被改变成这个，使用el,jstl的时候需要注意 modelAtturibute(value="a") User user 

视图中展示user 则是 ${a}

SpringMvc中的Controller默认是单例的，所以不要在控制器中定义成员变量

return "redirect:/login.jsp"; 用该种方式为重定向

如果希望是转发 则用 "forward"即可

重定向的情况下视图解析器无效，应该直接写需要跳转到的网页

该资源不能再WEB-INF下，无法访问，通过转发就可以找到是因为该权限是在服务器进行转交，所以可以找到，如果是重定向是在客户端重新发送请求，所以无法找到

异常处理

对单个控制器当中的异常，可以在方法内 

![img](D:/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq6A8D41E5978DE6EA5AB3219203EC493B/8da347ba980641249e7651735e9b5736/clipboard.png)

@ExceptionHandler(value={RuntimeException.class})

public String handlerException(RuntimeException       e,HttpServletRequest req) {

​		System.out.println(1);

​		req.setAttribute("e", e);

​		return "error";

​	}	

@ExceptionHandler

通过该注解将该控制器中发生的异常截获到本方法当中，

e.message 然后通过视图解析器即可在我们展示异常的页面展示该信息

全局异常处理

通过SimpleMappingExceptionResolver类来进行解析，

<bean		class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">

​		<property name="exceptionMappings">

​			<props>

​				<prop key="java.lang.RuntimeException">error</prop>

​			</props>

​		</property>

​	</bean>

SimpleMappingExceptionResolver

通过配置该类，属性中配置对应的异常类型来进行异常的截获，然后跳转到对应的异常处理页面

${exception.message}

处理页面的对象改为这个，信息仍是我们抛出异常时添加的异常信息

表单传递过来的时间值无法与Date类型进行绑定，可以在属性上打@DateTimeFormat(pattern='yyyy-MM-dd')

文件的上传

servlet中配置文件中的CommonsMultipartResolver类必须叫multipartResolver，因为是由前端控制器直接调用的

静态资源的访问

${pageContext.request.contextPath }/statics/css/style.css

<mvc:resources mapping="/statics/**" location="/statics/" />

<!-- 配置用于执行多条sql语句的sqlSession -->

<bean name="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">

<constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>

<constructor-arg name="executorType" value="BATCH"></constructor-arg>

</bean>

sqlSession.getMapper()；这样获得的mapper就是可以用于批量执行sql语句的mapper

@RunWith(SpringJUnit4ClassRunner.class)

@ContextConfiguration(locations={"classpath:applicationContext.xml"})

public class MapperTest {

​	@Autowired

​	private DepartmentMapper departmentMapper;

​	@Test

​	public void mapperT(){

​		System.out.println(departmentMapper);

​	}

}

​	<!-- 配置一个可以执行多条语句的sqlSession -->

​	<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">

​		<constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>		

​		<constructor-arg name="executorType" value="BATCH"></constructor-arg>

​	</bean>

服务器如果给手机等设备直接发送页面，客户端比较难进行解析，可以发送成ajax请求，服务器将将查询到的信息以json字符串的形式发送给浏览器，接收到用js对json进行解析，通过dom增删改的形式来改变页面。返回json就额可以实现客户端的无关性

文件上传

项目都是租用服务器，不能使用本地磁盘

@Controller

public class FileController {

​	@RequestMapping("/hello")

​	public String queryFileData(@RequestParam("upFile") MultipartFile file, HttpServletRequest request) {

​		System.out.println(1);

​		String path = null;

​		// MultipartFile是对当前上传的文件的封装，当要同时上传多个文件时，可以给定多个MultipartFile参数(数组)

​		if (!file.isEmpty()) {

​			String type = file.getOriginalFilename().substring(file.getOriginalFilename().indexOf("."));// 取文件格式后缀名

​			String filename = System.currentTimeMillis() + type;// 取当前时间戳作为文件名

​			//因为不同的系统分隔符不同所以要用File.separator，获得webaap下的images文件夹的绝对路径

​			path = request.getSession().getServletContext()

​					.getRealPath(File.separator + "images" + File.separator + filename);// 存放位置

​			File upFile = new File(path);

​			// 如果要限制文件大小

​			if (file.getSize() > 0) {

​			}

​			// 如果要限制文件类型

​			if (type.equals(".jpg")) {

​			}

​			try {

​				// FileUtils.copyInputStreamToFile()这个方法里对IO进行了自动操作，不需要额外的再去关闭IO流

​				FileUtils.copyInputStreamToFile(file.getInputStream(), upFile);// 复制临时文件到指定目录下

​				//此处将文件路径录入数据库

​				System.out.println(path);

​			} catch (IOException e) {

​				e.printStackTrace();

​			}

​			return "sucess";

​		} else {

​			return "redirect:index.jsp";

​		}

​	}

设置的上传的文件大小是总文件的大小，所有文件加一起的大小

上传路径应该使用File.separator做分隔符，因为不同系统的分隔符号不同

文件对象.transferTo(File);来进行实现

Spring本身没有提供Jsr303的校验， Hibernate Validator 实现

JSR303校验

@Null 必须为null

@NouNull 必须不为null

@AssertTrue 必须为true

@AssertFalse 必须为false

@Min 必须为数字，且值最小为指定值

@Max必须为数字，且值最大为指定值

@DecimalMin Decimal的值

@DecimalMax Decimal的值

@Size(max,min) 元素必须在指定范围之内

@Digits(integer,fraction) 必须是一个数组，其值必须在一个可接受的范围之内

@Past 被注释的元素必须是一个过去的日期

@Future 被注释的元素必须是一个将来的日期

@Pattern(value) 被注释的元素必须符合正则表达式

@NotEmpty 被注释的元素不能为空

@Length 被注释的字符串的大小必须在指定范围之内