# 基础使用

```javascript
v-model.number 会将输入的值转化为数值型 vue的声明周期,共有create,mount,update,destory
对应的方法包括 beforeCreate(),created(),beforeMount(),mounted(),beforeUpdate(),updated()
beforeDestory(),destoryed()方法 mount对应的是元素的挂载过程
v-model.lazy 只有在Input框失去焦点时，才更新数据  v-model.trim
v-model.number 会将输入的值转化为数值型
只能在 input textarea 及select元素上使用 select值为0.1.2
class v-bind:class= {active:isActive,'textdanger':isDanger}
可以直接定义数组 data:{ active : true ,'text-danger' : false}
方法 return {active:this.isActive && !this.hasError}
M层数据层，V视图层，VM视图模型层，根据数据的变化改变视图
vue会尽量服用页面上的元素，但是只不会清空，所以添加一个key="username",v-for为了性能应该添加不用index为标识的:key,频繁操作数组index会浪费性能，应该用唯一标识符
template,我们需要用div包裹一组元素，不希望该div渲染，使用template进行包裹即可
this.$set("address","beijing");	//响应式的添加属性
Vue.$set(vm.infoList,1,5)	//修改数组的第1项为5
```

# 计算属性的getter和setter

```js
//将一个计算属性分为两个部分
computed:{
    fullName:{
        get:function () {

        },
            set:function (val) {

            }
    }
}
```

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
			data(){
	           return {
	               
	           } 
			},
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

props:{
	msg:String,
	age:[Number,String]		//对传过来的数据进行校验
    email:{
    	type:String,
    	required:false,
    	default:"Default Vlue",
        validator:function(value){
        	return false;
        }//以对象的形式进行数据的检验,校验器，类型，默认值
    }
}

````

# 给组件绑定原生事件

```html
如果就想给模板上绑事件，使用	@click.native=""	//进行绑定
```

# 获取dom元素

vue中对于复杂的dom操作可能力不重新，为了减少dom的获取操作

```html
<div ref="hello" id="app"></div>
let divDom = this.$refs.hello		//获取的就是div的dom节点
技巧:通过ref获取一个模板的节点，可以直接获取该模板实例，更改他的属性或调取方法
```

# 插槽

```html
<slot></slot>	//插槽子组件中写这个，用父组件写的东西进行填充

<div slot="header">	//插槽中的内容
</div>

<slot name="header"></slot>	//引用header部分的内容作为插槽中的内容
```

# 动态组件

```html
<component :is="type"></component>	//会根据type的值加载对应名称的组件
```

# vue的动画效果

```css
出现效果,3秒出现
.v-enter{		//动画开始第一帧加载两个样式，第二帧移除-enter，opacity发生了变化，所以下面监
    opacity : 0;	//发生效果，3秒内完成opacity
}
.v-enter-active{
    transition: opacity 3s;
}

消失效果,3秒出现
.v-leave-to{
    opacity ： 0；
}
.v-leave-active{
    transition: opacity 3s;
}

    <transition name="">
        <div v-if="show" @click="handleDom"></div>
    </transition>
//如果name有值，植入dom则样式名，dom-enter,写v-if,或v-show，都可以
            
@keyframe bounce-in{
    0%{
        transform:scale(1);
    }              
    50%{
        transform:scale(1.5);
    }
    100%{
        transform:scale(2;
    }
}
.v-enter-active{
    transform-origin: left center;	//防止动画效果出现问题
    animation: bounce-in 1s;
}
.v-leave-active{
    transform-origin: left center;
    animation: bounce-in 1s reserse;
}

    <transition name=""
		enter-active-class=""
		leave-active-class=""
	>
        <div v-if="show" @click="handleDom"></div>
    </transition>
    //这样就可以使用我们预先准备好的样式，例如搭配animate.css
```

# 使用animate.css

```html
http://www.jq22.com/yanshi819	//添加样式名格式 class="animate flash" 就会添加对应的效果

这样添加出来的效果在页面第一次加载的时候没效果,需要按如下的写法，定义一个appear-class即可
<transition
            type="transition"		//:duration="10000"
            appear
            enter-active-class=""
            leave-active-class=""
            appear-active-class=""
            >
    <div v-if="show">
        aaaa
    </div>
</transition>
// 我们自己写的过度动画时长可能和animate.css中定义的时长不一样,type以哪个为准，这里以我们自己定义的为准，或者将type替换为:duration="10000",来规定动画的总时长，对应的class被清除掉
：duration={"enter":5000,"leave":5000}
```

# 动画的钩子函数

```html
@before-enter	//在动画效果运行之前，有一个el的函数参数
@enter			//在上面函数运行完毕后，运行动画效果,el,done，元素和回调函数函数参数，我们在写完自己的方法后，主动调用一下done()；方法
@after-enter	//

@before-leave
@leave
@after-leave
```

# vue-cli

````js
npm install --global vue-cli
vue init webpack Travel	//初始化项目
npm install 		 	//
cd Travel
npm run dev
````

## 搭建的项目分析

## 文件结构

## 路由

```js
src/main.js	整个项目的入口文件
import Vue from 'vue'
import App from './App'
import router from './router'

Vue.config.productionTip = false

new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
//挂在到index.html文件的div元素上，引入了App组件显示在页面上，默认首页上的内容就是组件中的内容

//App.vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>	//路由
  </div>
</template>
```

根据url不同展示给用户不同的页面

```html
<router-view/>		//当前路由对应的路径
找到router文件夹下的index.js
```

```js
//index.js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',		//访问路径，则访问helloWorld
      name: 'HelloWorld',
      component: HelloWorld
    }
  ]
})
//HelloWorld中就包含了页面中剩余的内容，路由就是根据路径/用路由显示了对应的页面
```

