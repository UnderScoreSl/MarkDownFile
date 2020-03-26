# VUE

[toc]

VUE  Angular  React

vue只关注图层，框架提高开发效率

提高开发效率：原生js开发 ==> jQuery类库 ==> 前端模板引擎 ==>  vue等框架

浏览器兼容问题 ==>  频繁操作dom元素 ==>  ==> 减少不必要的dom操作，提高渲染效率，双向数据绑定的概念，更多关注业务逻辑

框架：是一套完整的解决方案，对项目的侵入性较大；库（插件）：提供某一个小功能，易切换（从jq到zp，从ejs到art-template）



## MVC和MVVM



MVC后端概念思想：

app.js: 项目的入口模块，一切请求都要从这里进行处理；app.js没有处理路由分发的功能

router.js: 路由分发处理模块，职能单一不负责业务逻辑；

controller模块: 封装了具体的业务逻辑功能，和router.js构成后端的control层

model层: 只负责操作数据库，执行数据的CRUD（create，read，update，delete）；

view视图层：每当用户操作了界面，如果需要进行业务处理，都会通过网络请求，去请求后端的服务器，此时我们的这个请求，就会被后端的app.js监听到。



<hr/>
MVVM是前端视图层的分层开发思想：

主要是把每个页面，分成M，V，VM；

VM是其思想的核心，是M和V的调度者，数据的双向绑定是VM提供的；

M保存的是每个页面单独的数据，通过VM渲染到页面中去；

V就是每个页面的结构，V层获取的数据保存的时候，都要由VM做中间的处理。



## Vue基本代码

~~~html
<script src="lib/vue.js"></script>

<!--vue实例所控制的区域就是MVVM中的V-->
<div id="app">
  <p>{{ msg }}</p>
</div>

<script>
  // 当我们导入包后，浏览器的内存中就多了个vue构造函数
  // 2.创建script标签，创建vue实例，这个vue对象就是MVVM中的调度者VM
  var vm= new Vue({
    el: '#app', // 表示当前这个实例要控制页面的那个元素
    data: {   // data属性存放el中要用到的数据，data就是MVVM中的M用来保存页面的数据
      msg: '欢迎学习vue' // 通过vue提供指令，方便把数据渲染页面，程序员就不必操作dom元素了
    }
  })

</script>
~~~

second2

~~~html
<div id="app"></div>
// 1.引入
<script src="..."></script>
<script>
    new Vue({
        el:'#app',
        template:`<div v-html='msg'></div>`,
        // 一个组件的 data 选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝
        data(){ // es6匿名函数简写
        	return {
            	msg:'<h2>指令系统</h2>'
        	}
    	}
    })
</script>
~~~



## V-指令

### 基本指令

```
v-text：是没有闪烁问题的，但是一解析到v-text，会覆盖掉元素中原本的内容，不能解析html
v-html：可以解析出html，当然也会覆盖内容
v-cloak：利用v-cloak可以解决因网速过慢加载的插值表达式闪烁显示的问题，不会覆盖内容，不能解析html

v-bind：data中的数据不能直接给属性title，会解析成字符串，需要用v-bind绑定属性的指令，还可以加上合法的js表达式, (v-bind::缩写为:)
v-on：绑定事件 (v-on::缩写为@)

v-bind只能实现单向绑定,从M自动到V
v-model能实现数据表单元素和Model中数据的双向绑定，v-model只能运用在表单元素带有value属性中
[v-model = v-bind:value='msg',v-on:input=change实现双向绑定数据]

在vue实例中想要获取data的属性或者想要使用methods中的方法必须要用this.来访问
```



### 事件修饰符

~~~vue
事件修饰符直接在@click之后点出来
使用stop可以阻止冒泡,在inputHandler事件上修饰
使用prevent阻止默认行为
使用capture实现捕获触发事件机制,在divHandler事件上修饰
使用self实现只点击当前元素才触发事件,只阻止当前元素事件的冒泡和stop不同
使用once实现只触发一次事件
.prevent.once可以同时使用先后顺序不影响
~~~

### 绑定样式:class

