# Vue-cli



##  创建项目目录

~~~bash
npm install -g vue-cli
vue init webpack my-project
(修改npm配置json文件, 对热加载模块加选项) --open
(或者修改/config/index/autoOpenBrowser: true)
cd my-project
npm run dev
(config下的index.js文件配置eslint语法检查, false)
~~~

## 目录结构

~~~diff
|- /build
|- /config
|- /node_modules
|- /src
  |- /assets
    |- logo.png
  |- /commponents
    |- Home.vue
  |- /router
    |- index.js
  |- App.vue
  |- main.js
|- /static
|- /test
|- .babelrc
|- ...
|- index.html
|- package-lock.json
|- package.json
|- README.md
~~~

+ build 文件夹：用于存放 webpack 相关配置和脚本。开发中仅 偶尔使用 到此文件夹下 webpack.base.conf.js 用于配置 less、sass等css预编译库，或者配置一下 UI 库。

+ config 文件夹：主要存放配置文件，用于区分开发环境、线上环境的不同。 常用到此文件夹下 config.js 配置开发环境的 端口号、是否开启热加载 或者 设置生产环境的静态资源相对路径、是否开启gzip压缩、npm run build 命令打包生成静态资源的名称和路径等。

+ dist 文件夹：默认 npm run build 命令打包生成的静态资源文件，用于生产部署。

+ node_modules：存放npm命令下载的开发环境和生产环境的依赖包。

+ src: 存放项目源码及需要引用的资源文件。
  + src下assets：存放项目中需要用到的资源文件，css、js、images等。
  + src下componets：存放vue开发中一些公共组件：header.vue、footer.vue等。
  + src下router：vue-router vue路由的配置文件。
  + src下app.vue：使用标签``渲染整个工程的.vue组件。
  + src下main.js：vue-cli工程的入口文件。

+ index.html：设置项目的一些meta头信息和提供``用于挂载 vue 节点。
+ package.json：用于 node_modules资源部 和 启动、打包项目的 npm 命令管理。



## UI组件

饿了么出品

### mint-ui

如果针对移动端vue项目mint-ui

~~~bash
npm install mint-ui -S
~~~

使用mint-ui

~~~javascript
# /src/main.js
// 引入插件
import MintUi from 'mint-ui'
Vue.use(Mint)
// 样式引入
import 'mint-ui/lib/style.css'
~~~

移动端UI产生的bug

index.html无法产生轮播图

~~~html
删除index中<!DOCTYPE html>
~~~





### element-ui

如果针对pc端ui组件element-ui

~~~bash
npm install element-ui -S
~~~



## Violation

~~~txt
[Violation] Added non-passive event listener to a scroll-blocking 'mousewheel' event. Consider marking event handler as 'passive' to make the page more responsive.
~~~

 **Chrome51** 版本以后，Chrome 增加了新的事件捕获机制－Passive Event Listeners 

解决问题

~~~bash
npm i default-passive-events -S
~~~

~~~javascript
# main.js
import 'default-passive-events'
~~~

## axios

~~~bash
npm i axios -S
~~~

~~~javascript
# main.js
import Axios from 'axios'
Vue.prototype.$axios = Axios
Axios.defaults.baseURL = 'https://.../'

// 在vue实例中使用this.$axios.
this.$axios.get('..')
  .then(res=>console.log(res))  // fn--success
  .catch(err=>console.log(err)) // fn--fail
~~~



## 自定义插件

/src下新建plugins文件夹, 新建installer.js

~~~javascript
# installer.js
// vue 插件必须需要install函数
function Installer () {
  // 自身初始化行为
}

Installer.install = function (Vue) {
  // 接收Vue的构造函数, 给原型挂载属性或注册全局组件或过滤器
  console.log(Vue)
}

export default Installer
~~~

~~~javascript
# main.js中引入
import Installer from '@/plugins/installer'
Vue.use(Installer)
~~~

注册组件

~~~javascript
# installer.js
Installer.install = function (Vue) {
  // 1.注册全局组件
  Vue.component('test', {
    template: `<h1>哈哈哈</h1>`
  })
}
~~~

使用组件

~~~vue
# Home.vue
<test />
~~~

挂载属性1

