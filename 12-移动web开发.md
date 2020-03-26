

# 移动web开发





<h1 style="color:red">Hello</h1>
## 屏幕

对角线长度：3.5"/ 4.7"/ 5.5"（英寸in）

分辨率：垂直x水平： 1920*1080；（px）

长度单位：px，em，pt等

像素密度，每英寸能显示的像素     ppi ：勾股定理：长宽的平方和 / 对角线长度

设备的独立像素：不同ppi共存的设备，原始图片在ppi高的设备上看上去变小了

+ 单位：安卓设备叫dp
  + ​	ios上叫pt
+ 获取设备独立像素的值```alert(window.divicePixelRatio)```  

物理像素：```window.screen.width   window.screen.height``` 就是屏幕的分辨率，宽高

css像素：放大缩放，100%

2倍图：在低ppi上用smaller图，在高ppi设备上显示需要放大，这样图像就变模糊或者有锯齿了，所以需要准备更大的图			 来显示；也有3倍，4倍等.



## 调试

chrome 真机调试

+ ```bash
  npm i -g live-server
  ```

+ 手机打开开发者选项,  安装google浏览器App,  打开  

+ 打开项目live-server,  PC google浏览器F12>more tools>remote devices>可能需要手机端验证等

+ 打开手机浏览器, 输入live-server的localhost>ok

+ 可以利用投屏软件在pc端显示, 或者继续用chrome,devtools的inspect显示操作等等



spy-debugger

npm







## 视口

viewport

~~~html
<script>
    var clientWidth = document.documentElement.clientWidth;
    var clientWidth = document.documentElement.clientHeight;
    console.log(clientWidth, clientHeight)
    // pc视口就是浏览器的大小
    // 移动端视口会默认设置一个很大的视口
</script>
~~~

屏幕适配

移动页面最理想的状态,避免滚动条且不被默认缩放处理

~~~html
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
// 移动端
并改变浏览器默认的layout viewport的宽度
~~~





## 移动端事件

触屏事件 - - - - adeventListener 添加事件监听

touch事件的触发必须保证元素有**宽高值**

touchstart : 手指触摸屏幕触发 (一次)

touchmove :  手指滑动触发 ( 持续触发事件 )

touchend : 手指离开屏幕触发 (一次)

touchcancel : 意外中断触发 ( 例如突然手机来电 )

~~~javascript
.adEventListener("touchstart", function(){
    console.log(...);
})
~~~

touchevent对象

~~~javascript
.adEventListener("touchstart", function(e){
    console.log(e);
    console.log(e.targetTouches);
    console.log(e.targetTouches[0].clientX + "---" + e.targetTouches[0].clientY);
})
// e对象有3个属性
/*
touches 当前屏幕的所有手指对象
targetTouches  当前元素的手指对象 (touches和targetTouches在测试中没有区别)
changedTouches 当前屏幕变化的手指对象--从无到有,或者从有到无

手指对象的坐标值
clientX/clientY -- 手指相对元素的坐标值
screenX/screenY -- 手指相对屏幕的坐标值
pageX/pageY -- 手指相对内容的坐标值
*/

~~~

拖拽操作

~~~javascript
var div = document.querySelector("div");
var startX, startY, moveX, moveY, distanceX, distanceY;

div.addEventListener("touchstart", function(e){
    // 获取手指触摸时的坐标
    startX = e.targetTouches[0].clientX;
    startY = e.targetTouches[0].clientY;
})
div.addEventListener("touchmove", function(e){
    // 记录手指滑动过程中的坐标
    moveX = e.targetTouches[0].clientX;
    moveY = e.targetTouches[0].clientY;
    // 计算差异
    distanceX = moveX - startX;
    distanceY = moveY - startY;
    // 设置偏移
    div.style.transform = "translate("+distanceX+"px, "+distanceY+"px)";
})
~~~

~~~javascript
// 为document 添加事件,可以捕获当前响应元素
// e.target 事件源参数指向响应的元素
var startX, startY, moveX, moveY, distanceX, distanceY;

document.addEventListener("touchstart", function(e){
    console.log(e.target)
    // 获取手指触摸时的坐标
    startX = e.targetTouches[0].clientX;
    startY = e.targetTouches[0].clientY;
})
document.addEventListener("touchmove", function(e){
    // 记录手指滑动过程中的坐标
    moveX = e.targetTouches[0].clientX;
    moveY = e.targetTouches[0].clientY;
    // 计算差异
    distanceX = moveX - startX;
    distanceY = moveY - startY;
    // 设置偏移
    e.target.style.transform = "translate("+distanceX+"px, "+distanceY+"px)";
})
~~~



## 媒体查询

超小屏幕 ：w<768px

小屏设备：>=768-992

中等屏幕：>=992-1200

宽屏设备：>=1200

~~~html
媒体功能device-width / max-width
<style>
    body {
        background-color: red
    }
    @media screen and (max-width:768px) {
        body {
            background-color: green
        }
	}
    @media screen and (min-width:768px) and (max-width:992px){
        body {
            background-color: blue
        }
	}
    /*   1.@media关键字
     *	 2.媒体类型：all/print/screen/speech
     *	all用于所有设备
     *	print用于打印机或打印预览
     *	screen用于电脑平板智能手机等
     *	speech用于屏幕阅读器等发声设备
     *   3.媒体功能
     *	device-width / max-width (主要常用)
     * 	and 条件关联
     */
