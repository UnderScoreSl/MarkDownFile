# VUE

[toc]

VUE  Angular  React

vue只关注图层，框架提高开发效率

提高开发效率：原生js开发 ==> jQuery类库 ==> 前端模板引擎 ==>  vue等框架

浏览器兼容问题 ==>  频繁操作dom元素 ==>  ==> 减少不必要的dom操作，提高渲染效率，双向数据绑定的概念，更多关注业务逻辑

框架：是一套完整的解决方案，对项目的侵入性较大；库（插件）：提供某一个小功能，易切换（从jq到zp，从ejs到art-template）



## MVC和MVVM

MVC是后端开发概念

MVVM是前端图层概念，M：V：VM；数据的双向绑定是VM提供的



app.js: 项目的入口模块，一切请求都要从这里进行处理；app.js没有处理路由分发的功能

router.js: 路由分发处理模块，职能单一不负责业务逻辑；

controller模块: 封装了具体的业务逻辑功能；

model层: 只负责操作数据库，执行数据的CRUD（create，read，update，delete）；

view视图层：