~~~javascript
# installer.js
Installer.install = function (Vue) {
  	// 挂载属性
	Vue.prototype.$log = function () {
	  console.log('hhhhhhahhhha')
	}
	this.$log = 'abbasdbad' // 子类对象是可以修改父类的属性的
}
~~~

挂载属性2

~~~javascript
# installer.js
Installer.install = function (Vue) {
  let log = function () {
    console.log('我们自己的log插件')
  }
  
  Object.defineProperty(Vue.prototype, '$log', {
    // 设置$log属性时的行为
    set: function (newV) {
      console.log('你做梦')
      // log = newV
    },
    get: function () {
      // 获取的方式
      return log
    }
  })
}
~~~

使用插件

~~~vue
# Home.vue
<script>
export default {
  name: 'Home',
  data () {return {}},
  created () {
    this.$log = 'aaaabbb'
    this.$log()
  }
}
</script>
~~~



## 提取公共组件复用

### 创建vue代码片段

ctrl+shift+p >> 配置用户代码片段 >> vue.json

~~~json
{
  "Print to console": {
    "prefix": "vue:5", 
    "body": [
    "<template>",
    "",
    "</template>",
    "",
    "<script>",
    "export default {",
    "  data () {",
    "    return {",
    "    }",
    "  }",
    "}",
    "</script>",
    "",
    "<style scoped>",
    "",
    "</style>",
    ""
    ],
  "description": "Log output to console"
  }
}
~~~

ok, 在任意 .vue文件中输入vue:5 输出

~~~vue
<template>

</template>

<script>
export default {
  data () {
    return {
    }
  }
}
</script>

<style scoped>

</style>
~~~

### 设计ul组件

在/src/components下新建/common/MyUl.vue文件

~~~vue
<template>
  <ul>
    <slot></slot>
  </ul>
</template>

<script>
export default {
  // 设置组件名
  name: 'my-ul',
  data () {
    return {
    }
  }
}
</script>

<style scoped>
  /* 添加样式 */
  ul {
    margin:0;
    padding: 0;
    overflow: hidden
  }
</style>
~~~



### 引入注册应用组件

修改main.js, 注册全局组件

~~~javascript
import MyUl from './components/common/MyUl'
Vue.component(MyUl.name, MyUl)
~~~

Home.vue应用组件替换ul为my-ul, 删除ul样式

~~~vue
<my-ul>
  ...
</my-ul>
~~~

### 设计li组件

~~~text
同上操作
~~~



## 路由跳转1

由v-model绑定的selected值为点击对象的id ==> 页面输出 {{selected}}

修改id为英文名Home, Member, Shopcart, Search

分别创建Member, Shopcart, Search组件

~~~diff
  |- /src
    |- /components
+     |- /member
+       |- Member.vue
+     |- /shopcart
+       |- Shopcart.vue
+     |- /search
+       |- Search.vue
~~~

配置路由

~~~javascript
# /src/router/index.js
import Member from '@/components/member/Member'
import Shopcart from '@/components/shopcart/Shopcart'
import Search from '@/components/search/Search'

...
	{
      path: '/member',
      name: 'Member',
      component: Member
    },
    {
      path: '/shopcart',
      name: 'Shopcart',
      component: Shopcart
    },
    {
      path: '/search',
      name: 'Search',
      component: Search
    }
...
~~~



监视selected的改变实现点击跳转

~~~vue
<script>
    ...
    watch: {
        selected: function (newV, oldV) {
            // console.log(newV, oldV)
            // console.log(this)
            this.$router.push({
        		name: newV
      		})
        }
    }
</script>
~~~



## 路由跳转2

创建NewList组件

~~~diff
  |- /src
    |- /components
+     |- /newlist
+       |- NewList.vue
~~~



配置路由index.js

~~~javascript
import NewList from '@/components/newlist/NewList'
{
    path:'/newlist',
    name:'NewList',
    component: NewList
}
~~~

Home组件路由跳转设置

~~~javascript
# Home.vue
...
msc: [
    {..., route:{name: 'NewList'}},
    {..., route:{name: '...'}},
    {..., route:{name: '...'}},
    {..., route:{name: '...'}},
    {..., route:{name: '...'}},
    {..., route:{name: '...'}}
]
...
~~~

## NewList组件添加内容

使用mint-ui添加组件, 为div添加样式

