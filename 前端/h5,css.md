#基础

attr是dom对象的attriobutes对象,而prop是和attruibutes的同级属性，包含的是预定义属性，自定义属性不包含，无法修改

将图片宽度写成100% 写成块元素比较好 高度auto

<iframe src="" name=""></iframe>内联框架 不推荐使用 name可配合target使用。意义为在制定内联框架中打开新界面

## 表单

```html
<input name="" type="" > type类型共包括 text password radio checkbox submit reset button file email url number range serch testarea hidden
select option 下拉选框
label配合id使用             readonly disabled placeholder 
required 输入文本不允许为空
js需要验证input不能为空的写这个就行了
```

# 选择器

```html
p:first-child{}p元素且是父元素第一行第一个子元素。不是p中第一个子元素。
p:last-child{}
p:nth-child(n){}选中任意为第n个子元素的p元素 n=even 偶数行改变 n=odd 奇数行改变
内联元素脱离文档流后会变成块元素
```

# 特殊属性

```css
text-shadow:blue 10px 10px 10px;颜色 x轴位移 y轴位移 模糊半径 可以用逗号分开指定多个阴影
letter-spacing 字符间距离 word-spacing 单词间距离
visibility:hidden;隐藏的元素占据位置。display:none;隐藏的元素不占据位置
文档流位于网页最底层。网页中的元素未进行设置时就在文档流里 
块元素在文档流中默认宽度为父元素的100% 是auto
如果一行里容不下所有的浮动元素，则浮动元素自动换到下一行。浮动元素不会超过他的兄弟元素 最多在一行
浮动元素不会盖住文字 文字会自动环绕在浮动元素周围 可以通过浮动设置文字环绕图片的效果
通过z-index设计层级 可以指定一个正整数作为其值 该数会成为该元素层级 层级越高越优先显示
数字大的层级高 没有开启定位的元素不能使用z- index 父元素的层级再高 也不会盖住子元素
设置元素的透明背景 opacity 填写0-1 0完全透明 1完全不透明 0.5半透明 ie8一下浏览器不支持
用 filter:alpha(opacity)=50; 和透明度0.5一样 从0-100之间的值 ietester无法测试
两个一起设置
boder-spacing 边框间距离
border-collapse:collapse; 设置表格边框合并
nth-of-type()even odd 双数单数记住
表单的name属性是必须的
selection 的默认选择项是由selected=“selected” 而不是checked
ie6对png格式 兼容性不好。可以用png8 代替 png24格式 使用ps图像存储为web所用格式转换为png8
ie8 placeholder无效。需要用JS处理
cursor 更改按钮移入的光标显示形式 pointer 小手 move狮子移动光标 crosshair十字线
相对定位是以原先自己的位置为基准的，而绝对定位是以浏览器或者最近的一个开启了定位的父级元素为基准
contentable="true" 用户可以编辑div内的内容
```

# 动画效果

```css
变形：transform:; translate(x,y); 平移
   scale() 放大 填写放大的倍数
   rotate(reg)旋转
  skew()倾斜函数
要写4种兼容 -o- -webkit- -moz-
过度 transition:x,y,z,& ; x:要过度的属性 y过渡时间 z：linear &:延迟时间
需要hover等进行触发
动画:@keyframe 动画名{
填写动画的属性
}
animation:;调取动画 同样四个属性 动画名称 一次播放时间 linear infinite 无限播放
也可以
填写播放次数
```

# 响应式布局

```css
<meta name="viewport" content="width=device-width, initial-scale=1">
@media all{
}
<link rel="stylesheet" type="text/css" href="" media="screen"/>
如果写了媒体特性多个在交界点有冲突，width:800，那么后面的生效，后者覆盖前者
或者通过link进行规定引入的文件时什么媒体类型的
媒体特性
@media only screen and (min-width: 100px) and (max-width: 640px) {}
其中min-width就属于媒体特性，所以归类是分为媒体类型和媒体特性等
最常用包括 max-width min-width 
不常用包括 device-width,device-height,width,height,orientation横屏或者竖屏，portrait/landspace，resolution分辨率，color每种色彩的字节数,color-index色彩表中的字节数
媒体查询
```

```css
设备宽高 device-width device-height
渲染窗口的宽与高 width height
手持设备的方向 orientation
设备的分辨率 resolution
media="only screen and (max-width:640px)"
only关键字 用来指定某种特定的媒体类型，可以用来排除不支持媒体查询的浏览器。用于解决某些浏览器哦仅支持媒体类型却不支持媒体查询的浏览器，为了这些浏览器依然对媒体类型进行兼容，使用only关键字进行兼容
not 关键字 
@media not print and (max-width:500px)除了打印机以外的媒体设备且大小大于500px的设备，后面的值同样取反
写在link里，如果屏幕大小超过640px，则引用的css文件无效
@media only screen and (x) {
 x    max-width:640px 最大640px
min-width    max-height       min-height
and连接符如果使用or则使用  ， 都好进行选择
可以写在css文件中，and后面还可以继续写and多个条件进行设定
当最小的时候，将中间模块的宽度设计成和屏幕大小一样的
```

# 弹性布局

```css
flex:1; 	//将父元素中的剩余位置全部占据
display:flex;  display:inline-flex;  display:-webkit-flex;  
//父元素添加,该情况下默认的子元素横向排列
不想压缩在子元素上添加 flex-shrink :0; 
```

## 排列方式

```css
flex-direction: row | row-reverse | column | column-reverse;
- row（默认值）：主轴为水平方向，起点在左端。
- row-reverse：主轴为水平方向，起点在右端。
- column：主轴为垂直方向，起点在上沿。
- column-reverse：主轴为垂直方向，起点在下沿。

flex-wrap: nowrap | wrap | wrap-reverse;
```

## 横向排列方式

```css
 justify-content: flex-start | flex-end | center | space-between | space-around;
排序方式，排在左侧，右侧，中心，两侧靠边，中间空间平分，空间全部平分
align-items: flex-start | flex-end | center | baseline | stretch;
元素在纵轴排列的顺序 column排列方式才能进行排列
align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
align-content: flex-start | flex-end | center | space-between | space-around | stretch;
flex-grow: 1; /* default 0 */1就是占据剩余的全部空间
```

# 多字...代替

```css
overflow:hidden
whitespace:nowrap
text-overflow:ellipsis
```

# Canvas

canvas建议成对出现，默认高宽300，150,默认颜色和body背景颜色一样

## HelloWorld

```html
<canvas id="test" width="" height="">	//不能使用css为canvas指定高宽
	<span id="">
		您的浏览器不支持画布元素，请更换浏览器<a href=""></a>
	</span>			
</canvas>
```

```js
		window.onload = function() {
			var canvas = document.querySelector("#test");
			//调取画笔
			if(canvas.getContext("2d")){
				var ctx = canvas.getContext("2d");
				//设置画笔的颜色				
				ctx.fillStyle = "blue";
                 //设置线的粗细
				ctx.lineWidth = 50;
                 //样式
				ctx.lineJoin = "bevel";
				//不带边框的矩形,偏移,大小
				ctx.fillRect(10,10,100,100);
				//设置画笔的颜色
				ctx.strokeStyle = "black";
				//带边框的矩形,默认边框量像素,如果想画一像素的边框，写0.5
				ctx.strokeRect(10.5,10.5,100,100)
				//透明矩形，覆盖，会留下边框,.5会留下一般边框,整数会留下1像素边框
				ctx.clearRect(10.5,10.5,100,100)
			}
		}
        //后画的会覆盖先覆盖的
        //lineJoin bevel，斜角 round，圆角 miter，默认直角
```