~~~vue
  <!--第一种方式直接传递一个数组，这里的class必须用v-bind绑定，类名必须加引号
  在数组中可以用三元表达式, 三元表达式可以写成对象的形式-->
  <h1 :class="['thin', 'italic', flag?'active':'']">这是一个h1标签</h1>
  <h1 :class="['thin', 'italic', {active:flag}]">这是一个h1标签</h1>

  <!--直接使用对象, 对象的属性类名可以不用引号-->
  <h1 :class="{red:true, thin:true, active:true}">这是一个h1标签</h1>
  <h1 :class=classObj1>这是一个h1标签</h1>

  <!--直接使用对象的数组-->
  <h1 :class='[classObj1, classObj2]'>这是一个h1标签</h1>
~~~

### 内联样式:style

```vue
普通绑定
<h1 :style="{color:'red', 'font-weight':200}">这是一个h1</h1>
对象
<h1 :style="styObj1">这是一个h1</h1>
对象数组
<h1 :style="[styObj1, styObj2]">这是一个h1</h1>
```

### 循环v-for

~~~vue
<p v-for="item in list">{{ item }}</p>
<p v-for="(item, i) in list">索引：{{ i }}----值：{{ item }}</p>
<!--对象遍历，vue2.20+组件中使用时必须使用key-->
<p v-for="(value, key) in user">值：{{ value }}---键：{{ key }}</p>
<!--v-for迭代数字，数字从1开始-->
<p v-for="count in 10">这是第{{ count }}次循环</p>

<script>
  var vn = new Vue({
    el: '#app',
    data: {
      list: [1,2,3,4,5,6],
      user: {
        id: 1,
        name: '托尼',
        gender: 'male'
      }
    }
  })
</script>
~~~

### v-for注意

#### v-for中key的意义

+ 先说说虚拟Dom的diff算法
  + 不直接操作dom元素而操作数据重新渲染页面, 背后原理就是高效的diff算法, 基于两种假设
  + 两个相同的组件产生类似的dom结构, 不同组件产生不同的dom结构
  + 同个层级的一组节点通过唯一的id区分
+ key是为了给每个节点做唯一标识, diff算法就能正确识别此节点, 找到正确的位置区插入新的节点, 也就是更高效的更新虚拟dom

~~~vue

<div id="app">
  <div>
    <label>ID：
      <input type="text" v-model="id">
    </label>
    <label>NAME：
      <input type="text" v-model="name">
    </label>
    <input type="button" value="添加" @click="add">
  </div>
    
  <!--v-for循环key只能使用number或者string-->
  <!--key使用必须使用v-bind绑定-->
  <!--在组件中或一些特殊情况下使用应该指定唯一的key值-->
  <!-- <p v-for="item in list" :key="item.id"> -->
      
  <p v-for="item in list">
    <input type="checkbox">
    {{ item.id }}---{{item.name}}
  </p>
</div>
<script>
  var vm = new Vue({
    el:'#app',
    data:{
      id:'',
      name:'',
      list: [
        {id:1, name:'李斯'},
        {id:2, name:'嬴政'},
        {id:3, name:'赵高'},
        {id:4, name:'韩非'},
        {id:5, name:'荀子'},
      ]
    },
    methods: {
      add(){
        // this.list.push({id: this.id, name: this.name}) // 结尾追加
        // 在不使用key的情况下，add时，已选择的checkbox会往前移，
        // vue不会记录选择的哪一项内容而只记录选择的index
        // 在头部添加时，所有数据的index已改变，vue根据index重新选择checkbox
        this.list.unshift({id: this.id, name: this.name})  // 头部追加
        this.id = this.name = '';
      }
    }

  })
</script>
~~~

### v-if 、v-show

```vue
<div id="app">
  <input type="button" value="toggle" @click="toggle">
  <!--v-if每次都会删除或创建元素-->
  <h3 v-if="flag">这是v-if控制的元素</h3>
  <!--v-show不会删除创建，只会切换display：none;样式-->
  <!--v-if有较高的切换性能消耗，v-show有较高的初始渲染消耗-->
  <h3 v-show="flag">这是v-show控制的元素</h3>
  <!--元素涉及频繁的切换不使用v-if，而是用v-show-->
  <!--元素可能永远不会被显示出来（即一开始就是display：none，不想被页面初始就渲染的意思）则不使用v-show而是用v-if-->
</div>
flag: true
```



### 过滤器

~~~vue
msg | 过滤器名称 // 管道符 ，如果全局和私有过滤器名称相同，优选调用私有过滤器
// 全局过滤器，所有实例都能使用
Vue.filter('过滤器的名称', function(){
	...
})
// 私有过滤器，在实例中定义
filters: { 
	// dataFormat 过滤器名称
	dataFormat: function(dataStr){
		...
	}
}
~~~