~~~vue
<template>
  <div class="tmpl">
    <mt-header title="标题过长会隐藏后面的内容啊哈哈哈哈">
      <router-link to="/" slot="left">
        <mt-button icon="back">返回</mt-button>
      </router-link>
    <mt-button icon="more" slot="right"></mt-button>
    </mt-header>
  </div>
</template>
...
<style scropd>
    .tmpl .mintheader {
        background-color: yellowgreen
    }
</style>
~~~

用div代替router-link, 重定向用@click

~~~vue
<div @click="goback" slot="left">
    ...
</div>
<script>
    ...
    methods: {
        goback: function(){
            this.$router.go(-1)
        }
    }
</script>
~~~



## 动态修改title

+ 由于组件需要多次使用, 可以继续提取为公共组件复用

~~~diff
  |- /src
    |- /components
+     |- /common
+       |- NavBar.vue
~~~

+ 将NewList组件中的模板内容, 样式和方法直接剪切到NavBar中
+ 为了能复用组件且动态修改标题使用props属性传值

~~~vue
# NavBar.vue
...
	<mt-header :title="title">
    ...
...
<script>
    ...
    name: 'nav-bar',
    props: ['title'], // props属性在模板中可以直接使用
    ...
</script>
~~~

+ 注册全局组件

~~~javascript
import NavBar from './components/common/NavBar'
Vue.component(NavBar.name, NavBar)
~~~

+ 使用组件

~~~vue
# NewList.vue
<template>
	<nav-bar :title="title"/>
</template>

...
<script>
    ...
    data (){
        return {
            title: '新闻列表' // 这样可以根据修改title动态传值
        }
    }
</script>
~~~



## moment.js

格式化日期js

~~~javascript
moment().format('YYYY-MM-DD') // "2020-03-20"
moment().format('YYYY-MM-DD HH:mm:ss') // "2020-03-20 15:14:50"
moment('2020-03-19').fromNow() // "2 days ago"
moment.locale('zh-cn') // "zh-cn"
moment('2020-03-19').fromNow() // "2 天前"
moment('2020-03-19 20:20:20').fromNow() // "19 小时前"

moment().format('YYYY-MM-DD')
~~~

*定义全局过滤器*

~~~bash
npm i moment -S
~~~

~~~javascript
# main.js
import Moment from 'moment'
Vue.filter('convertTime', function (data, format) {
  return Moment(data).format(format)
})
~~~

*使用过滤器*

~~~vue
# NewList.vue
...
<p>发表时间:{{news.passtime | convertTime('YYYY-MM-DD')}}</p>
...
~~~



## 参数路由跳转

NewList.vue带参

> router-link :to = "{name:'NewsDetail', query: {id: news.id}}"
>
> router-link :to = "{name:'NewsDetail', params: {id: news.id}}"



index.js导航

> 以query传参
>
> {name: 'NewsDetail', path: '/news/detail', component: NewsDetail} 
>
> 
>
> 以params传参
>
> {name: 'NewsDetail', path: '/news/detail/:id', component: NewsDetail}



crated钩子

+ 获取路由参数

> this.$route.params.id ==> {id: xx}

~~~javascript
let { id } = this.$route.params // or this.$route.query
~~~



+ 拼接url发送请求获取详情数据

~~~javascript
let url = '../' + id
this.$axios.get(url).then(res=>{this.newInfo=res.data}).catch()
~~~



## 冲突

+ v-html不能结合scoped使用,
+ 如果要对单个组件中v-html中的样式做修改, ,在当前组件scoped需要删除
+ 或者在全局样式中修改



## 组件内的守卫

~~~javascript
beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
    let categoryId = to.query
    // 发请求更改数据
  },
~~~



##  vue-preview

预览功能

~~~bash
npm i vue2-preview -S
~~~

~~~javascript
# main.js
import VuePreview from 'vue2-preview'
Vue.use(VuePreview)
~~~



## vuex

~~~bash
npm i vuex -S
~~~

~~~diff
  |- /src
    |- /components
+   |- /store
+     |- index.js
~~~

### 基本使用

初始化store下的index.js

~~~javascript
# /store/index.js
import Vue from 'vue'
import Vuex from 'vuex'

//挂载Vuex
Vue.use(Vuex)

//创建store对象
const store = new Vuex.Store({
  // ...
    state: {
        num:1
    }
})
// 将对象暴露出去
export default store
~~~

