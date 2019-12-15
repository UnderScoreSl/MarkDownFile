# Less

[toc]

## 安装

npm install -g less: 联网装

lessc -v检测是否安装成功

lessc  less文件路径  新文件.css；；可以直接编译

一般设置：settings--tools --file watchers--改变 lessc 安装的路径  C:\dev\nvm\v13.2.0\lessc，可以即时编译，无需手动

~~~less
/* 注释：第一种与css相同，*/
// 也可以用双斜杠，当双斜杠注释不会被编译到css文件中
/* 变量：@变量名：值 */
@baseColor: #e92312;
a{
  color: @baseColor;
}


/* 混入: 可以将一个样式引入到另一个样式中，类似函数的调用 */
/* 不加变量和参数的情形
.addRadius {
  border-radius: 10px;
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
}
*/

/* 选择器后可以加变量及参数，参数为默认值，混入时不传参就按默认值 */
.addRadius(@r:10px) {
  border-radius: @r;
  -webkit-border-radius: @r;
  -moz-border-radius: @r;
}
div {
  width: 200px;
  height: 200px;
  .addRadius(5px);
}


/*嵌套: 实现选择器的继承，减少代码量，代码结构更清晰*/

/* 之前在css中的写法
.jd_banner{}
.jd_banner > div{}
.jd_banner > div >h3{}
.jd_banner > div >h3:before{}
.jd_banner > div >a{}
.jd_banner > div >a:hover{}
*/
/* 这里无法折叠，不太好看 */
.jd_banner {
  width: 100%;
  height: 200px;
  .addRadius(5px);
  >div{
    // &用于串联选择器，不写就会产生空格
    &::before{
      content:"";
    }
    // 直接子元素可以加尖括号，不加就是后代元素
    width: 100%;
    >a{
      text-decoration: underline;
      &:hover{
        text-decoration: none;
      }
    }
  }
}

~~~