### ES6字符串新方法

~~~JavaScript
String.prototype.padStart(2, '0'); // 字符串补0操作
~~~

### 自定义按键修饰符

~~~vue
// 用法和事件修饰符一样
.enter
.tab
.delete
.esc
.space
.up
.down
.left
.right

// 自定义全局修饰符键盘码
Vue.config.keyCodes.f2 = 113;
~~~

### 自定义指令

~~~javascript
// 给文本框一个自定义指令 v-focus 自动获取焦点
// 全局的自定义指令  钩子函数
Vue.directive('focus', {
    inserted: function (el) {
      el.focus()
    }
})

// 私有指令，在实例中定义
directives: { // 自定义私有指令
  'fontweight':{
    bind:function(el, binding){ 
      el.style.fontWeight = binding.value
    }
  }
  'fontsize':function(el,binding){ // 指令简写方式，等同于在bind和update中添加
	el.style.fontSize = parseInt(binding.value) + 'px'
  }
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
},
~~~

钩子函数

~~~html
<div id="hook-arguments-example" v-demo:foo.a.b="message"></div>
<script>
  Vue.directive('demo', {
    // 指令被编译好, 即将运行功能时触发, 运行一次, 进行一次性的初始化设置
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
})

new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'hello!'
  }
})
</script>
~~~

> name: "demo"
> value: "hello!"
> expression: "message"
> argument: "foo"
> modifiers: {"a":true,"b":true}
> vnode keys: tag, data, children, text, elm, ns, context, fnContext, fnOptions, fnScopeId, key, componentOptions, componentInstance, parent, raw, isStatic, isRootInsert, isComment, isCloned, isOnce, asyncFactory, asyncMeta, isAsyncPlaceholder



## Vue生命周期

~~~vue
<script>
    var vm = new Vue({
      el:'#app',
      data:{},
      methods:{},
        
      /* 创建实例阶段 */
      beforeCreate(){
        // 在此生命周期函数中，data和 methods都没有初始化
      },
      created(){
        // 可以操作后端数据,数据响应视图,,发起ajax请求 
      },
      beforeMount(){
        // 模板已经在内存中编辑完成，还没有渲染到页面的，执行时，页面的内容还没有被替换
      },
      mounted(){
        // 如果要操作dom节点，最早可以在 mounted中进行
      },
      /* 组件运行阶段 */
      beforeUpdate(){
        // 数据被更新了执行，执行时页面中的数据仍是原来的旧数据，而data中的数据已经改变
      },
      update(){
        // 数据被更新了执行，执行时页面中的数据同步data的数据
      },
      /* 组件销毁阶段 */
      beforeDestroy(){
        // 前面所有数据方法等等都是可用的
      },
      destroyed(){
        // 不可用data，等。。
      },
      actived(){
      }
      deactivated(){
      }
    })
</script>
~~~

## vue-resource

axios等

实现数据请求



### 全局配置根路径

~~~vue
Vue.http.options.root = '/root', // 通过全局配置的根路径域名，在每次请求时请求的url路径应该以相对路径开头，即不能以斜线开头，否则不会启用根路径拼接
~~~

全局配置emulateJSON选项

```
Vue.http.options.emulateJSON = true;
```



## Vue动画

~~~vue
半场动画v-enter-active
v-enter
v-enter-to

半场动画v-leave-active
v-leave
v-leave-to
~~~





## 定义Vue组件

模块化：从代码逻辑的角度划分，后端的模块化，方便代码分层开发，保证每个功能模块单一

组件化：从UI界面的角度划分，前端的组件化，方便UI组件的重用



局部组件、全局组件

~~~html
<div id="app"></div>

<script>
// 全局组件
// <slot></slot>内置组件，承载分发内容的出口，插槽：登录、播放、删除
Vue.component('Vbtn', {
    template:`<button><slot></slot></button>`
})
    
    
// 生子2
var Vheader = {
    // 全局组件直接用
    template:`<div class="head">我是头部组件<Vbtn>登录<Vbtn></div>`
}     
    
var Vcontent = {
    template:`<div class="content">我是content组件<Vbtn>播放<Vbtn></div>`
}    
    