页面渲染

~~~vue
# App.vue
<template>
	<div id='app'>
        {{ $store.state.num }}
    </div>
</template>
~~~

### getter方法使用

~~~javascript
# /store/index.js
...
const store = new Vuex.Store({
  // ...
    state: {
        num:1
    },
    getters{
    	getNum(state) { // 获取值, 离state最近
    		return state.num
		}
	}
})
...
~~~

~~~vue
# App.vue
<template>
	<div id='app'>
        {{ $store.getters.getNum }}
    </div>
</template>
~~~

### 项目中结合当前组件computed使用

~~~vue
# App.vue
<template>
	<div id='app'>
        {{ getNum }}
    </div>
</template>
<script>
    ...
    computed: {
        getNum: function(){
            return this.$store.getters.getNum
        }
    }
    ...
</script>

~~~

### mutations对数据改变处理

~~~javascript
# /store/index.js
...
const store = new Vuex.Store({
  // ...
    state: {
        num:1
    },
    getters{
    	getNum(state) { // 获取值, 离state最近
    		return state.num
		}
	},
    mutations: {
        addNum(state) {
        	state.num++
        },
      	addsNum(state, data){
        	state.num += data
        },
        // 异步丢失记录
        addNumAsync(state, data) {
        	setTimeout(() => {
        		state.num += data
        	}, 0)
        }
    }
})
...
~~~

~~~vue
# App.vue
<template>
	<div id='app'>
        {{ getNum }}
        <button @click="add">添加数量</button>
    	<button @click="addByNum">添加10数量</button>
        <button @click="addAsync">添加20数量</button>
    </div>
</template>
<script>
    ...
    methods: {
    	add() {
        	this.$store.commit('addNum')
    	},
    	addByNum() {
        	this.$store.commit('addsNum', 10)
    	},
        
    	addAsync() {
        	this.$store.commit('addNumAsync', 20)
    	}
  	},
    ...
</script>
~~~



### mutations的问题

mutations对state数据的操作是只能同步的, 而异步的操作, 页面看上去数据ok, 实则会丢失记录

vue dev-tools查看历史记录,  修改 addNumAsync

~~~javascript
# /store/index.js
mutations: {
    ...
	addNumAsync(state, data) {
        state.num += data
    }
	...
}
// 异步的操作在actions中进行
actions: {
    addByNumAction({commit}, num) {
        setTimeout(() => {
            commit('addNumAsync', num)
        }, 0);
    }
}
~~~

~~~vue
# App.vue
...
methods: {
	...
	addAsync() {
        this.$store.dispatch('addByNumAction', 20)
    }
}
~~~

### 总结

防止程序的bug, 规范数据的流程

+ state定义数据
+ getters获取数据
+ mutations修改数据
+ actions提交更改数据的方法: 对mutations中需要异步修改的数据在这里进行

整个修改数据流程

+  组件中通过.dispatch进行分发Actions 
+  Action 可以提交mutation方法
+  mutation来改变state
+  getters得到并返回改变的state
+  组件通过computed监测到改变的state, 更新数据



### ...mapGetters

mapGetters接收getters中的key

~~~vue
<script>
    import { mapGetters} from 'vuex'
	...
    computed: {
        // 直接传, 展开数组
        ...mapGetters([
            'getNum',
            'getText',
            ...
        ])
        // 重命名, 展开对象, 前面的代替后边
        // 将this.num映射到this.$store.getters.getNum
        /*
        ...mapGetters({
            num: 'getNum',
            text: 'getText',
            ...
        })
        */
    }
	...
</script>

~~~

### modules

随着数据的庞大可能需要modules功能

增加单独的数据

~~~diff
  |- /src
    |- /components
    |- /store
      |- index.js
+     |- num.js
~~~

~~~javascript
# num.js
export default {
    // 将已经写好的/store/index.js中创建出来对象中的全部移到此
    states: {
        // ...
    },
    getters: {
        // ...
    },
    mutations: {
        // ...
    }
}
~~~

~~~javascript
# /store/index.js
// 通过modules接收num, test数据
import num from './num'
const store = new Vuex.Store({
    // 出现同名函数和变量的时候, 为了保护其不被覆盖, webpack机制
    modules: {
        // 不会影响使用
        num:num,
        test:test
    }
})
export default store
~~~

