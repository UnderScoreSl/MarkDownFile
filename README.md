README

[toc]

# webpack项目搭建

项目名 : webpackTool  (不要直接用webpack命名) 

## 确保安装node/npm

~~~bash
node.js@12.14.1(已安装版本)
npm i -g nrm(安装镜像源)
nrm ls(查看镜像源)
nrm use ... (切换镜像源)

// 4.x 的安装, 必须要安-cli, 可以先安装个全局的, 不想要也可以删掉
npm i webpack -g
npm i webpack-cli -g

npm init -y (环境下(项目文件夹下执行)安装, 先初始化)
npm i -D webpack@4.x.x
npm i -D webpack-cli
~~~

### 当前目录结构

~~~diff
webpack-demo
  |- package.json(配置安装包, 及npm命令配置区)
~~~



### 创建目录结构

<src>  为开发时编写的代码区         <dist>  为最终项目输出区

可根据自身项目修改

~~~diff
webpack-demo
  |- package.json
+ |- /dist
+ |- index.html
+ |- /src
+   |- /js
+     |- index.js
+   |- /css
+     |- style.css
+   |- /img
+   |- /font
~~~

---



## 使用npm脚本

修改package.json中的script脚本中添加

~~~json
{
    ...
    "script":{
        ...
        "build": "webpack",      
        "start": "webpack-dev-server --open"  
    }
}
  
~~~

~~~bash
npm run build  // 运行打包时命令(手动编译)
npm run start  // 运行热加载模块(开发工作时实时编译)---(热加载模块插件)
~~~

---



## 创建配置文件

环境下创建webpack.config.js

~~~javascript
const path = require('path');

module.exports = {
  // 根据项目目录配置
  entry: './src/js/index.js',   // 入口js
  output: {
    filename: 'bundle.js',	   // 出口js, 将来执行打包编译, 自动生成的文件, 可以先创建
    path: path.resolve(__dirname, 'dist')  // 输出目录, 根(环境)下dist, 可以先创建
  }
};
~~~

---



## 安装资源插件

其余插件用法慢慢搜吧

~~~bash
npm i -D css-loader style-loader file-loader url-loader(主要包含加载css文件, 图片, font字体的插件)
~~~

### 修改入口文件

~~~javascript
// index.js
import '../css/style' (在主文件index.js中(入口文件), 中导包)
~~~

### 修改配置文件

~~~javascript
module.exports = {
    ...
    // 加载模块的规则, 具体怎么写看插件的用法
    // 以下分别是css, 图片, 字体文字的加载方式
    module: {
    rules: [{
      test: /\.css$/,
      use: [
        'style-loader',
        'css-loader'
      ]
    }, {
      test: /\.(png|svg|jpg|gif)$/,
      use: [
        'url-loader' // 小于8kb的base64编码, 大于的直接输出到dist文件夹
      ]
    }, {
      test: /\.(woff|woff2|eot|ttf|otf)$/,
      use: [
        'file-loader'
      ]
    }]
  }
}
~~~

---



## 热加载模块插件

~~~bash
npm i -D webpack-dev-server
~~~

### 修改配置文件

~~~javascript
module.exports = {
    ...
    devServer: {
    contentBase: "dist", // 默认服务于根路径下的index.html, 如果原本就在根路径则不需要配置该选项
    compress: true,
    port: 8090  // 默认8080, 可以修改指定
  }
}
~~~





## 安装必要插件

```bash
npm i -D html-webpack-plugin clean-webpack-plugin (一个是定义模板html插件, 一个是清空dist插件)
```

### 修改配置文件

webpack.config.js / 各个版本的配置有出入注意

~~~javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const {CleanWebpackPlugin} = require('clean-webpack-plugin');

module.exports = {
    ...
	plugins: [
	   new webpack.ProgressPlugin(),
	   new CleanWebpackPlugin(),
	   new HtmlWebpackPlugin({
	     template: "./src/index.html"   // 模板直接引用自建的index
	   })
	]
}
~~~

---



## 使用jQuery说明

~~~bash
npm i -D jquery
~~~



~~~javascript
// 配置文件webpack.config.js---项目全局使用jquery
const webpack = require('webpack')

module.exports = {
    ...
	plugins: [
        ...
        new webpack.ProvidePlugin({
      		$: "jquery",
      		jQuery: "jquery",
      		"windows.jQuery": "jquery"
    	})
    ]
}
// 之后如果想使用bootstrap的js插件, 就可以先复制预编译的代码到src中, 再去index.js中import就ok
~~~