//     
var Vaside = {
    template:`<div class="aside">我是aside组件<Vbtn>删除<Vbtn></div>`
}    
// 1.声明局部组件：声子1
var App = {
    // 用子2
     template: `
        <div class="main">
            <Vheader/>
            <div class="main2">
                <Vaside/>
                <Vcontent/>
            </div>
        </div>
        `,
    data(){
        return {
        }
    },
    // 子组件直接挂载在根组件中，无需在实例中挂载：挂子2
    components:{
        Vheader,
        Vcontent,
        Vaside
    }
}
new Vue({
    el:'#app',
    // 3.使用组件，也可以在页面上直接用：用子1
    // 即将下一行的模板注释掉，直接在**页面控制区域**加上<App></App>
    template:`<App></App>`,
    data(){return{}},
    // 2.挂载组件：挂子1
    components:{App}
    
})
</script>
~~~

​	

// 给vue组件绑定事件时候，必须加上native ，不然不会生效（监听根元素的原生事件，使用 .native 修饰符）

// 等同于在自组件中：子组件内部处理click事件然后向外发送click事件：$emit("click".fn)



父组件通信子组件

> 1.父组件中返回数据
>
> 2.父组件中绑定自定义属性
>
> 3.子组件声明属性名
>
> 4.子组件操作需要的数据

​		在 Vue 中，父子组件的关系可以总结为 props down, events up。父组件通过 props 向下传递数据给子组件，子组件通过 events 给父组件发送消息。

​		静态props:子组件要显式地用 props 选项声明它期待获得的数据, 静态Prop通过为子组件在父组件中的占位符添加特性的方式来达到传值的目的

​		动态props: 在模板中，要动态地绑定父组件的数据到子模板的 props，与绑定到任何普通的HTML特性相类似，就是用 v-bind。每当父组件的数据变化时，该变化也会传导给子组件



### slot

<slot></slot>具名插槽的渲染顺序，完全取决于模板, 而不是取决于父组件中元素的顺序, 若子组件模板中solt存在，且父组件含有solt的name为空的标签，会把这些标签合都渲染，无论多少。



### keep-alive

包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们 ,  它自身不会渲染一个 DOM 元素，也不会出现在组件的父组件链中。  当组件在 <keep-alive></keep-alive>内被切换，它的 `activated` 和 `deactivated` 这两个生命周期钩子函数将会被对应执行。



## Vue-router

传统路由: url改变,立刻发出请求, 响应页面, 可能资源过多. 页面容易出现白屏

SPA: 单页面应用single  page application, 依靠锚点值得改变, 不会立刻发出请求, 而是在某个合适的时机, 发送ajax请求, 局部改变数据

对于单页面应用程序，主要通过URl中的hash（#号）来实现不同页面的切换；同时，hash有一个特点：HTTP请求中不会包含hash相关的内容；所以单页面应用主要用hash实现。

后端路由：对于普通网站，所有超链接都是URL地址，对应着服务器上的资源。



+ vue-router插件依赖vue.js, 引入在vue.js之后
+ 如果是局部作用域下的Vue则必须写Vue.use(VueRouter)
+ 生成实例new VueRouter并创建匹配规则
+ path就是路由, name是重命名路由, component是依赖的路由组件
+ 抛出两个全局组件router-link, router-view
+ 路由对象关联

~~~javascript
// 定义路由组件
var Login = {
      template:`<div>我是登录页面</div>`
    }
var Register = {
      template:`<div>我是注册页面</div>`
    }

const router = new VueRouter({
      routes:[
        // 路由匹配规则
        {
          path:'/login',
          // 路由重命名
          name:'loogin',
          component:Login
        },
        {
          path:'/register',
          component:Register
        }
      ]
    })

// router-link默认渲染成a, to渲染成href
//  命名路由, 在router-link中绑定to(加:),并修改
// router-view用来渲染路由组件
var App = {
      template:`
      <div>
        <router-link :to ="{name:'loogin'}">登录</router-link>
        <router-link to ='/register'>注册</router-link>
        <router-view></router-view>
      </div>
      `,
    }

new Vue({
      el:'#app',
      template:`<App />`,
      // 将配置好的路由对象关联到实例化对象中
      router:router,
      components:{
        App
      }
    })
~~~

路由参数设置

嵌套路由

动态路由匹配





## PWA(渐进式/web/application)

积极研究其中应用场景

+ 离线浏览
+ 桌面应用图标
+ 顶部通知栏
+ 预缓存(页面未启动加载)
+ 骨架屏
+ 分享