### 踩坑关于新添加的属性

对state中新添加的属性操作会怎么样呢

~~~vue
# App.vue
...
{{ name }}
...
<script>
    ...
    created() {
    	setTimeout(() => {
        	this.$store.dispatch('addProps', 'jack')
    	}, 100);
  	},
    computed: {
        ...mapGetters({
      		getMyNum: 'getNum',
      		name: 'getName'
    	}),
    }
    ...
</script>
~~~

~~~javascript
# /store/num.js ***页面并未渲染出name***
...
	getters: {
        ...
        getName(state) {
        	return state.name
    	}
    },
    mutations: {
        ...
        addProp(state, name) {
        	state.name = name
        }
    },
    actions: {
        ...
        addProps({ commit }, name) {
        	commit('addProp', name)
    	}
    }
...
~~~



~~~javascript
# /store/num.js
import Vue from 'vue'
...
mutations: {
    ...
    addProp(state, name) {
        // state的数据已经被Vue实现双向绑定, 而新添加的属性没有
        // 实例对象.$xxx
        // 构造函数.xxx
        if(!state.name){
            Vue.set(state, 'name', name)
        }else{
            state.name = name
        }
        
    }
},
...
~~~



## mixin

~~~html
<div id='app'></div>
<h2>实例化组件</h2>
<div id='content'></div>
~~~

### 局部混入

~~~javascript
// 定义一个混入对象
var myMixin = {
    created() {
        this.hello()
    },
    methods: {
        hello() {
            console.log('hello from myMixin')
        }
    },
}

// 定义一个使用混入对象的组件
var MyComponent = Vue.extend({
    template: `<p><slot></slot>{{firstName}} {{lastName}} aka {{alias}}</p>`,
    mixins: [myMixin],
    data() {
        return {
            firstName: 'Walter',
            lastName: 'White',
            alias: 'Heisenberg'
        }
    },
})

// MyComponent就是一个组件构造器, 可以直接注册, 或像vue实例样挂载到dom上
// new MyComponent({
//    el:'#content'
// })
var myComponent = new MyComponent() // => "hello from mixin!"
myComponent.$mount('#content')

// 全局注册组件
// Vue.component('my-component', MyComponent)
// 局部注册组件
let App = {
    template:`
		<div id='app'>
            hello
            <MyComponent>局部组件</MyComponent>
		</div>
	`,
    components: {
        MyComponent
    }
}
new Vue({
    el: '#app',
    template: `<App />`,
    components: {
        App
    }
});
~~~

当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。

+ **数据对象在内部会进行递归合并**，并在发生冲突时**以组件数据优先**

+ **同名钩子函数将合并为一个数组**，因此都将被调用。**混入对象的钩子将在组件自身钩子之前调用**。 

+ 值为**对象**的选项,  将被合并为同一个对象。两个对象键名冲突时，**取组件对象的键值对**。 
+ **Vue.extend() 也使用同样的策略进行合并**



### 全局混入

~~~javascript
Vue.mixin({
    created: function () {
        var myOption = this.$options.myOption
        if (myOption) {
            console.log(myOption)
        }
    }
})
new Vue({
    myOption: 'hello!'
})
~~~

### 项目中局部使用

src下新建my_mixin.js

~~~javascript
# /src/my_mixin.js
export default {
    created() {
        // this.$options.name组件的名字
        console.log(`${this.$options.name}请求发出去了`)
    }
}
~~~

组件中引入使用

~~~vue
# App.vue
...
<script>
    import minxin from './my_mixin'
    ...
    mixins: [mixin],
</script>
~~~

全局混入就在main.js中创建引入, 之后的组件直接继承

### 结合vuex使用场景

多个页面公用一个数据字典

~~~javascript
# 定义一个插件, 处理不同组件发出请求
# myPlugin.js
let options
function myPlugin(options){
    
}
myPlugin.install = function(Vue) {
    // 判断当前组件名
    this.$
    // 根据vuex去调用
    Vue.mixin({
        created() {
            if(this.$options.name === 'app' || this.$options.name === 'home') {
                this.$store.dispatch('addByAction')
            }
        }
    })
}
export default myPlugin
~~~

~~~javascript
# main.js 引入
import MyPlugin from './myPlugin'
Vue.use(MyPlugin)
~~~

