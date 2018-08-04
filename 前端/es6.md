#变量

var的缺点，可以重复定义，无法限制修改，没有块级作用域，仅识别函数，所以迭代的时候要在中间垫一层function，可以先使用后申请，有变量提升

var中的	= 0不是java中的int i = 0，这里是一个对象，所以循环完，所有的都会指向一个值，一个值可以指向一个一个函数，本质上都是一样的

let 变量

const 常量 两者都不能重复定义，有块级作用域，可以直接用于迭代

# 箭头函数数组展开

```js
var show = （）=》{}
如果只有一个retrun{}可以省略  var show = a => a*2;
show(...arr);
let arrs = [...arr1,...arr2] 也可以通过这种方式对数组中的参数全部进行展开
```

# 默认值与解构赋值

```js
默认参数 function(a,b=10,c=6){} 如果没写该参数则使用默认值
解构赋值 	左右结构一样，不能分开赋值，右边的应该具有iterator，数组，json
let [a,b,c] = [10,20,30]	let {a,b} = {a:11,b:'ccc'}	前后名字必须一样
let [{a,b},[n1,n2],num,str] = [{a:1,b:'c'},[10,'cc'],18,'str']
let [JSON1,Array1,num,str] = [{a:1,b:'c'},[10,'cc'],18,'str']可以将一组元素不拆分
都可以，解构赋值居右较高的可拆分性和粒度可控性，解构赋值支持默认值，但只有右边对应值为defined时，才有效，null无效 ===判断的
字符串的解构赋值 let [a,b,c,d,e] = "hello"	
除了undefined和null其他的值会被先转化成一个对象.toString()所以可以结构成功	[x,y] = [y,x]变量的交换	
```

# 新增数组函数

```js
reduce()	汇总
数组		arr.includes()	let arr = [65,30,99,54]		map映射
let newArr = arr.map(item => item > 60?'及格':'不及格')
let num = arr.reduce((temp,item,index)=>temp+item)
temp上一次计算的出的值 item是本次计算的元素 index下标
let arr2 = arr.filter(item => item>50)
将符合规则的元素留下，不符合的进行过滤，返回true和false
arr.forEach(item => alert(item))	迭代
```

# 新增字符串函数

```js
字符串
新方法
repeat(n)	返回一个重复了n次原字符串的字符串
includes（'a'）       startsWith("a")		endsWith('a')	for(let p of str)迭代字符串	
```

# 方法尾调用

```js
方法.name 获取方法名	尾调用优化 return g(x)为使用方法，方法正常使用栈堆叠的方式执行，如果调用的g(x)栈帧中还需要前面方法的参数是，那么g(x)中就会保存多余的信息，尾调用的话，使用的是一个栈帧，所以就可以减少栈内存，达到少量的优化
```

# 字符串模板

```js
var x = ` <h1> </h1>`;	//可以换行
let a = 'yu';
let name = 'wang${a}qiang';

eval 将一个对象字符串转化为一个对象
```

# 面向对象

```js
class User{
	constructor(name,age){
		this.name = name;
		this.age = age;
	}
	showName(){
		alert(this.name)
	}
	showAge(){
		alert(this.age)
	}
}
let user = new User('小王',25)
user.showName()
user.showAge()
继承		调用属性需要写this关键字
		class VipUser extends User{
			constructor(name,age,level){
				super(name,age);
				this.level = level;
			}
			showLevel(){
				alert(this.level)
			}
		}
		let vipUser = new VipUser("小王",25,3);
		vipUser.showName()
		vipUser.showAge()
		vipUser.showLevel()
```

#Json对象

```js
let jsonStr = encodeURIComponent(JSON.stringify(jsonT));
JSON.stringify(jsonT)将JSON对象转化为字符串，encodeURIComponent将字符串对string的json串进行编码
http://www.w3school.com.cn/p 1/	|	http%3A%2F%2Fwww.w3school.com.cn%2Fp%201%2F
let jsonObj = {a,b}	名字与json中的对应位置一样时，可以只写一个{a:a,b:b}
```

#Promise	承诺

异步:操作之间无关，可以同时进行多个操作，ajax不异步，请求没回来，页面不能操作，代码复杂

同步:多个操作一起发生，只能执行一个操作，代码简单

可以用同步的方式写异步的代码.多个promise对象基本一样，则通过创建方法创建promise对象	其操作有三种结果是，成功，失败，进行中，结果只能根据异步操作的结果判定，其他操作无法影响，所以叫承诺

```js
		let arr = {"a":1,"b":2}
function createPromise(url){
		return new Promise(function(resolve,reject){
			$.ajax({
				type:"get",
				url:url,
				async:true,
				dateType :'json',
				success(arr){
					resolve(arr)
				},error(err){
					reject(err)
				}
			});
		});
		p.then(function(arr){
			alert('a')
		},function(err){
			alert('b'+err)
		})
}
```

##简写

```js
通过promise对象监控发送的ajax请求，resolve(arr)，reject(err)，成功失败接管到底部的方法当中，p.then
		Promise.all([
			createPromise('data/arr.txt'),p2
		]).then(function(arr){
			let [o1,o2] = arr;
			alert(o1)
			alert(o2)
		})
将全部的promise对象的值封装成一个数组，可以在底部进行解析，对所有
高版本ajax自带jQuery支持
let promise =  $.ajax（{url;'',dataType:json}）
返回的值就是一个promise对象
		Promise.all([
			$.ajax({url：‘in.txt’,dataType:'json'})
		]).then(function(arr){
			let [o1,o2] = arr;
			alert(o1)
			alert(o2)
		},function(err){
})
在不影响异步的情况下简化了写法
```

#generator	-生成器

普通函数-一直执行完毕

generator-可以暂停

```js
		function *show(){
			alert('a')
			yield;
			alert('b')
			alert('c')
		}
		let genObj = show();
		genObj.next()
		genObj.next()
let c = yield = 10 左边为输入参数，右边是返回参数， 
```

执行方法获得一个对象，相当于线程的start()

当next()会执行直到碰到yield，可以在请求数据的时候让方法进行暂停，原理相当于将方法内部进行分割成多个小函数，可以在next中传递参数

let num =  yield     let genObj = show(); show.next(10);	show.next(5);

num = 5 yield的部分属于前面执行的，直到方法暂停，而赋值是在后半部分

yield 5;用于返回值	第一步返回 {value : 5,done : false}，

最后一步返回{value:undefinded,done:true}

value用于记录返回的值

done说明函数是否执行完毕

最后一步没有yield，想返回值通过	return进行返回

let 参数 yield = 返回值
