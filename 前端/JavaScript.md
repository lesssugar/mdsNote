语法

$(元素).事件(function(){

$(元素).操作或者继续事件

})

jquery对象是DOM对象的集合 可以用遍历方式解析成DOM对象

选择器

CSS全部选择器 外加  :eq  :gt  :lt   :header :visible :hidden 

绑定事件

$（"元素"）.bind("事件",function(){

})

可以同时绑定多个元素用空格隔开

绑定的方法触发后会继续触发父节点绑定的事件 向上冒泡

阻止向上冒泡使用 参数.stopPropagation();	

建议使用on 或delegate()绑定事件

$("").on("父节点的子节点","事件"，function(){})

bind不能为还未出现的元素绑定事件

hover(function(){},function（）{})鼠标经过离开重复触发这两个事件。

toggle(function(){},function(){})鼠标点击循环触发几个函数

fadeIn fadeOut 元素的淡入与淡出

slideUp slideDown 元素高度的增加与减少 

都可以添加参数 （1000）意思1秒执行完毕

或者 slow fast 不建议，没感觉速度差很多

动画事件 animate({"":""},time,callback());

addClass()     removeClass()

.html() 和 .text()的区别在于 前者可以操作标签如写入带有标签对的代码，后者只能操作文本

.val() 获取一个元素的value属性的值

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

正则表达式

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

获取的元素的validity属性值

表单设置了required 如果值为空 可以使用valueMissing 检测

u.validity.valueMissing 

typeMismatch 用于检测用户输入的类型是否符合 text，number等类型

patternMismatch 检测输入的值是否符合正则表达式 

tooLong 检测是否大于maxLendth 属性如果大于返回true说明太长了

rangeUnderflow输入的值相遇最小值

rangeOverFlow 输入的值大于最大值

stepMismatch 用step的规则进行检测不符合规则则返回true

u.setCustomerValidity 设置要提示的信息 如用户名不能为空

未成功暂不知道原因

var eNameReg = /(^[\u4e00-\u9fa5]{2,5}$)|(^[A-Za-z]{3,20}$)/;

正则表达式要用 /开头 /结尾，不是字符串，每部分用()圈起来

各个部分最好无空格，

$.each(x, function(index,item)

遍历x每个元素为item

$("#hope_select").val([ emp.dId ]);

var name = $(this).parents("tr").find("td:eq(2)").text();

全选功能

$(".check_item").prop("checked", $(this).prop("checked"));

# 属性获取

```js
data-自定义属性名
var x = node.dataset.xxx = 111;	//即可赋值
```



