

[toc]





## 性能优化



### 资源合并与压缩

web前端本质上是GUI软件，基于BS架构，代码发布到webserver

深入理解http请求的流程是前端性能优化的核心

请求的流程，url通过浏览器请求，dns解析，网络，服务端，处理，网络，渲染到浏览器生成页面

+ dns优化缓存减少dns查询时间
+ 网络请求过程最佳网络环境
+ 相同静态资源是否可以缓存，cdn（静态资源）
+ 减少请求大小，次数
+ 服务端渲染

html压缩

+ 不显示的字符（空格，制表符，换行符），注释
+ 方式：在线网站压缩，工具（nodejs等提供）压缩，后端模板引擎渲染压缩

css压缩

+ 无效代码（注释，无效字符）
+ 语义合并
+ 方式：跟html差不多

js压缩

+ 无效字符
+ 剔除注释
+ 语义缩减优化
+ 代码保护（降低代码可读性）

文件合并

+ 存在问题：首屏渲染问题，缓存失效问题
+ 建议：公共库合并，不同页面合并



### 图片优化

JPG有损压缩，压缩率高，不支持透明

png兼容好

png8（2^8色，一个像素8bit支持透明）

png24（2^24，一个像素24bit，不支持透明）

png32（2^24，一个像素24bit，支持透明）

webp安卓很好用

svg矢量图，代码内嵌，相对小，放大不会马赛克，图片样式相对简单的业务场景



+ 真实图片舍弃不必要的色彩
+ css雪碧图（整合图片），减少http请求
+ image inline base编码图片内容内嵌到html，减少请求数量

~~~javascript
var inlineImage = __inline('./image/~.webp')
var TEMPLATE = '<img src=" '+inlineImage+'" />'
// 应用vue渲染
~~~

+ 矢量图svg，iconfont解决icon问题
+ 安卓下使用webp



### 页面加载过程

+ 顺序执行，并发加载

  + 词法分析

  + 并发加载（并发请求）
  + 并发上限

+ 是否阻塞

  + css阻塞：css阻塞页面渲染，阻塞js执行，不阻塞外部脚本的加载
  + js阻塞：直接引入js阻塞页面渲染（页面结构），不阻塞资源的加载，顺序执行，阻塞后续js逻辑的执行

+ 依赖关系

+ 引入方式



### 懒加载和预加载

懒加载

+ 图片进入可视区域之后请求图片资源
+ 对电商等图片很多，页面很长的业务场景适用
+ 减少无效资源的加载
+ 并发加载资源过多会阻塞js的加载

~~~html
js懒加载实现
<div class="image-list">
    <img src="" class="image-item" lazyload="true" data-original="http://..."/>
	<img src="" class="image-item" lazyload="true" data-original="http://..."/>
    ...
</div>
<script src="./lazyload.js"></script>
~~~

~~~ javascript 
var viewHeight = document.documentElement.clientHeight // 可视区高度
function lazyload (){
    var eles = document.querySelectorAll('img[data-original][lazyload]')
    Array.prototype.forEach.call(eles, function(item, index){
        var rect
        if (item.dataset.original ==="")
            return
        rect = item.getBoundingClientRect()
        
        if(rect.bottom>=0&&rect.top<viewHeight){
            !function(){
                var img = new Image()
                img.src = item.dataset.url
                img.onload = function (){
                    item.src = img.src
                }
                item.removeAttribute('data-original')
                item.removeAttribute('lazyload')
            }()
        }
    })
}

lazyload()
document.addEventListener('scrool', lazyload)
// zepto懒加载库
~~~



预加载

+ 对资源的提前请求，缓存

~~~javascript
// 1.html标签
// <img src="http://..." style="display:none">

// 2.使用Image对象
var image = new Image()
image.src = "http://..."
// 3.使用ajax请求
// preload库
~~~



### 重绘与回流

css性能让js变慢吗？会

UI频繁渲染，最终导致js变慢

回流

+ 渲染树的元素规模尺寸，布局，隐藏等改变就需要重新构建，这就会回流
+ 页面布局和几何属性改变就需要回流

重绘

+ 渲染树元素需要更新属性，这些属性只影响元素外观，风格而不影响布局比如background-color，就会重绘
+ 回流必将引发重绘，重绘不一定会引发回流



触发页面重布局属性

+ 盒子模型相关属性（width，height，padding，margin，display，border-width，border，min-height）
+ 定位浮动属性（top，left，right，bottom，position，float，clear）
+ 改变节点内部文字结构（text-align，font-weight，font-size。。。）



触发重绘属性

+ color，background等
+ border-style，border-radius
+ text-decoration，outline，outline-color，outline-width等
+ visibility
+ box-shadow



优化方法

+ + 代替方案
  + 频繁回流单独图层（图层不能被滥用，消耗性能）
    + 给dom元素新建图层：transform：translateZ(0)；will-change：transform。
    + transform：translateY(0)，js修改translate代替top改变
    + opacity代替visibility(先给dom新建图层transform：translateZ(0))；visibility会触发重绘
    + 不要一条一条修改dom样式，预定义好所有class样式，直接修改dom的className覆盖原有样式
    + dom离线修改：就是给dom的display：none，修改100次，再显示
    + 不要把dom节点的属性值放在一个循环里当做循环的变量offsetHeight，offsetWidth。。
    + 不要使用table布局
    + 动画实现速度选择
    + 动画可以新建图层
    + 启用GPU加速
    + video标签就是单独图层



### 浏览器存储

localstorage、cookie、sessionstorage、indexdb、、pwa、service workers

cookie

+ 浏览器和服务端交互
+ 客户端自身数据存储

cookie去维护客户端状态，大小4kb左右，过期时间（expire）

cookie存储能力被localstorage取代，httponly，cdn流量损耗（cdn域名和主站域名要分开）



localstorage

5M左右，客户端使用，不与服务端通信，浏览器本地缓存方案，提升渲染速度



sessionstorage

5M左右，客户端使用，不与服务端通信，关闭标签，就会消失，刷新不消失，对表单的维护。



IndexDB

存储客户端结构化数据



pwa没有网也能提供基本的页面访问lighthouse测试



### 缓存

### 服务端性能优化

构建层模板编译

数据无关的prerender方式

服务端渲染