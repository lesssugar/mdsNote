# 外部文件加载

```js
defer	//延时加载js文件，遇到先后台下载，不会阻止dom元素的渲染，js文件按顺序执行,DOMContentLoaded
async	//异步加载脚本，脚本加载完毕就进行调用，不考虑顺序
<script defer="defer" async="async">
//如果js的逻辑需要dom元素参与，使用defer，否则使用async，不确定用defer
    
DOMContentLoaded事件只要页面DOM元素加载完毕后就会触发，onLoad()在样式表，图片，Flash都加载完毕后触发
```
 # 逻辑运算

```js
1、只要“||”前面为false，无论“||”后面是true还是false，结果都返回“||”后面的值。
2、只要“||”前面为true，无论“||”后面是true还是false，结果都返回“||”前面的值。
3、只要“&&”前面是false，无论“&&”后面是true还是false，结果都将返“&&”前面的值;
4、只要“&&”前面是true，无论“&&”后面是true还是false，结果都将返“&&”后面的值;
```

