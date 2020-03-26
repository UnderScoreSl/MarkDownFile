# nrm

npm i nrm -g

nrm提供常用下载源

nrm ls查看可用下载源



修改下载源

nrm use npm

nrm use cnpm



nrm只是提供镜像地址



## 网页常见静态资源

js文件：.js	.jsx	.coffee	.js

样式文件：.css	.less	.scss(.sass：用sass编写的后缀名已经都用scss)

图片文件：.jpg	.png	.gif	.bmp	.svg

字体文件：.svg	.ttf	.eot	.woff	.woff2

模板文件：.ejs	.jade	.vue(webpack中定义组件的方式)



## 网页加载问题

+ 加载速度慢，发起二次请求
+ 处理错综复杂的依赖关系

## 解决

合并，压缩，精灵图，图片的base64编码，requireJS，webpack解决复杂依赖关系

webpack基于整个项目构建（更宏观），gulp基于task任务构建（更简洁，灵活任务处理）