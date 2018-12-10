```bash
cnpm install stylus --save
cnpm install stylus-loader --save
```

## iconfont的使用

- 选择图片添加到购物车，添加到项目中，项目下载，将文件拿出放入到项目中
- vue main.js    import,修改iconfont.css文件，让他们指向对应的文件
- 页面中使用iconfont的class，然后写入对应的图标代码即可

```css
.header
	.left
		color: red
		font-size: 14px
	.right
```

# 定义变量

```
varibles.styl	文件
$bgColor = #fff	
import '~styles/varibles.styl'
```

# 多字...代替功能

```
ellipsis()
	overflow:hidden
    whitespace:nowrap
    text-overflow:ellipsis
    
@import '';
ellipsis()	//调用即可
```

