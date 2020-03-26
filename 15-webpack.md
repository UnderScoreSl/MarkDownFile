---
webpack
grunp
gulp
---





# webpack

+ 它的世界里所有文件都是模块（除了html）
+ 只能加载 js 和 json （v-1.n只能加载 js ）其它文件只能用 loader 进行转化加载



## 核心概念

entry (入口， 指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始 )

output (出口， 告诉 webpack 在哪里输出它所创建的 *bundles*，以及如何命名这些文件 )

loader ( 处理那些非 JavaScript 文件 )

 plugins (插件， 从打包优化和压缩 ， 处理各种各样的任务 ) 



## 安装

安装前配置好node.js，npm

（npm下载慢，可以安装cnpm直接替换npm命令，以下不在赘述，cnpm和npm用法完全相同）

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org

/* 还有种说法用以下设置npm的配置源，尚未试过 
   修改配置npm的dev环境setting
   arch: 64 
   proxy: none
   node_mirror: http://npm.taobao.org/mirrors/node/ 
   npm_mirror: https://npm.taobao.org/mirrors/npm/
*/
npm config set registry https://registry.npm.taobao.org
```

npm install webpack -g（4.0以上安装cli，官网有说明）

```bash
npm install webpack -g
cnpm install webpack -g
```

npm install --save-dev webpack@<version> （特定版本 局部安装，一般在项目文件夹安装）

~~~bash
npm install --save-dev webpack@3.12.0
cnpm install --save-dev webpack@1
webpack -v  // 查看webpack版本
~~~

在局部安装之前，先在项目文件夹下创建一个package.json文件

或者

~~~bash
npm init -y  // 项目文件夹不要有中文名
~~~

~~~json
{
    "name": "webpack_test",
    "version": "1.0.0"
}
~~~

打包命令：```webpack 源文件 目标文件``` 



## 配置文件

Project目录结构

```diff
webpack-demo
  |- package.json
+ |- webpack.config.js
  |- /dist
    |- index.html
  |- /src
    |- index.js
```

项目下创建：webpack.config.js（将官网上的内容复制下来）

~~~JavaScript
const path = require('path');

module.exports = {
  entry: './src/index.js',                // 入口文件的配置，源文件
  output: {								  // 出口文件的配置，目标文件
    filename: 'bundle.js',				  // 目标文件名
    path: path.resolve(__dirname, 'dist') // 输出路径：__dirname根路径(项目路径)，resolve是拼接
  }
};
// 以上都是可修改的，可按照自己的开发目录配置，当这些配置好了，打包时就直接执行"webpack"就可以了
~~~



关于css文件，图片文件（在局部环境中安装）

```bash
cnpm install --save-dev css-loader
cnpm install style-loader --save-dev