</style>


~~~

条件覆盖样式

~~~html
<style>
    /* 向上兼容，向下覆盖 */
    /* 如果是判断最小值，应该从小到大写 */
    /* 如果是判断最大值，应该从大到小写 */
    body {
        background-color: red
    }
    @media screen and (min-width:768px) {
        body {
            background-color: green
        }
	}
    @media screen and (min-width:992px) {
        body {
            background-color: blue
        }
	}
    @media screen and (min-width:1200px) {
        body {
            background-color: pink
        }
	}
    
</style>
~~~

min-width：在pc和移动端都能响应

min-device-width：在pc端是不能响应的，指当前设备的宽度，在移动端是可以响应的

媒体查询还可以针对不同的媒体使用不同样式

~~~html
<link rel="stylesheet" href="a.css">
<link rel="stylesheet" media="screen and (min-width:992px) and (max-width:1200px)" href="b.css">
~~~

not：取反，写在and之前



## 移动端适配方案

### 传统百分比+固定高度布局

固定屏幕为理想视口

少许媒体查询设置字体

水平百分比布局或用弹性布局

### web-em/rem 适配

em：长度单位，参照当前元素字号，如果没有设置，则参考父元素直到浏览器默认字号

rem：c3新增，参照根元素（html）的字号，html默认为浏览器默认字号



rem实现适配的说明

~~~html
ui稿件640px---->人为划分20份，每份32px
元素320px*320px---->占比10份

移动端320px---->人为划分20份，每份16px
元素为了占比10份---->实际160px*160px

移动端360px---->人为划分20份，每份18px
元素为了占比10份---->实际180px*180px
<style>
    @media screen and (device-width:320px) {
		html {
			font-size:16px; /* 划分20份, 每份16px=320px/20*/
		}
	}
	@media screen and (device-width:360px) {
		html {
			font-size:18px;
		}
	}
    @media screen and (device-width:375px) {
		html {
			font-size:18.75px;
		}
	}
	@media screen and (device-width:414px) {
		html {
			font-size:20.07px;
		}
	}
    /* 元素的单位 */
    div {
        width:10rem;/*元素的大小跟随设备屏幕宽度改变以适配，10份=160px/16px*/
    }
</style>

~~~



## 移动端click延迟

移动设备需要判断用户是单击还是双击。大约300ms的延迟



## 移动端tap事件

移动端的单击事件也叫tap

单击，双击，长按

单击：一根手指，判断手指开始触摸和松开的时间差异（指定值），保证没有滑动（若有抖动，必须保证抖动距离在一定范围内）

~~~javascript
var div = document.querySelector("div");
var startX,startY,starTime
div.addEventListener("touchstart", function(e){
    if(e.targetTouches.length > 1){ // 不止一只手指
        return;
    }
    starTime = Date.now();
    startX = e.targetTouches[0].clientX;
    startY = e.targetTouches[0].clientY;
    // 这里可以做一些与事件相关的初始化操作
})

/* touchend松开手指已经没有手指了，不能再用targetTouches */
div.addEventListener("touchend", function(e){
    if(e.changeTouches.length > 1){ 
        return;
    }
    if (Date.now()-startTime > 150){ // 长按操作
        return;
    }
    /* 判断松开手指时坐标与触摸时坐标差异 */
    var endX = e.changeTouches[0].clientX;
    var endY = e.changeTouches[0].clientY;
    if(Math.abs(endX-startX)<6 && Math.abs(endY-startY)<6){
        console.log("移动端单击事件");// 这里执行tap响应后的处理
    }
})
~~~

封装移动端的tap事件，

~~~javascript
var itcast = {
    /* dom: 传入dom元素，让我们为任意的元素添加tap事件 */
    tap:function(dom, callback){
        if(!dom || typeof dom!="object"){
            // 判断是否传入dom元素或者dom不会一个对象
            return;
        }
        // var div = document.querySelector("div");
		var startX,startY,starTime
		dom.addEventListener("touchstart", function(e){
    		if(e.targetTouches.length > 1){ // 不止一只手指
       		    return;
    		}
    		starTime = Date.now();
    		startX = e.targetTouches[0].clientX;
    		startY = e.targetTouches[0].clientY;
    		// 这里可以做一些与事件相关的初始化操作
		})

		/* touchend松开手指已经没有手指了，不能再用targetTouches */
		dom.addEventListener("touchend", function(e){
    		if(e.changeTouches.length > 1){ 
        		return;
    		}
    		if (Date.now()-startTime > 150){ // 长按操作
        		return;
    		}
    		/* 判断松开手指时坐标与触摸时坐标差异 */
    		var endX = e.changeTouches[0].clientX;
    		var endY = e.changeTouches[0].clientY;
    		if(Math.abs(endX-startX)<6 && Math.abs(endY-startY)<6){
        		console.log("移动端单击事件");
                // 这里执行tap响应后的处理
                // 判断用户是否传入回调函数。&&前面为假时后边就不执行了。
                callback&&callback();
    		}
		})
	}
}
~~~

zepto封装tap事件（touch事件）

点击穿透（点透）



fastclick插件（单击事件）

解决点透。。。

touch事件只能在移动端反应，而此插件可以让click在pc端响应，且不延迟（或延迟小）



iscroll插件（滚动）

固定结构使用。。。



swipe插件（轮播图）

固定结构使用。。。

固定样式。。。



swiper插件（轮播图等，，）