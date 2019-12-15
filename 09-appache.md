# appache



## wamp

deny from all == allow



documentroot == 修改固定访问路径





## 多站点

virtual hosts 去掉#

以及修改对应的文件配置



主机hosts修改

127.0.0.1 localhost // 添加映射

127.0.0.1 域名





## 静态网站

维护性低，没有交互性，与动态网站区分，是不是动态生成的（页面或者数据）





## PHP语法

### 变量的声明：

​	字母数字下划线组成，数字不能开头，区分大小写，和js一致

~~~php
<?php
    // 所有php代码都放在这一对标签中
    $num = 123;
	// echo输出字符串数据
	echo $num;

	// php字符串拼接为英文的.
	echo '编号为：'.$num;
	echo '<div>编号为：'.$num.'</div>';

	// php单引号将其中的变量视为字符串
	echo '<div>编号为：$num</div>';

	// php双引号才将其中的变量视为变量
	echo "<div>编号为：$num</div>";

	// php数组不能用echo输出，print_r() 可以打印复杂数据类型
	// echo只能打印数组中的数据, 访问时一般用数组中的键来访问
	$arr = array(1,2,3,4,5);
	echo $arr[1];
	echo '<br />';
	print_r($arr);
		
	// 自定义键
	$arr1 = array("username":"zhangshan", "age":"12", "geder":"male");
	echo $arr1["username"];
	var_dump($arr1); // var_dump() 打印详细信息
	
?>
~~~

js中单双引号相同，只有json格式数据必须使用双引号

~~~js
var json = '{"username":"zhangshan", "age":"12", "gender":"male"}'; // 定义一个json格式的字符串
var obj = JSON.parse(json); // 创建json对象接受要解析的数据字符串
console.dir(obj);
~~~



### 预定义变量

~~~php+HTML
<?php
$_GET;
$_POST;
?>
~~~





### 数据类型

字符串，整型，浮点型，布尔型，数组，对象，NULL



### 函数

~~~php+HTML
<div>
    <?php
    // 自定义函数，处理方式和js很像
    function foo($info){
        return $info;
    }
    $ret = foo('hi');
    echo $ret;
    
    // 系统函数
    $arr = array("a"=>111, "b"=>"zhangshan","c"=>333);
    $tmp = json_encode($arr);  // 对变量进行 JSON 编码, 返回json格式的对象
    echo $tmp;
    ?>
</div>

~~~



## 后台接口

~~~php
<?php
// 后台接口一般都是返回特定格式的数据
    echo "1"; // 这就是最简单的，返回一个字符串
	header('Content-Type:text/html; charset=utf-8');
	echo "<div>test</div>"; //也可以返回一个片段html，不过要先定义返回的是网页
	// 也可以定义一个数组，或者json格式的数据字符串
	// json_encode($arr) 将数组或对象转换成json格式的字符串
	$arr = array(123, 234);
	// 也定义稍微复杂的业务逻辑
	$f = $_GET['flag'];
	if ($f == 1){
        echo "123";
    }else{
        echo "345";
    }

?>
~~~



## 异步效果与 *js*事件处理机制

单线程 + 事件队列

事件队列中事件的执行条件：

+ 主线程已经空闲
+ 任务满足触发条件
  + 1.定时函数，（延时时间到达）
  + 2.事件函数，（特定事件被触发）
  + 3.ajax的回调函数，（服务器端有数据响应）

## Ajax跨域

同源：请求url地址的的协议，域名，端口都相同。只要有一点不同，就是跨域。

同源策略：浏览器不允许ajax跨域获取服务器数据，保证其安全性。

跨域如何解决：

+ jsonp首选
+ document.domain+iframe
+ location.hash+iframe
+ window.name+iframe
+ window.passMessage
+ flash等第三方插件