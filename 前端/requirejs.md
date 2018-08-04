# 优点

- 异步加载文件，不会因为加载一个js文件停止页面的渲染
- 需要写很多行的

# HelloWorld

 ```javascript
define('helper',['jquery'],function($){
    return {
        trim: function(str){
            return $.trim(str)
        }
    }
});
 ```

# BaseUrl

下载文件是以该路径为基本路径，如果不设置，默认路径是html文件本身

当我们设置了data-main后

```javascript
<script data-main="js/app" src="js/requirejs.js" ></script>
//不定义路径底下模块在引用的时候回找不到路径

//在一个文件中写上该属性，该js文件中所有引入模块的路径就是基于这个
requirejs.config({
	baseUrl : '/canvas/js',
    paths : {
    	'jquery' : 'lib/jquery'   
    }    
})
//但有些文件可能引用了但是不放在baseUrl下，比如jquery等文件,这时候配置path,，相对于
'jquery' : [
    '',
    'lib/jquery' 
]	//可以填写数组，可以在第一个文件填写cdn，如果第一个路径找不到则会加载第二个文件
```

# 不支持的库怎么处理

## modernizr.js



## bootstrap

