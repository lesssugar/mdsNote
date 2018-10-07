# 基础使用

```javascript
v-model.number 会将输入的值转化为数值型 vue的声明周期,共有create,mount,update,destory
对应的方法包括 beforeCreate(),created(),beforeMount(),mounted(),beforeUpdate(),updated()
beforeDestory(),destoryed()方法 mount对应的是元素的挂载过程
updated()	页面因为数据变化重新渲染触发该函数
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
.v-enter-active,.v-leave-active{
    transition: all 3s;		//这样的话什么动画效果都可以直接进行使用，而不用都写
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
cnpm install --global vue-cli
vue init webpack Travel	//初始化项目
cnpm install 		 	//
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

export default new VueRouter ({
  // 注册应用中所有的路由
    
  routes: [
    {
      path: '/about',
      component: About
    },
    {
      path: '/home',
      component: Home,
      children: [	//如果想保留父页面更换子组件，需要这种，否则会跳转页面
        {
          path: '/home/news',
          component: News
        }
      ]
    },
    {
      path: '/',
      redirect: '/about'
    }
  ]
})
```

## 路由传递参数

```html
{
path:'detail/:id',
component: MessageDetail
}
//父组件
<ul>
    <li v-for="m in messages" :key="m.id">
        <router-link :to="`/home/message/detail/${m.id}`">{{m.title}}</router-link>
        <button @click="pushShow(m.id)">push查看</button>
        <button @click="replaceShow(m.id)">replace查看</button>
    </li>
</ul>
//子组件
<ul>
    <li>id: {{$route.params.id}}</li>
    <li>title: {{detail.title}}</li>
    <li>content: {{detail.content}}</li>
</ul>
```

## 路由参数改变

```js
watch:{
    $route:{	//对路由进行监视即可
        handler:function(){
            
        }
    }
}
```



## 页面跳转

```html
vue项目中使用
<router-link to="/list" class="home">点击进入列表页</router-link>	a标签功能+ 
//js中用路由进行页面跳转
this.$router.push('/')
```

# 定制自己的变量

```bash
webpack.base.conf.js	文件
  resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
    }
  },
 //可以模仿@添加一个路径变量，添加后需要重启项目
```

# 轮播插件 Vue-Awesome-Swiper

```bash
npm install vue-awesome-swiper --save

main.js引入
import Vue from 'vue'	//默认有无需引入
import VueAwesomeSwiper from 'vue-awesome-swiper'

// require styles
import 'swiper/dist/css/swiper.css'

Vue.use(VueAwesomeSwiper, /* { default global options } */)	//无参数可以暂时后面的去掉
```

```html
//v-if的作用在父组件给子组件传值使用，防止吟开始是空数组，获取数据后重新渲染，进入页面不是第一个
<template>
  <swiper :options="swiperOption" v-if="infoList.length">
    <swiper-slide v-for="(slide, index) in swiperSlides" :key="index">I'm Slide {{ slide }}</swiper-slide>
    <div class="swiper-pagination" slot="pagination"></div>
  </swiper>
</template>

<script>
export default {
  name: 'carrousel',
  data () {
    return {
      swiperOption: {
        pagination: {
          el: '.swiper-pagination'，
          loop: true,		//可以进行循环
          autoplay : false	//自动滚动
        }
      },
      swiperSlides: [1, 2, 3, 4, 5]
    }
  },
  mounted () {
    setInterval(() => {
      console.log('simulate async data')
      if (this.swiperSlides.length < 10) {
        this.swiperSlides.push(this.swiperSlides.length + 1)
      }
    }, 3000)
  }
}
</script>
<style scoped lang="stylus">
  >>> .swiper-pagination-bullet-active
    background: #fff;
  .swiper-img
    width :100%
    height :100px
</style>

```

# 本地缓存

```js
window.localStorage.setItem('key',value);window.localStorage.setItem('key',value);
window.localStorage.getItem('key');
```



# 样式的穿透

```html
写了scoped的样式仅在该组件中有效，如果想让他对子组件残生效果
  >>> .swiper-pagination-bullet-active
    background: #fff;
这样的话该样式在子组件中也会被修改
```

# vue-devtool

```bash
https://github.com/vuejs/vue-devtools	//下载zip包
https://www.cnblogs.com/yuqing6/p/7440549.html	//安装教程
```

# axios

```bash
cnpm install axios --save
axios.get('/static/mock/index.json').then(this.showGetInfo())
showGetInfo(res){
	console.log(res);
}
//项目中的文件夹只有mock文件夹可以被外部进行访问，所以测试数据应该放在这，当时不该路径写死，可以用官方的代理功能进行映射
/config/index.js

 dev: {
    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      '/api':{	//添加
        target:'http://localhost:8080',
        pathRewrite:{
          '^/api': '/static/mock'
        }
      }
    },
   //添加一个路径的代理即可，这样就可以再开发环境进行代理 
```