cnpm install --save-dev file-loader
cnpm install --save-dev url-loader
```

配置webpack.config.js

```javascript
module.exports = {
  entry: './src/js/entry.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist/js')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [ 'style-loader', 'css-loader' ]
      },
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192   // 小于8kb转换base64，大于8kb的可能解析不到
            }
          }
        ]
      }
    ]
  } // 配置样式和图片
};
```

index.html可以同bundle.js同目录，解决大于8kb解析不到的文件



## 热加载实现

webpack开发服务器工具webpack-dev-server

~~~bash
cnpm install --save-dev webpack-dev-server@2.9.0 //webpack3.12.0所用最新版本
~~~

配置webpack.config.js

~~~javascript
module.exports= {
    ...,
    devServer:{
    	contentBase: "dist/js/"  // 默认服务于根路径下的index.html,如果原本就在根路径则不需要配置该选项
	}
}
~~~



## 插件

下载安装要使用的插件

~~~bash
cnpm install --save-dev html-webpack-plugin
cnpm install --save-dev clean-webpack-plugin
~~~

配置webpack.config.js

~~~JavaScript
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports= {
    ...
    plugins:[
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({template: "./src/index.html"})
    ]
}
// 注意，这样的配置在执行webpack时dist文件夹会全部删除，重新打包
// 如果执行webpack-dev-server，dist文件夹会删除而不会生成新的文件？
~~~

处理ES6高级语法的babel插件

~~~bash
cnpm i -D babel-core babel-loader babel-plugin-transform-runtime
cnpm i -D babel-preset-env babel-preset-stage-0
// --save-dev 可以简写 -D  install 简写 i
~~~



## webpack3.12.0

关于生产与开发环境配置

~~~json
// package.json 修改script脚本, 如果是用npm init --yes生成的package.json,则如下
// 注意删除注释
{
  "name": "webpack3.12.0",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    // 修改此处脚本为,如下, 运行命令为 npm run dev / npm run build
    // 之后分别配置webpack的config文件
    "dev": "webpack --config ./webpack.config.dev.js",
    "build": "webpack --config ./webpack.config.prod.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^3.12.0"
  }
}
~~~

~~~javascript
// webpack.config.dev.js
module.exports = {
   entry: {
     "main": './main.js'
   },
   output: {
     filename: './build.js'
   },
    // 文件监视改动, 自动产出build.js
   watch: true
}
~~~

~~~javascript
// webpack.config.prod.js
module.exports = {
   entry: {
     "main": './main.js'
   },
   output: {
     filename: './build.js'
   }
}
~~~







## 注意

以上模块的安装，都可能出现不兼容的现象，如果你用webpack1.x.x或3.x.x版本，而服务器工具webpack-dev-server，css-loader等等没有指明版本（默认可能最新版本），就会出错或者无法解析等状况。





# Gulp

特点：任务化（统一接口）；基于流（数据流IO）

插件：访问英文官网

## 安装

 gulp-cli的目的是让你像使用全局程序一样使用gulp，但并不是在全局安装gulp。 

~~~bash
npm install gulp-cli -g 
~~~

~~~bash
npm install --save-dev gulp  // 与webpack相同
~~~

在局部安装时，先在项目文件夹下创建一个package.json文件

或者用

~~~bash
npm init -y
~~~

~~~javas
{
    "name": "gulp",
    "version": "1.0.0"
}
~~~



## 配置文件

项目文件夹下创建gulpfile.js（新版4.x.x）

~~~javascript
var gulp = require('gulp');

gulp.task('任务名', function () {
  
})

gulp.task('default', ['任务名']);
~~~



## 插件

gulp-concat：合并文件（js、css）

gulp-uglify：压缩js文件

gulp-rename：重命名

gulp-less：编译less文件

gulp-clean-css：压缩css

gulp-livereload：实时自动编译刷新



## 重要API

gulp.src(filePath/pathArr)

gulp.dist(dirPath/pathArr)

gulp.task(name, [deps], fn)

gulp.watch()监视文件变化



## 合并js

先安装包

~~~bash
npm i -D gulp-concat gulp-uglify gulp-rename
~~~

配置文件引入并注册合并

~~~JavaScript
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');

// 注册合并js
gulp.task('js', function () {
  return gulp.src('src/js/*.js')  		 // 浅度遍历
  // return gulp.src('src/js/**/*.js')   //深度遍历
    .pipe(concat('bulit.js'))  			// 临时合并
    .pipe(gulp.dest('dist/js')) 		// 创建输出（临时）
    .pipe(uglify())             		// 压缩
    .pipe(rename({suffix: '.min'})) 	// 重命名
    .pipe(gulp.dest('dist/js'))			// 最终的输出
})
// 注意：如果js中存在不使用的定义的函数和变量时，压缩时会自动将这些删除
~~~

## 构建less任务

~~~bash
npm i -D gulp-less gulp-clean-css
~~~

~~~javascript
var less = require('gulp-less');
var cssClean = require('gulp-clean-css');

// 注册转换less文件的任务
gulp.task('less', function () {
  return gulp.src('src/less/*.less')
    .pipe(less())     // 编译less文件为css文件
    .pipe(gulp.dest('src/css'))  // 输出到src的css文件夹
})
~~~



## 注册压缩合并css文件

~~~JavaScript
// 注册转换less文件的任务
gulp.task('css', function () {
  return gulp.src('src/css/*.css')
    .pipe(concat('build.css'))     // 合并css文件
    .pipe(rename({suffix: '.min'}))
    .pipe(cssClean({compatibility: 'ie8'}))
    .pipe(gulp.dest('dist/css'))  // 输出到src的css文件夹
})
~~~

## 异步执行合并任务

~~~bash
gulp.task('default', ['js', 'less', 'css']);
// 任务的 return 删除则变成同步执行
~~~



## 依赖关系

异步执行任务的机制可能出现【less】任务未执行完【css】任务就不能执行

less任务与css任务有依赖关系，这时可以在css任务上添加依赖关系

~~~JavaScript
gulp.task('css', ['less'], function () {
    ...
    // 此时less任务执行完才会执行css任务
})
~~~



## 压缩html（同上）

~~~bash
npm i -D gulp-htmlmin
参数
htmlMin({collapseWhitespace:true})
~~~



## 半自动编译

~~~bash
npm i -D gulp-livereload
~~~

注册监视任务

~~~JavaScript
var livereload = require('gulp-livereload');

gulp.task('watch', ['default'], function(){
    livereload.listen();
    gulp.watch('src/js/*.js', ['js']);
    gulp.watch(['src/css/*.css', 'src/less/*.less'], ['css']);
})

// 每个任务还要添加pipe
.pipe(livereload())
~~~

执行监视任务

~~~bash
gulp watch
~~~

## 全自动编译

实时热加载插件gulp-connect

自动打开server，，，open插件-----配置server任务时直接加：open( " 地址 " ) 

~~~bash
npm i -D gulp-connect
npm i -D gulp-open
~~~

~~~JavaScript
gulp.task('server', ['default'], function () {
  // 配置服务器选项
  connect.server({
    root: 'dist',
    livereload: true,
    port: 5000
  });
  open("http://localhost:5000");
  gulp.watch('src/js/*.js', ['js']);
  gulp.watch(['src/css/*.css', 'src/less/*.less'], ['css']);
})


// 每个任务还要添加pipe
.pipe(connect.reload())
~~~







## 下载打包插件

gulp-load-plugins

~~~bash
npm i -D gulp-load-plugins
~~~

配置

~~~javascript
var $ = require('gulp-load-plugins')();
// 这样其他的插件就不用引入
// 直接用$.调用插件的方法
两个方法注意
$.htmlmin
$.cleanCss
~~~





# Grunt

## 安装

~~~bash
npm i -g grunt-cli

npm i -D grunt
~~~

## 配置文件

~~~bash
npm init -y // 生成package.json
~~~

创建Gruntfile.js

~~~javascript
module.exports = function (grunt) {

  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
      }
    }
  });

  // 加载包含 "uglify" 任务的插件。
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // 默认被执行的任务列表。
  grunt.registerTask('default', ['uglify']);

};
~~~



## 合并js文件

~~~bash
npm i -D grunt-contrib-concat
~~~

配置文件

~~~JavaScript
grunt.initConfig({
  concat: {  // 任务名
    options: {
      separator: ';',
    },
    dist: {
      /* src: ['src/intro.js', 'src/project.js', 'src/outro.js'], 官网的配置
      dest: 'dist/built.js', */
        
      src: ['src/js/*.js'], // 根据个人项目配置
      dest: 'built/js/built.js',
    },
  },
});


// grunt任务执行时加载的插件
grunt.loadNpmTasks('grunt-contrib-concat');
// 注意：无用的变量和没有调用的函数也会被删除
~~~



## 多任务

~~~JavaScript
// 执行
grunt.registerTask('default', ['concat', 'uglify']);

// bash ==>  $grunt
~~~



## 同步执行任务

~~~JavaScript
grunt.registerTask('default', ['uglify', 'concat']);
// bash ==>  $grunt
// 发现先压缩后合并失败，说明任务的同步执行，只有concat先执行，uglify才能执行
~~~

## js语法检查

~~~bash
npm i -D grunt-contrib-jshint
// 安装用法与其他插件相同，不再赘述
~~~

在根目录下创建 .jshintrc 文件

~~~javascript
// 可以把.jshintrc看着json文件，复制后删除所有注释

{
  "curly": true,
  "eqnull": true,
  "eqeqeq": true,
  "expr": true,
  "immed": true,
  "newcap": true,
  "noempty": true,
  "noarg": true,
  "browser": true,
  "devel": true,
  "node": true,
  "boss": true,
  "undef": true, // 不能使用未定义的变量
  "asi": false, // 语句后必须有分号
  "predef": ["define", "BMap", "angular", "..."], // 预定义不检查全局变量
  }
}
~~~

配置

~~~javascript
grunt.initConfig({
    ...
	jshint: {
      	options: {
        jshintrc: ".jshintrc"  // 指定配置文件
      	},
      	built: ['Gruntfile.js', 'src/js/*.js']
	}
})

grunt.loadNpmTasks('grunt-contrib-jshint');
grunt.registerTask('default', ['concat', 'uglify', 'jshint']);
~~~

## css文件

~~~bash
npm i -D grunt-contrib-cssmin // 具体操作官网，执行压缩和合并的功能
~~~

## 自动化插件

~~~bash
npm i -D grunt-contrib-watch
~~~

配置

~~~javascript
watch: {   // 任务名
  scripts: {
    files: ['src/js/*.js', 'src/css/*.css'],
    tasks: ['concat', 'uglify', 'jshint', 'cssmin'],
    options: {
      spawn: false, // 变量更新， true：全量更新
    },
  },
},
~~~

~~~javascript
grunt.loadNpmTasks('grunt-contrib-watch');
grunt.registerTask('default', ['concat', 'uglify', 'jshint', 'cssmin', 'watch']);
~~~

有这种场景，项目不需要监视，上线的时候，需要把任务 'watch' 删除

~~~javascript
// 改写监视任务
grunt.registerTask('default', ['concat', 'uglify', 'jshint', 'cssmin']);
grunt.registerTask('mywatch', ['default', 'watch']);
// bash  ==>  $grunt mywatch  // 来监视开发过程中的项目
// 			 $grunt  // 来打包上线项目
~~~

注意：其实这种还是没有直接启动server舒服。回到网页还是要按刷新按钮