# Node.js





[toc]





## 开发前准备

### 多版本的安装

删除原有node.js及nvm

 https://github.com/coreybutler/nvm-windows/releases  下载   [nvm-noinstall.zip](https://github.com/coreybutler/nvm-windows/releases/download/1.1.7/nvm-noinstall.zip)

c 盘创建 dev 文件夹，dev下创建 nvm 和 nodjs 两个文件夹，上述解压至 nvm

右键管理员方式运行 install.cmd，直接回车生成 settings.txt 另存在 nvm 文件夹下

~~~text
root:  C:\dev\nvm
path:  C:\dev\nodejs
arch: 64 
proxy: none
~~~

配置环境变量

~~~text
NVM_HOME   C:\dev\nvm
NVM_SYMLINK   C:\dev\nodejs
~~~

把配置好的环境变量添加到path中

~~~text
%NVM_HOME%
%NVM_SYMLINK%
~~~

测试 cmd命令行窗口：nvm version  npm -v



### 安装node.js环境

nvm list 查看安装的node列表

nvm install <version> 

nvm install latest 安装最新版本

nvm install 6.10.3

nvm uninstall 。。。

nvm use 。。。之后nodejs文件夹就会指向正在用node版本的文件夹



node -v  查看当前使用的node版本



命令行方式：node进去REPL模式，read-eval-print-loop，读取执行打印循环

~~~JavaScript
node
> 1 + 1
2
> _ + 1
3
> _ + 3
6
// REPL模式下 _ 表示上次执行的结果
.exit // 退出
// 也可以写成一个js文件
// 用node xxx.js执行
~~~



## Node

node中是没有window对象的，取而代之的是Global Objects

全局成员概述：

~~~javascript
console.log(__filename)
console.log(__dirname)
~~~