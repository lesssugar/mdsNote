#创建对象兼容性

		var xmlhttp;
		if(window.XMLHttpRequest) {
			//  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
			xmlhttp = new XMLHttpRequest();
		} else {
			// IE6, IE5 浏览器执行代码
			xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
		}
#基本使用

```html
//发送post请求时需要添加
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");

document.querySelector('input').onclick = function() {
	var xhr = new XMLHttpRequest();
	//请求行 
	xhr.open('get', 'xxx.html');
	//请求头 参数1键名 参数2 值
	xhr.setRequestHeader('h', 'gg')
	//请求主体
	xhr.send(null);
	xhr.onreadystatechange = function() { // 状态发生变化时，函数被回调
		if(xhr.readyState === 4) { // 成功完成
			// 判断响应结果:
			if(xhr.status === 200) {
			// 成功，通过responseText拿到响应的文本,:
				return success(request.responseText);
			} else {
				// 失败，根据响应码判断失败原因:
				return fail(request.status);
			}
		} else {
			// HTTP请求还在继续...
		}
	}
}
```

#可返回的格式

html	使用简单格式清晰，单需要创建大量html页面

xml		解析getElementByTagName[0].nodevalue	解析复杂

json		轻量级，语法严谨，以responseText的形式返回

# JSON语法

```html
var jsonObject = {
	"name" : "wangyuqiang",
	"age" : "18",
	"info" : function(){
		alert(a)
	},
	"books" : ["a","b"]
}
```

# 获得json的方法

```java
//可以把字符串转换为一个本地的代码来进行执行，那么就可以将一个json的string转换为json对象使用
var user = {'name':'w','age':18}
var userObj = eval(user)
```



# 页面遍历

```html
var $data = $(data);
$data.each(function() {
	$("ul").append("<li>用户id:"+this.id+"用名:"+this.name+"用户年龄"+this.age+"</li>")
})
对于可能有 ""符号的字符串，需要  .replace("\"","")处理
```

#jQuery使用ajax

```html
$(document).ready(function() {

	$.ajax({
		beforeSend: function() {
			alert("before");
		},
		type: "get",
		url: "userServlet",
		data: "username=" + username + "&age = " + age,
		dataType: "text",
		async: true,
		success: callback,
		error: errorFunction
	});
})
function callback(data) {
	if(data == "true") {
		alert("该用户名已存在")
	} else {
		alert("该用户名可以使用")
	}
}
```

##$.load

```html
$(selector).load(url,data,complete);
$.get("url"，"data"，funciton(){
	$(selector).html(data);
})	//load的作用基本等同于下面的代码
```

# 将表单数据拼装为json字符串

```javascript
var queryString = $inputs.serialize();
```

#FastJSON

```java
toJSONString(Object obj);
JSON.toJSONString(user,true);
JSON.toJSONString(users,SerializerFeature.WriteMapNullValue) 该参数作用为输入null值，参数共有
SerializerFeature.WriteDateUseDateFormat 将时间对象格式化
JSON.toJSONStringWithDateFormat(date,"yyyy-MM-dd HH:mm:ss.SS");将时间格式化为自己规定的格式
QuoteFieldNames 输入JSON字段名时使用双引号
JSON.toJSONString(list, SerializerFeature.UseSingleQuotes);
用单引号输出字符串
user, SerializerFeature.WriteClassName 第一个键值对为 @Type:class
WriteMapNullValue 输入null值
WriteNullListAsEmpty 将值为null的list字段输出为[]
WriteNullStringAsEmpty 将值为null的String字段输出为""
WriteNullNumberAsZero 将值为null的数字字段输出为0
WriteNullBooleanAsFalse 将值为null的布尔类型值输出位false
SkipTransientField 忽略transient字段，默认即忽略
PrettyFormat 格式化JSON字符串，默认不格式化
ajax向前台以JSON格式进行数据传递时，若返回的是中文字符串，则会出现乱码。
RequestMapping(produces={"application/json;charset=UTF-8"})
然后出现406错误
```

# JackSon

## jar包

DataBind,Core,Annotations三个jar包

## 使用

```java
        ObjectMapper objectMapper = new ObjectMapper();
        User user = new User("name", 18);
        String s = objectMapper.writeValueAsString(user);	//将一个对象解析为JSON字符串，其中每一个获取的属性都应该有一个getter开头的方法，返回值就会被记录在JSON的字符串中
```

## 忽略某个属性

```java
com.fasterxml.jackson.annotation.JsonIgnore
    @JsonIgnore
    public String getCity(){	//放在某个get方法上，则会忽略该属性
```



