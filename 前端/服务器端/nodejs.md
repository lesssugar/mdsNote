

# 特点

可以在服务器端运行的js代码，不仅限于浏览器操作，谷歌的v8引擎，事件驱动非阻塞和异步I/O模型，请求的处理过程，请求，响应，服务器代码，数据库I/O其中最大的问题就在IO，响应通过加大带宽，服务器代码可以优化，服务器时多线程的，每有一个请求，就会创建一条线程为其服务，但在IO的时候就需要进行等待，这种情况就叫做阻塞，node.js就可以解决该种情况，单线程，在和数据库IO交互时，线程先去处理别的请求，但单线程无法承受过大访问

# HelloWorld

```java
先到ws中配置node，选择文件位置，然后将	Coding assistance for Node.js打开，开启语法提示
node helloWorld.js
```

## 官网

```js
http://nodejs.cn/
```

#Common Js

为了弥补js没有标准的缺陷，并且希望js可以在各处运行，定义简单，包含

- 模块引用
- 模块定义
- 模块标识

一个js文件就是一个模块，在node中通过 require引入外部的模块

```js
var md = require("");	//写相对路径必须以	./或者../开头，建议以后写别的也养成该标准
console.log(md.x);		//每一个模块都是运行在一个方法中，所以是不能调用其中的函数的
export.x = "x";			//如果想向外部暴露成员，通过export；
module.exports = export	//两者本质无区别，但是可以一次导出多个
moudle.exports = {
    name: name,
    age : age;
    sayHello : sayHello 
}

//如果定义了一个成员自己使用还想让别人调用
var age = 10;
exports.age = age;
```

##模块分类

```js
//用户自定义模块
var math = require("./math");
//核心模块，node.js自带的模块
var fs = require("fs");
```

## 全局对象介绍

```js
var a = 10;
console.log(global)；类似js中的window，全局中的变量，方法都会进行记录,一个模块中的a,global中没有，说明a仅在一个函数内部，不是全局变量，去掉var,	a = 10;就成了全局变量
```
## node模块化原理

```js
//通过argument查看参数，node将每个模块添加到
function(exports,require,moudle,_filename,_dirname){
    ...
}函数中，所以内部的变量都是局部变量
```

# package

## 包结构

```js
package.json	//描述文件，必须
bin				//可执行的二进制文件
lib			//js代码
doc			//文档
test		//单元测试
```

```js
//node自带的package.json
{
  "version": "5.6.0",	//版本
  "name": "npm",	//名字
  "description": "a package manager for JavaScript",	//描述
  "keywords": [		//关键字
    "install",
    "modules",
    "package manager",
    "package.json"
  ],
  "preferGlobal": true,	//
  "config": {
    "publishtest": false
  },
  "homepage": "https://docs.npmjs.com/",	//主页
  "author": "Isaac Z. Schlueter <i@izs.me> (http://blog.izs.me)",	//作者
  "repository": {	//仓库地址
    "type": "git",
    "url": "https://github.com/npm/npm"
  },
  "bugs": {
    "url": "https://github.com/npm/npm/issues"
  },
  "directories": {
    "bin": "./bin",
    "doc": "./doc",
    "lib": "./lib",
    "man": "./man"
  },
  "main": "./lib/npm.js",
  "bin": {
    "npm": "./bin/npm-cli.js",
    "npx": "./bin/npx-cli.js"
  },
  "dependencies": {	//依赖
  },
  "bundleDependencies": [	//
  ],
  "devDependencies": {	//开发依赖
  },
  "scripts": {	
    "dumpconf": "env | grep npm | sort | uniq",
    "prepare": "node bin/npm-cli.js --no-timing prune --prefix=. --no-global && rimraf test/*/*/node_modules && make -j4 doc",
    "preversion": "bash scripts/update-authors.sh && git add AUTHORS && git commit -m \"update AUTHORS\" || true",
    "tap": "tap --timeout 300",
    "tap-cover": "tap --nyc-arg='--cache' --coverage --timeout 600",
    "test": "standard && npm run test-tap",
    "test-coverage": "npm run tap-cover -- \"test/tap/*.js\" \"test/network/*.js\" \"test/broken-under-*/*.js\"",
    "test-tap": "npm run tap -- \"test/tap/*.js\" \"test/network/*.js\" \"test/broken-under-*/*.js\"",
    "test-node": "tap --timeout 240 \"test/tap/*.js\" \"test/network/*.js\" \"test/broken-under-nyc*/*.js\""
  },
  "license": "Artistic-2.0"
}
//版本号前面的符号，~,^,*,版本号1.0.0，补丁更新，最后一位改动，小版本更新，第二位改动，大版本的更新第一位改动，如果你仅接受补丁改动~,接受小版本的改动^，接受大版本的改动*
```

# NPM(node package manager)

