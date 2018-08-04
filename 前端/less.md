# 注释

```less
//		//不会编译到网页中
/**/	
<!-- -->	//会编译到网页中
```

# 变量

```less
<!--定义属性值-->
@color:red;
<!--定义属性-->
@m:margin;
<!--定义选择器-->
@selector:#warp;
*{
	color : @color;
	@{m} : 0px;
}
@{selector}{
	color:#000;
}
```

# 变量的延迟加载

如果一个块级作用域中定义了两个同名变量，按后定义的为基准，因为读取代码的时候是读取完一整块在解析

# 块嵌套

```less
*{
    margin:0;
    padding:0;
    #container{
        color:red;
        &:hover{//如果不想要两个之间有空格加&
            
        }
    }
}
//暂时使用coala进行编译
```

# 混合

```less
//对于重复性的css代码,不加括号该段代码也会编译到css文件中
.xxx(){
    color:red;
}
//调用
#{
    .xxx
}
```

## 添加参数的混合

```less
.xxx(@w：100px,@h:100px,@c:red){	//：默认值
    width:@w;
    height:@h;
    color:@c;
}
//这样就可以对外观大致相同但值不同的属性块赋值 
.container1{
    .xxx(100px,100px,red);
}
.container2{
    .xxx(200px,200px,black);
}
.container{
    .xxx(@c:#bfa;); //指定要输入的参数是哪个
}
```

# 模块化

```less
@import "./xxx.less"	//引用别的less模块
```

# 匹配模式

```less
.aaa(@_){
    width:100px;
    heigth:100px;
}

.aaa(A,@w){
    width:@w;
}
.aaa(B,@w){
    width:@w;
}
//外部调用模式的时候(A，100px)选择调用不同的参数，第一行是这两段中的重复代码，命名一样，意思是调用不同模式的时候都会先调用该段css
```

# 避免编译

```less
~""	//将一些css自带的函数放进去可以避免编译
```

