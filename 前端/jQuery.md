#语法

```js
$(元素).事件(function(){
	$(元素).操作或者继续事件
})
```

jquery对象是DOM对象的集合 可以用遍历方式解析成DOM对象

#选择器

CSS全部选择器 外加  :eq  :gt  :lt   :header :visible :hidden 

#绑定事件

$（"元素"）.bind("事件",function(){

})

可以同时绑定多个元素用空格隔开

绑定的方法触发后会继续触发父节点绑定的事件 向上冒泡

阻止向上冒泡使用 参数.stopPropagation();	

建议使用on 或delegate()绑定事件

$("").on("父节点的子节点","事件"，function(){})

bind不能为还未出现的元素绑定事件

#特殊效果

```js
hover(function(){},function（）{})鼠标经过离开重复触发这两个事件。
toggle(function(){},function(){})鼠标点击循环触发几个函数
fadeIn fadeOut 元素的淡入与淡出
slideUp slideDown 元素高度的增加与减少 
都可以添加参数 （1000）意思1秒执行完毕
或者 slow fast 不建议，没感觉速度差很多
动画事件 animate({"":""},time,callback());
```

#样式操作

addClass()     removeClass()

.val() 获取一个元素的value属性的值

#节点的操作

```js
a.append(b); 将b插入到a的最后
a.appendTo(b); 将a插入到b的最后
a.prepend(b); 将b插入到a的最前
a.before(b);   将b插入到a的前面
a.after(b);   将b插入到a的后面
a.remove(); 移除a节点 同时移除事件
a.detach();  移除a节点 并不移除事件 不常用
.empty();      清空所有子节点
.replaceWith(x);  将节点换成x
.clone();        复制节点
.attr({"":""}) 属性
.removeAttr("");
.next();
.prev();
.siblings(); 其余的全部兄弟节点
.each();      
blur       失去焦点
focus	获得焦点
select();
var reg = ; 正则表达式
reg.test(str); 测试正则表达式 返回布尔类型的值
```

#正则表达式

```js
var reg = / /x;中间填写内容
x可以填写g全局匹配 i不分大小写进行匹配 m代表可以进行多段匹配
/china/; 代表匹配 为china 的字符串 此模式为简单匹配
通配符
^ 代表字符串的开始，$代表字符串的结束
\s 任何空白字符
\S 任何非空白字符
\d匹配一个数字字符
\D除了数字外的任何字符
\w 匹配一个数字下划线、下划线或者字母字符
\W任何非单字字符
.除了换行符外的任何字符
{n}匹配前一项n次
{n,}匹配前一项n次或者多次
{n,m}匹配前一项少则n次多则m次
```

#函数

```js
$.each(x, function(index,item)
遍历x每个元素为item
$("#hope_select").val([ emp.dId ]);
var name = $(this).parents("tr").find("td:eq(2)").text();
全选功能
$(".check_item").prop("checked", $(this).prop("checked"));
```

## offSet

```javascript
var offSet = $("#div1").offSet();	//获取一个Dom元素的offSet对象，相对于屏幕
offSet.left;	//获取距离左边的距离
offSet.top;		//获取距离上边的距离
offSet({left:100,top:50})
```

```javascript
var position = $("#div1").position();	//获取一个Dom元素的offSet对象，相对于父元素
position.left;	//获取距离左边的距离
position.top;	//获取距离上边的距离
```

## scroll

```javascript
$("div").scrollTop();	//元素内滚动条的位置
$("body").scrollTop();	//页面滚动条的位置，不兼容，有的是HTML
$(document.documentElement).scrollTop()+$(document.body).scrollTop();//兼容浏览器
$("html,body").scrollTop(300);	//取并集进行设置
```

## 平滑滚动

通过定时器，多次滚动滚动条，如果距离上边距离为0则取消，定时器

# 轮播图

第一张和最后一张添加一个暂时可以的元素，然后在滚动完完成后切换图片的顺序，这样就可以在用户未发现的情况下，进行顺兴的更换

# 插件

给$核心函数扩展方法

```javascript
//可以申请组件方法
$.extend({
	min: function(a, b) {
		return a < b ? a : b;
	},max : function(a,b){
		return a > b ? a : b;
	}
});
//调用申请的组件
$.max(10,12)
```
## jquery-validation

```javascript
<script src="js/jquery-3.1.1.js" type="text/javascript" charset="utf-8"></script>
<script src="js/jquery.validate.js" type="text/javascript" charset="utf-8"></script>
```

```javascript
	required: "This field is required.",
	remote: "Please fix this field.",
	email: "Please enter a valid email address.",
	url: "Please enter a valid URL.",
	date: "Please enter a valid date.",
	dateISO: "Please enter a valid date (ISO).",
	number: "Please enter a valid number.",
	digits: "Please enter only digits.",
	equalTo: "Please enter the same value again.",	//和某一个表表单的值必须相同,写选择器,id
	maxlength: $.validator.format( "Please enter no more than {0} characters." ),	//最长
	minlength: $.validator.format( "Please enter at least {0} characters." ),	//最短
	rangelength: $.validator.format( "Please enter a value between {0} and {1} characters long." ),
	range: $.validator.format( "Please enter a value between {0} and {1}." ),
	max: $.validator.format( "Please enter a value less than or equal to {0}." ),
	min: $.validator.format( "Please enter a value greater than or equal to {0}." ),
	step: $.validator.format( "Please enter a multiple of {0}." )
```
$("#myForm").validate();	开启校验

```javascript
	$("#myForm").validate({
		messages : {
			username : {
				required : "用户名不能为空",
				minlength : "用户名长度不能大于5"
			},pwd : {
				required : "密码不能为空",
				minlength : "密码长度不能小于5"
			}
		}
	});	//开启校验和定义校验提示信息
```
在表单上写required和对应要验证的属性

提示的信息都带有class属性error，如果想要修改可以.error为提示信息修改样式

## jquery-ui

根据index中的模仿引入css与js

然后复制对应的模块，使用对应的方法激活，

```javascript
var availableTags = [
	"ActionScript",
	"AppleScript",
	"Asp",
	"BASIC"
];
$( "#autocomplete" ).autocomplete({
	source: availableTags
});//搜索框的代码
```



## laydate

解压出来的文件需要整体一起倒入，

```javascript
<script src="js/laydate.js" type="text/javascript" charset="utf-8"></script>	//引入js
<script type="text/javascript">
	laydate.render({
		elem: '#test1' //指定元素
	});
</script>	//开启效果
```