# 类app页面滚动效果 Better-scroll

```html
网页原版的上下拖动效果比较难看，需要使用插件,先将要禁止拖动的区域,
position:absolute
top:1.58rem			//1.58是为了将头部让出来，头部会一直在上方，根据自己的元素大小更改
left:0
right:0
bottom:0
overflow:hidden
cnpm install better-scroll --save
//在要使用的页面中
@import Bsscroll from 'better-scroll'
mounted(){
	this.scroll = new Bsscroll(this.$ref.wrapper);	//将要滚动的dom元素放入
}
//上拉下拉效果和弹性效果
```

# alplabet效果

```html
为元素添加:ref="letter"
监听
letter(){
	if(!!this.letter){
		const element = this.$refs[this.letter][0];
		this.scroll.scrollToElement(element);
	}
}

    handleLetterClick (e) {
      this.$emit('change', e.target.innerText)
    },
    handleTouchStart () {
      this.touchStatus = true
    },
    handleTouchMove (e) {
      if (this.touchStatus) {
        if (this.timer) {
          clearTimeout(this.timer)
        }
        this.timer = setTimeout(() => {		//拖动效果也会产生页面的变化
          const touchY = e.touches[0].clientY - 79
          const index = Math.floor((touchY - this.startY) / 20)
          if (index >= 0 && index < this.letters.length) {
            this.$emit('change', this.letters[index])
          }
        }, 16)
      }
    },
    handleTouchEnd () {
      this.touchStatus = false
    }
```

# vuex

```javascript
//state中存储的是公用的数据，我们如果想修改数据通过dispatch方法调用actions,然后通过commit方法操作mutations，最后通过mutate来操作state
cnpm install vuex --save

//创建index.js文件,如果拆分成多个文件，每个export default即可  
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
const state = {
    count: 0
}
const mutations = {		//页面中的方法分发到action,action调用mutation
    //增加的mutation
    increment(state，{item}){	//接受传过来的参数item
        state.count++;
    },
    decrement(state){
        state.count--;
    }
}
const actions = {
    incrment({commit,state}，item){	//如果不需要读取state可以不写,还可以接受传过来的数据 
        commit('increment')
    }
}
const getters = {	//写计算属性返回数据的方法
    
}

export default new Vuex.Store(
	state,		//状态对象
    mutations,	//包含多个更新state的对象
    actions,	//包含多个对应事件回调函数的对象
    getters		//包含多个getter计算属性函数的对象
)
    
//main.js，下面引入
import store from  ./store' 

<div>$store.state.count</div> 

//页面中的方法
increment(){
   this.$store.dispatch('incrment');	//出发store中的actions
}

//简化写法
import {mapState,mapGetters,mapActions} from 'vuex'
//读取属性都应该放在计算属性里，actions都放在methods里 
methods:{
    ...mapActions['increment','decrement']	//这样的话页面中的方法就和store中的对应起来，名称一样
}
```

# 项目的打包发布

```bash
npm run build		//打包
cnpm install -g serve	//安装服务器
serve dist 
```

## 使用tomcat

```bash
webpack.prod.conf.js	//文件修改
  output: {
    path: config.build.assetsRoot,
    filename: utils.assetsPath('js/[name].[chunkhash].js'),
    chunkFilename: utils.assetsPath('js/[id].[chunkhash].js'),
    publicPath: 'vue_demo'		//添加项目名
  },
  重新打包，将dist复制一份改名vue_demo
  
```

# Mint-ui

```bash
cnpm install --save mint-ui
npm install --save-dev babel-plugin-component	//安装

//修改babelrc配置文件
{
  "presets": [
    ["es2015", { "modules": false }]
  ],
  "plugins": [["component", [
    {
      "libraryName": "mint-ui",
      "style": true
    }
  ]]]
}
//将这部分添加到项目的配置文件的插件部分，将Mint-ui的部分按需打包
["component", [
    {
      "libraryName": "mint-ui",
      "style": true
    }
  ]]
```

# keep-alive

```html
//缓存路由组件
只能缓存路由组件
    <keep-alive>
      <router-link to="/list">
        <div class="to-page">点击到列表页面</div>
      </router-link>
    </keep-alive>
```

# 编程式路由导航

```vue
this.$router.push();	//相当于点击路由连接可以返回
this.$router.replace()	//用新路由代替当前路由(不可以返回当前路由界面)
this.$router.back()		//回退
```

# render

```js
//渲染函数
render: h => h(App)

//相当于
el: '#app'
render:function(createElement){
	return createElement(App);
}
//创建了一个元素并插入到#app页面当中
```