```less
npm -v
npm install npm@latest -g	//nmp更新
npm version	//查看所有模块的版本
npm search xxx	//搜索包名，搜索math查询到和math有关的包
```

## 包的安装与删除

```less
npm init //先创建一个package.json文件，名字不要有大写字母
npm init --yes	//只创建文件，不回答问题
npm install xxx	//根据包名进行安装,会出现node_moudles文件夹,里面就是下载的模块
npm i math	//install的简写
npm i math --save  ****重要，下载一个包的同时将他添加到依赖中***
npm i //会根据依赖下载包，下载的项目都需要先install
npm remove math	//移除一个包		r 简写
npm install 包名[@1.10] [] //全局安装，一般都是一些工具，不是项目中用的，用的少
					  -g
					  -dev
					  --save-dev
					  --production
npm outdated		//查看包的更新
npm update [name]	//更新某个包或者所有包
```

##包的调用

```less
var math = require("math");	//不是我们自己定义的模块，不用加./或../
```

# CNPM

```js
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

# Buffer(缓冲区)

类似于数组，操作方式也类似数组，原生js数组性能相对来说较差，而且数组不能存取二进制文件，而buffer就是专门用来存储二进制数据的数组

```js
var str = "Hello node!"
var buffer = Buffer.from(str);
console.log(buffer);	//字符串被转化成了2进制数据，但是在显示时都是以16进制的形式显示的
str.length	和	buffer.length	是不一样的.汉字字节数比较多
```

## 创建

```js
var buf = Buffer.alloc(10);
buf[0] = 88;	//10进制，会被进行转换
buf[1] = 0xaa;
buf[10] = 10;	//类似数组，超出下标但不报错，长度不可改变，内存分配连续10长度空间

console.log(buf[1])	//打印170,我们获取进行打印显示的都是十进制，如果想看16进制
console.log(buf[1].toString(16))	//转化为16进制的字符串

//想要将一个buffer转化为字符串
buf.toString();	//即可
```

#FS(file system)

```js
//获取文件	sync同时同步的方法，会阻塞服务器运行
var fs = require("fs");
var fd = fs.openSync("helloworld.txt","w");
console.log(fd)	//fd返回3是一个文件编号，意思我们可以操作3号文件即可
fs.writeSync(fd,"hello,node",[position],[encoding])	//想该文件中写入信息,position,写入的起始位置，encoding,默认UTF-8
fs.closeSync(fd);		//对文件操作完毕后应该对文件进行关闭，否则服务器会打开很多文件

//异步操作,console.log(arguments),第一个为error，第二个是fd
fs.openSync("helloworld.txt","w"，function(err,fd){
    if(!err){
        fs.write(fd,"Hello Node",function(err){
            if(!err){
                fs.close(fd,function(err){
                    if(!err){
                       
                    }
                })
            }
        })   
    }else{
        console.log(err);
    }
});
```

## 流读写

```js
ws.once('open',function(){	//once和on类似，但只绑定一次，效率稍好
    
})
ws.once('close',function(){	//once和on类似，但只绑定一次，效率稍好
    
})
var ws = fs.createWriteStream("hello3.txt");
ws.write("通过流写入文件");
ws.write("第二次写入");
//ws.close();	//关了流可能在内容没写完的情况下，就关闭了连接，烧写入东西
ws.end();	//会在流写入完成后再进行关闭
```

## 文件操作

```js
var fs = require("fs");
fs.readFile("hello1.mp3",function(err,data){
    if(!err){
    	//返回的data就是buffer格式
        fs.writeFile("hello2.mp3",data,function(err){
            if(!err){
               console.log("文件上传成功!")
            }else{
               console.log(err);
            }
        });
    }else{
        console.log(err);
    }
})
```

## 大文件读取

```js
var rs = fs.createReadStream("a.mp3");
var ws = fs.createWriteStream("a2.mp3");
rs.on("data",function(data){		//读完会自动关闭
    ws.write(data);
});
rs.once("close",function(){
    //读取流关闭后，写入流准备关闭
    ws.end();
})


//上面的代码麻烦，简写
var rs = fs.createReadStream("a.mp3");
var ws = fs.createWriteStream("a2.mp3");
rs.pipe(ws);	//将可读流连接到可写流上，结束后关闭流
```

## 常用方法

```js
fs.existsSync("path");	//验证文件是否存在
fs.stat("a.mp3",function(err,stats){});	
var stats = fs.statSync("a.mp3",function(){});	//获取文件的状态
fs.unlink("a.mp3",function(){});	//删除一个文件
fs.readdir(".",function(err,files){})	//获取文件夹下有哪些文件
fs.truncateSync("a.mp3",10);	//截断文件保留文件的前10个字节
fs.mkdir("hello")	//创建文件夹
fs.rmdirSync("hello")	//删除文件夹
fs.renameSync("hello","fly")	//重命名，写路径也有剪切功能
fs.watchFile()
```

