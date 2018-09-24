# 安装

```bash
npm init -y
cnpm install webpack webpack-cli -D
```

# 使用

```js
写一个配置文件 webpack.config.js

let path = require('path');
let HtmlWebpackPlugin = require('html-webpack-plugin');
let CleanWebpackPlugin = require('clean-webpack-plugin');
let webpack = require('webpack');
module.exports={
	entry:['./src/resources/js/index.js','./src/resources/js/hello.js'],	//打包入口
	output:{
		filename:'main.[hash:8].js',		//添加8位的hash用于清除缓存
		path:path.resolve('./build')		//路径
	},	//出口
	devServer:{
		contentBase:'./build'，
        port:3000,
        compress:true, //服务器压缩
        open:true,	//自动打开浏览器
        hot:true	//热更新
	},	//开发服务器
	module:{},		//模块
	plugins:[],		//插件
	mode:'development',	//模式
	resolve:{}		//配置解析
}

npx webpack就会按照配置文件进行js文件的打包
cnpm install webpack-dev-server -D,安装类就不用每次都进行上面的操作，
  "scripts": {
    "build": "webpack",
    "start":"webpack-dev-server"
  },
配置该脚本运行npm run start 即可，会在内存中生成入口文件，不需要创建该文件夹，即可访问该资源
```

## html-webpack-plugin -D

```html
cnpm install html-webpack-plugin -D	
plugins:[
		new HtmlWebpackPlugin({
			template:'./src/index.html',
			title:'title',
			minify:{
				removeAttributeQuotes:true,	//去除掉属性的双引号
				collapseWhiteSpace:true,	//折叠空行
				hash:true					//js后面带遗传哈希码，用于清除缓存
			}
		})
	],		//插件
<title><%=htmlWebpackPlugin.options.title%></title>	//将属性赋值到页面上
hash每次会生成一个新的js文件，旧文件未去除，使用clean-webpack-plugin插件
```

## clean-webpack-plugin

```html
cnpm install clean-webpack-plugin -D
new CleanWebpackPlugin(['./build'])	//添加一个插件对象
```

## 热更新插件

```bash
webpack自带插件
	devServer:{
        hot:true	//热更新
	},	//开发服务器
	plugins:[
		new webpack.HotModuleReplacementPlugin();
	],
	在js的入口文件写如下代码
	if(moudle.hot){
        moudle.hot.accept();		//这样的话代码有更新会进行热更新该模块
	}
```

## 多入口配置

```js
//有可能有多个入口网页与文件
//entry写数组会把多个js打包到一个html上，如果希望多个html
entry{
    index:'./src/index.js',
        a：'./src/a.js'
}，//一个出口会多个文件会打包到一个文件，配置多出口,文件名更换为[name]，但是一个页面加载多个js
output:{
		filename:'[name].[hash:8].js',		//添加8位的hash用于清除缓存
		path:path.resolve('./build')		//路径
},	//出口,多个页面加载各自加载自己的js
plugins:[
		new HtmlWebpackPlugin({
             filename:'a.html',
			template:'./src/index.html',
			title:'title',
			minify:{
				removeAttributeQuotes:true,	//去除掉属性的双引号
				collapseWhiteSpace:true,	//折叠空行
				hash:true					//js后面带遗传哈希码，用于清除缓存
			},
            chunks:['index']	//规定打包的a.html引入index.js文件
		}),
    	new HtmlWebpackPlugin({
             filename:'b.html',
             chunks:['a']		//b.html引入a.js
			template:'./src/index.html',
			title:'title',
			minify:{
				removeAttributeQuotes:true,	//去除掉属性的双引号
				collapseWhiteSpace:true,	//折叠空行
				hash:true					//js后面带遗传哈希码，用于清除缓存
			}
		})
	],		//插件
```

# Css模块

```js
//js文件中引入	import './index.css'
//解析除js文件外的格式可能需要加载不同的loader
cnpm install style-loader css-loader less less-loader -D
//需要什么加载器就安装什么加载器即可
module:{
    rules:[
        {
            test:/\.css$/,use:[
                {loader:'style-loader'},
                {loader:'css-loader'}
            ]
        },
        {
            test:/\.less$/,use:[
                {loader:'style-loader'},
                {loader:'css-loader'},
                {loader:'less-loader'},
            ]
        },
        	
    ]
},		//模块,都会放到一个js文件中，js文件会过大，应该对样式进行抽离到一个css文件中
    //extract-text-webpack-plugin@next,有可能被mini-css-extract-plugin取代
    //安装两个
```

## extract-text-webpack-plugin@next

```js
let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin');
let lessExtract = new ExtractTextWebpackPlugin('./css/less.css');
let cssExtract = new ExtractTextWebpackPlugin('./css/css.css');
plugins:[
    lessExtract,	//如果有多钟抽离文件名用这种形式，否则用下面哪种
    cssExtract,
    new ExtractTextWebpackPlugin({
		filename:'css/index.css'	//把抽离的样式抽离到哪里        
    })
],		//插件
module:{
    rules:[
        {
            test:/\.css$/,use:lessExtract.extract({
                use:[
                	{loader:'css-loader'}
                ]
        	})
        },
        {
            test:/\.less$/,use:cssExtract.extract({
            	[
                	{loader:'css-loader'},
                	{loader:'less-loader'},	//顺序要倒着写
          	    ]
            })
        },
    ]
},
```

## mini-css-extract-plugin

```js
let MiniCssTractPlugin = require('mini-css-extract-plugin');
plugins:[
    new MiniCssTractPlugin({
        filename:'./css/index.css'
    })
],		//插件
module:{
    rules:[
        {
            test:/\.css$/,use:[
                	MiniCssTractPlugin.loader,
                	{loader:'css-loader'}
            ]
        }
    ]
},
```

## 样式抽离热更新失效解决

```js
let lessExtract = new ExtractTextWebpackPlugin({
    filename:'./css/less.css',
    disable:true
});
module:{
    rules:[
        {
            test:/\.css$/,use:lessExtract.extract({
                fallback:'style-loader',	//开发的时候使用style-loader
                use:[
                	{loader:'css-loader'}
                ]
        	})
        }
    ]
},
```

## 样式不全部加载插件

```html
我们使用某个css样式文件，可能只需要其中一两个样式，如果加载整个文件浪费资源
cnpm install purifycss-webpack purify-css glob -D
let PurifycssWebpack = require('purifycss-webpack');
let glob = require('glob');
plugins:[
    new PurifycssWebpack({
        paths:glob.sync(path.resolve('src/*.html'));
    })
],		//插件
//这样的话会把没有用到的css进行剔除
```

## CSS自动添加前缀

```html
cnpm install postcss-loader autoprefixer -D
module:{
    rules:[
        {
            test:/\.css$/,use:lessExtract.extract({
                fallback:'style-loader',	//开发的时候使用style-loader
                use:[
                	{loader:'css-loader'}
				   {loader:'postcss-loader'}
                ]
        	})
        }
    ]
},
//需要写一个配置文件
```

```js
postcss.config.js
module.exports = {
    plugins:[
        require('autoperfixer');
    ]
}
```

# 文件拷贝插件（不常用）

```html
//文件拷贝到线上
cnpm install copy-webpack-plugin -D
let CopyWebpackPlugin = require('copy-webpack-plugin');
plugins:[
    new CopyWebpackPlugin([{
		from:'./src/doc',
		to:'public'		//将doc文件夹拷贝到build下的public文件夹
    }])
],		//插件
```

