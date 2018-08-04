vue精简版

var app = new Vue({})

vue的声明周期,共有create,mount,update,destory

对应的方法包括 beforeCreate(),created(),beforeMount(),mounted(),beforeUpdate(),updated()

beforeDestory(),destoryed()方法 mount对应的是元素的挂载过程

对象.$watch('num',function(){})

v-model 写在input上，可以将输入框中的值绑定修改一个值 v-model="name"

v-model.lazy 只有在Input框失去焦点时，才更新数据  v-model.trim 

v-model.number 会将输入的值转化为数值型 

只能在 input textarea 及select元素上使用 select值为0.1.2

v-if v-else-if v-else 

template标签包含多个元素一起进行v-if，不支持v-show

<li v-for="food in foods">{{food.name}}￥{{food.price * food.discount}}</li> 可以运算

foods: [{name: "葱",price: 10}, {name: "姜",price: 5}, {name: "蒜",price: 5}]

app.foodList.push({name: "椒",price: 8}) 可以插入元素

如果想对属性进行{{}} 则需要对属性添加 v-bind：对属性进行绑定

v-on: 绑定的是元素的事件

v-on = "{mouseenter : menter(),mouseleave : mleave()}"

v-on:submit.prevent

v-on:submit.stop 停止冒泡事件

v-on:keyup = "onKeyUp"

v-on:keyup.enter = "onKeyUp" 如果用户敲了回车键

@ 可以 代替v-on指令

v-on:click = "{{name = 1}}"

<form v-on:submit.prevent="onSubmit">...</form>

.prevent 修饰符调用 event.preventDefault()

{{ message.split('').reverse().join('') }}

methods : { show : function(){return this.name}}

computed : { show : function(){return this.name}}

computed所定义的方法不用写()

侦听属性 watch：{ }当监听的属性值发生变化时，执行回调函数

计算属性将计算式封装成 computed中的方法 

sum : function(){ }  avg = Math.round(this.sum / 3) 计算属性中的数据有缓存，methods 中的方法每次都会重新调用

[侦听器](https://cn.vuejs.org/v2/guide/computed.html#侦听器) ？

class绑定

class v-bind:class= {active:isActive,'textdanger':isDanger}

class v-bind:class = [active,sss]

class v-bind:class = "classObject"

可以直接定义数组 data:{ active : true ,'text-danger' : false}

方法 return {active:this.isActive && !this.hasError}

！null为true

template: '<p class="foo bar">Hi</p>'

<my-component class="baz boo"></my-component>

<p class="foo bar baz boo">Hi</p>

class绑定同样效果

<my-component v-bind:class="{ active: isActive }"></my-component>

内联样式

v-bind:style 在行内进行样式的绑定,会自动添加 -o, -webkit等属性

{color:showColor,fontSize: fontSize + 'px'}fontSize形式

或者直接引入一个对象的样式

styleArr : { color : 'red' ，fontSize : '50px'}

[styleArr,styleArr2] 以数组的方式绑定css样式

display: ['-webkit-box', '-ms-flexbox', 'flex']

会渲染最后一个被浏览器支持的值，本例中，如果浏览器不支持带前缀的属性，那么本例中的属性为  display : flex;

两个互相切换的模板使用相同的元素时，相同的元素不会被进行替换，节省开销，如果希望重新渲染为元素添加key="" 唯一值

v-show开销低v-if切换开销高

v-for和v-if一起使用，v-for具有更高的优先级

v-for="p in ps" {{p.id}}	in也可以用of

v-for="（p，index) in ps" {{p.id}}{{index}} 序号

(value，key，index) of pet  {{index}}:{{key}}:{{value}}

1个参数遍历值，两个参数遍历key,value，三个带下标

遍历的时候最好给一个唯一标识的 :key=""

Vue.component('my-component-name', { })

组件的注册，组件名使用 -进行名字的连接

注册好的组件可以使用在vue的根实例当中

局部注册

组件中的data标签写的是一个方法，返回的值具有和vue对象中data中的值一样的功能，可以在template中使用，多个组件具有相同的变量，在同一vue作用域下，值就会相互影响

vue对象需要在组件之后进行申请，一个vue对象进行注册的时候会对它的子元素进行检查是否为正常的标签或者组件，如果某个子元素为不存在的标签则会报错，所以需要先进行组件的注册在进行vue对象的注册

每使用一次组件，对应一个新的组件实例

data对应的是一个可以返回值得方法，而不是属性对，放置组件实例间的数据共享

组件通过 props: ['a'],获取外部元素 :a类似的值，组件内部{{}}

实现外部对组件的传参

局部组件的注册components:{ com : { template : '',methods {}}}

可以将组件的模板写在h5里然后通过选择器引用

数组操作的方法 push() pop() shift() reverse() sort() splice()

splice(index,i) 从i数组index的位置开始，删除一个元素，所以可以用来删除指定的元素

event.target.tagName获取触发事件的元素对象

数组中的值是一个对象，当vue中的元素指向数组中的元素时，如果数组中的值改变了引用地址，由于网页中渲染的元素还指向原来的元素地址，所以无法进行动态绑定，如果相对值进行更新

Vue.set(this.nums,2,80)

可以通过计算属性对数组的值进行监督

多选框，单选按钮和select绑定到数组

​      { text: 'One', value: 'A' },

​      { text: 'Two', value: 'B' },

​      { text: 'Three', value: 'C' }

如果输入的是One 想要传递

让radio等默认选中，将值给成和和他value一样的值即可

表单v-model .lazy  .number .trim

# 模块化

## 注册全局组件



## 注册局部组件

```java
		var TodoItem = {
			props: ['item', 'index'],
			template: '<li @click="handleTodoItem">{{item}}</li>',
			methods: {
				handleTodoItem: function() {
					this.$emit('delete', this.index)
				}
			}
		}
```

## 局部组件的使用

```java
		components : {
				TodoItem :TodoItem	
			},
```

## 注册全局组件

		Vue.component('TodoItem',{
			props : ['item','index'],
			template : '<li @click="handleTodoItem">{{item}}</li>',
			methods : {
				handleTodoItem : function(){
					this.$emit('delete',this.index)
				}
			}
		})
## 父子之间的通信

````java
this.$emit('delete',this.index)	//向父组件发送事件和参数
@delete="deleteItem"	//父组件监听事件
​```


:item="item" :index="index"	//父组件向子组件发送数据
props : ['item','index']	//子组件接收父组件传递的数据


````
