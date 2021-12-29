

# AJAX 简介

## 什么是 AJAX ？

AJAX = 异步 JavaScript 和 XML。

AJAX 是一种用于创建快速动态网页的技术。

通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

传统的网页（不使用 AJAX）如果需要更新内容，必需重载整个网页面。

有很多使用 AJAX 的应用程序案例：新浪微博、Google 地图、开心网等等。

## Google Suggest

在 2005 年，Google 通过其 Google Suggest 使 AJAX 变得流行起来。

Google Suggest 使用 AJAX 创造出动态性极强的 web 界面：当您在谷歌的搜索框输入关键字时，JavaScript 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表。

# AJAX - 创建 XMLHttpRequest 对象

XMLHttpRequest 是 AJAX 的基础。

## XMLHttpRequest 对象

所有现代浏览器均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）。

XMLHttpRequest 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

## 创建 XMLHttpRequest 对象的语法

```js
var xmlhttp = new XMLHttpRequest();
```

# 快速入门

```html
<script>
    // 1.创建ajax对象
    var xhr = new XMLHttpRequest();
    // 2.告诉Ajax对象要向哪发送地址，以什么样的方式发送请求
    // (1)请求方式 (2)请求地址
    xhr.open('get','http://localhost:3000/first');
    // 3.发送请求
    xhr.send();
    // 4.获取服务器端响应到客户端的数据
    xhr.onload = function() {
        // xhr.responseText 
         console.log(xhr.responseText);
    }
</script>
```

## 在服务器端创建路由

```js
// 引入express框架
const express = require('express');
// 创建web服务器
const app = express();

// 创建路由
app.get('/first',(req,res) => {
    res.send('Hello,ajax');
});

// 监听端口
app.listen(3000,()=>{
    console.log('服务器启动成功');
});

```

# 向服务器发送请求

XMLHttpRequest 对象用于和服务器交换数据。

## 向服务器发送请求

如需将请求发送到服务器，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法：

```js
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```

open(*method*,*url*,*async*)

规定请求的类型、URL 以及是否异步处理请求。

- *method*：请求的类型；GET 或 POST
- *url*：文件在服务器上的位置
- *async*：true（异步）或 false（同步）

send(*string*)

将请求发送到服务器。

- *string*：仅用于 POST 请求

## GET 还是 POST？

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。

然而，在以下情况中，请使用 POST 请求：

- 无法使用缓存文件（更新服务器上的文件或数据库）
- 向服务器发送大量数据（POST 没有数据量限制）
- 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

### GET 请求

一个简单的 GET 请求：

```js
xmlhttp.open("GET","demo_get.asp",true);
xmlhttp.send();
```

在上面的例子中，您可能得到的是缓存的结果。

为了避免这种情况，请向 URL 添加一个唯一的 ID：

```js
xmlhttp.open("GET","demo_get.asp?t=" + Math.random(),true);
xmlhttp.send();
```

如果您希望通过 GET 方法发送信息，请向 URL 添加信息：

```js
xmlhttp.open("GET","demo_get2.asp?fname=Bill&lname=Gates",true);
xmlhttp.send();
```

```html
<body>
    <input type="text" id="username"><br/>
    <input type="text" id="age"><br/>
    <input type="button" value="提交" id="btn">
    <script type="text/javascript">
        // 获取按钮元素
        var btn = document.getElementById('btn');
        // 获取姓名文本框
        var username = document.getElementById('username');
        // 获取年龄文本框
        var age = document.getElementById('age');
        // 为按钮添加点击事件
        btn.onclick = function() {
            // 创建ajax对象
            var xhr = new XMLHttpRequest();
            // 获取用户在文本框中输入的值
            var nameValue = username.value;
            var ageValue = age.value;
            // 拼接请求参数
            var params = 'username=' + nameValue + '&age=' + ageValue;
            // 配置ajax对象请求方式和请求地址
            xhr.open('get', 'http://localhost:3000/get?' + params);
            // 发送请求
            xhr.send();
            // 获取服务器端响应的数据
            xhr.onload = function() {
                console.log(xhr.responseText)
            }
        }
    </script>
</body>
```

### POST 请求

一个简单 POST 请求：

```js
xmlhttp.open("POST","demo_post.asp",true);
xmlhttp.send();
```

如果需要像 HTML 表单那样 POST 数据，请使用 setRequestHeader() 来添加 HTTP 头。然后在 send() 方法中规定您希望发送的数据：

```js
xmlhttp.open("POST","ajax_test.asp",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xmlhttp.send("fname=Bill&lname=Gates");
```

## url - 服务器上的文件

open() 方法的 *url* 参数是服务器上文件的地址。

该文件可以是任何类型的文件，比如 .txt 和 .xml，或者服务器脚本文件，比如 .asp 和 .php （在传回响应之前，能够在服务器上执行任务）。

## 异步 - True 或 False？

AJAX 指的是异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。

XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true。

对于 web 开发人员来说，发送异步请求是一个巨大的进步。很多在服务器执行的任务都相当费时。AJAX 出现之前，这可能会引起应用程序挂起或停止。

通过 AJAX，JavaScript 无需等待服务器的响应，而是：

- 在等待服务器响应时执行其他脚本
- 当响应就绪后对响应进行处理

### Async = true

当使用 async=true 时，请规定在响应处于 onreadystatechange 事件中的就绪状态时执行的函数：

```js
xmlhttp.onreadystatechange=function() {
  if (xmlhttp.readyState==4 && xmlhttp.status==200) {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
  }
}
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```

### Async = false

如需使用 async=false，请将 open() 方法中的第三个参数改为 false：

我们不推荐使用 async=false，但是对于一些小型的请求，也是可以的。

请记住，JavaScript 会等到服务器响应就绪才继续执行。如果服务器繁忙或缓慢，应用程序会挂起或停止。

**注释：**当您使用 async=false 时，请不要编写 onreadystatechange 函数 - 把代码放到 send() 语句后面即可：

```js
xmlhttp.open("GET","test1.txt",false);
xmlhttp.send();
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```

# AJAX - 服务器 响应

## 服务器响应

如需获得来自服务器的响应，请使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性。

### responseText 属性

获得字符串形式的响应数据。

```js
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```

### responseXML 属性

如果来自服务器的响应是 XML，而且需要作为 XML 对象进行解析，请使用 responseXML 属性：

请求 [books.xml](https://www.w3school.com.cn/example/xmle/books.xml) 文件，并解析响应：

```js
xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i=0;i<x.length;i++){
  txt=txt + x[i].childNodes[0].nodeValue + "<br />";
}
document.getElementById("myDiv").innerHTML=txt;
```

## 服务器端响应的数据格式

在真实的项目中，服务器端大多数情况下会以 JSON 对象作为响应数据的格式。

当客户端拿到响应数据时，要将 JSON 数据和 HTML 字符串进行拼接，然后将拼接的结果展示在页面中。

在 http 请求与响应的过程中，无论是请求参数还是响应内容，如果是对象类型，最终都会被转换为对象字符串进行传输。

```html
<script>
    // 1.创建ajax对象
    var xhr = new XMLHttpRequest();
    // 2.告诉Ajax对象要向哪发送地址，以什么样的方式发送请求
    // (1)请求方式 (2)请求地址
    xhr.open('get', 'http://localhost:3000/responseData');
    // 3.发送请求
    xhr.send();
    // 4.获取服务器端响应到客户端的数据
    xhr.onload = function() {
        console.log(xhr.responseText);
        console.log(typeof xhr.responseText)
    }
</script>
```

在 app.js 中设置路由

```js
// 引入express框架
const express = require('express');
// 路径处理模块
const path = require('path');
// 创建web服务器
const app = express();
// 静态资源访问服务功能
app.use(express.static(path.join(__dirname, 'public')));

// 创建路由
app.get('/responseData', (req, res) => {
    // 响应一个 json 对象
    res.send({ "name": "zhangsan" });
});

// 监听端口
app.listen(3000, () => {
    console.log('服务器启动成功');
});

```


我们需要将 json 字符串转换为 json 对象

```html
<script>
    // 1.创建ajax对象
    var xhr = new XMLHttpRequest();
    // 2.告诉Ajax对象要向哪发送地址，以什么样的方式发送请求
    // (1)请求方式 (2)请求地址
    xhr.open('get', 'http://localhost:3000/responseData');
    // 3.发送请求
    xhr.send();
    // 4.获取服务器端响应到客户端的数据
    xhr.onload = function() {
        // 将JSON字符串转换为JSON对象
        var responseText = JSON.parse(xhr.responseText);
        // 测试:在控制台输出处理结果
        console.log(responseText)
        // 将数据和html字符串进行拼接
        var str = '<h2>' + responseText.name + '</h2>';
        // 将拼接的结果追加到页面中
        document.body.innerHTML = str;
    }
</script>
```

# AJAX - onreadystatechange 事件

## onreadystatechange 事件

当请求被发送到服务器时，我们需要执行一些基于响应的任务。

每当readyState改变时，就会触发onreadystatechange事件。

readyState 属性存有 XMLHttpRequest 的状态信息。

下面是 XMLHttpRequest 对象的三个重要的属性：

+ onreadystatechange 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。
+ readyState 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。
  - 0: 请求未初始化
  - 1: 服务器连接已建立
  - 2: 请求已接收
  - 3: 请求处理中
  - 4: 请求已完成，且响应已就绪
+ status 200: "OK" ；404: 未找到页面

在 onreadystatechange 事件中，我们规定当服务器响应已做好被处理的准备时所执行的任务。

当 readyState 等于 4 且状态为 200 时，表示响应已就绪：

```js
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
  }
```

## 使用 Callback 函数

callback 函数是一种以参数形式传递给另一个函数的函数。

如果您的网站上存在多个 AJAX 任务，那么您应该为创建 XMLHttpRequest 对象编写一个标准的函数，并为每个 AJAX 任务调用该函数。

该函数调用应该包含 URL 以及发生 onreadystatechange 事件时执行的任务（每次调用可能不尽相同）：

```js
function myFunction(){
	loadXMLDoc("ajax_info.txt",function() {
  		if (xmlhttp.readyState==4 && xmlhttp.status==200) {
    		document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    	}
  });
}
```

# Ajax封装

问题: 发送一次请求代码过多，发送多次请求代码冗余且重复

解决方案：将请求代码封装到函数中，发请求时调用函数即可

Ajax 封装代码如下：

```js
function ajax (options) {
	// 默认值
	var defaults = {
		type: 'get',
		url: '',
		async: true,
		data: {},
		header: {
			'Content-Type': 'application/x-www-form-urlencoded'
		},
		success: function () {},
		error: function () {}
	}
	// 使用用户传递的参数替换默认值参数
	Object.assign(defaults, options);
	// 创建ajax对象
	var xhr = new XMLHttpRequest();
	// 参数拼接变量
	var params = '';
	// 循环参数
	for (var attr in defaults.data) {
		// 参数拼接
		params += attr + '=' + defaults.data[attr] + '&';
		// 去掉参数中最后一个&
		params = params.substr(0, params.length-1)
	}
	// 如果请求方式为get
	if (defaults.type == 'get') {
		// 将参数拼接在url地址的后面
		defaults.url += '?' + params;
	}

	// 配置ajax请求
	xhr.open(defaults.type, defaults.url, defaults.async);
	// 如果请求方式为post
	if (defaults.type == 'post') {
		// 设置请求头
		xhr.setRequestHeader('Content-Type', defaults.header['Content-Type']);
		// 如果想服务器端传递的参数类型为json
		if (defaults.header['Content-Type'] == 'application/json') {
			// 将json对象转换为json字符串
			xhr.send(JSON.stringify(defaults.data))
		}else {
			// 发送请求
			xhr.send(params);
		}
	} else {
		xhr.send();
	}
	// 请求加载完成
	xhr.onload = function () {
		// 获取服务器端返回数据的类型
		var contentType = xhr.getResponseHeader('content-type');
		// 获取服务器端返回的响应数据
		var responseText = xhr.responseText;
		// 如果服务器端返回的数据是json数据类型
		if (contentType.includes('application/json')) {
			// 将json字符串转换为json对象
			responseText = JSON.parse(responseText);
		}
		// 如果请求成功
		if (xhr.status == 200) {
			// 调用成功回调函数, 并且将服务器端返回的结果传递给成功回调函数
			defaults.success(responseText, xhr);
		} else {
			// 调用失败回调函数并且将xhr对象传递给回调函数
			defaults.error(responseText, xhr);
		} 
	}
	// 当网络中断时
	xhr.onerror = function () {
		// 调用失败回调函数并且将xhr对象传递给回调函数
		defaults.error(xhr);
	}
}
```

# 同源政策

1.1、Ajax请求限制
Ajax 只能向自己的服务器发送请求。比如现在有一个A网站、有一个B网站，A网站中的 HTML 文件只能向A网站服务器中发送 Ajax 请求，B网站中的 HTML 文件只能向 B 网站中发送 Ajax 请求，但是 A 网站是不能向 B 网站发送 Ajax请求的，同理，B 网站也不能向 A 网站发送 Ajax请求。

1.2、什么是同源
如果两个页面拥有相同的协议、域名和端口，那么这两个页面就属于同一个源，其中只要有一个不相同，就是不同源

1.3、同源政策的目的
同源政策是为了保证用户信息的安全，防止恶意的网站窃取数据。最初的同源政策是指 A 网站在客户端设置的 Cookie，B网站是不能访问的。

随着互联网的发展，同源政策也越来越严格，在不同源的情况下，其中有一项规定就是无法向非同源地址发送Ajax 请求，如果请求，浏览器就会报错。

1.4、使用JSONP解决同源限制问题

jsonp 是 json with padding 的缩写，它不属于 Ajax 请求，但它可以模拟 Ajax 请求

1. 将不同源的服务器端请求地址写在 script 标签的 src 属性中。

   ```html
   <script src="www.example.com"></script>
   ```

   ```html
   <script src=“https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>

2. 服务器端响应数据必须是一个函数的调用，真正要发送给客户端的数据需要作为函数调用的参数。

   ```js
   const data = 'fn({name: "张三", age: "20"})';
   res.send(data);
   ```

3. 在客户端全局作用域下定义函数fn，在fn函数内部对服务端返回的数据进行处理

   注意要将函数定义放在 script 标签的前面，因为 script 标签加载完服务器端的响应内容以后会直接调用这个准备好的函数，如果客户端没有定义这个函数，函数在调用时找不到这个函数的定义部分，代码将会报错。

   ```js
   function fn(data) {
       console.log(data);
   }
   ```

   示例：

   我们开启两个服务器，一个端口是3000，另一个端口是3001，我们在端口为3000的客户端发送ajax请求至端口为3001的服务器：

   ```html
   <body>
   	<script>
   		function fn (data) {
               // 在客户端定义函数
   			console.log('客户端的fn函数被调用了')
   			console.log(data);
   		}
   	</script>
   	<!-- 1.将非同源服务器端的请求地址写在script标签的src属性中 -->
   	<script src="http://localhost:3001/test"></script>
   </body>
   ```

   在服务器端调用函数：

   ```js
   // 引入express框架
   const express = require('express');
   // 路径处理模块
   const path = require('path');
   // 创建web服务器
   const app = express();
   // 静态资源访问服务功能
   app.use(express.static(path.join(__dirname, 'public')));
   // 创建路由
   app.get('/test', (req, res) => {
       // 在服务器调用函数
       const result = 'fn({name: "张三"})';
       res.send(result);
   });
   // 监听端口
   app.listen(3001);
   ```

4. JSONP代码优化

   1. 客户端需要将函数名称传递到服务器端
   2. 将 script 请求的发送变成动态请求。

   ```html
   <body>
   	<script>
   		function fn (data) {
   			console.log('客户端的fn函数被调用了')
   			console.log(data);
   		}
   	</script>
   	<!-- 1.将非同源服务器端的请求地址写在script标签的src属性中 -->
   	<script src="http://localhost:3001/better?callback=fn"></script>
   </body>
   ```

   在非同源服务器端进行接收客户端传递的函数名称

   ```js
   // 引入express框架
   const express = require('express');
   // 路径处理模块
   const path = require('path');
   // 创建web服务器
   const app = express();
   // 静态资源访问服务功能
   app.use(express.static(path.join(__dirname, 'public')));
   // 创建路由
   app.get('/better', (req, res) => {
       // 接收客户端传递过来的函数的名称
       const fnName = req.query.callback;
       // 将函数名称对应的函数调用代码返回给客户端
       const result = fnName + '({"name": "张三"})';
       res.send(result);
   });
   // 监听端口
   app.listen(3001);
   ```

5. 封装 jsonp 函数，方便请求发送

   ```js
   function jsonp (options) {
   	// 动态创建script标签
   	var script = document.createElement('script');
   	// 拼接字符串的变量
   	var params = '';
   
   	for (var attr in options.data) {
   		params += '&' + attr + '=' + options.data[attr];
   	}
   	
   	// myJsonp0124741
   	var fnName = 'myJsonp' + Math.random().toString().replace('.', '');
   	// 它已经不是一个全局函数了
   	// 我们要想办法将它变成全局函数
   	window[fnName] = options.success;
   	// 为script标签添加src属性
   	script.src = options.url + '?callback=' + fnName + params;
   	// 将script标签追加到页面中
   	document.body.appendChild(script);
   	// 为script标签添加onload事件
   	script.onload = function () {
   		document.body.removeChild(script);
   	}
   }
   ```

   我们也可以说是封装了 JSONP 函数，我们可以将 JSONP 抽离为 jsonp.js文件，这样我们在客户端就可以引入 jsonp.js 文件使用了

6. 服务器端代码优化之 res.jsonp 方法

   服务器端接收客户端传递过来的函数名称，并且拼接函数调用，在函数调用的内部，我们还需要将真实的数据写在里面，如果数据是从数据库查出来的 json 对象，我们还需要先转换成 json 字符串，比较麻烦，express 框架给我们 res 下提供了 res.jsonp 方法

   ```js
   app.get('/better',(req,res)=>{
       res.jsonp({name:"lisi",age:20})
   })
   ```

1.5、CORS跨域资源共享
CORS：全称为 Cross-origin resource sharing，即跨域资源共享，它允许浏览器向跨域服务器发送 Ajax 请求，克服了 Ajax 只能同源使用的限制。

CORS 怎么工作的？ 

CORS 是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行。 

CORS 的使用 

主要是服务器端的设置： 

```js
router.get("/testAJAX" , function (req , res) {

    //通过 res 来设置响应头，来允许跨域请求 

    //res.set("Access-Control-Allow-Origin","http://127.0.0.1:3000"); 

    res.set("Access-Control-Allow-Origin","*"); 

    res.send("testAJAX 返回的响应"); 

}); 
```

# jQuery中的Ajax


$.ajax()方法作用：发送 Ajax 请求

发送Ajax请求

```js
$.ajax({
     type: 'get',
     url: 'http://www.example.com',
     data: { name: 'zhangsan', age: '20' },
     contentType: 'application/x-www-form-urlencoded',
     beforeSend: function () { 
         return false
     },
     success: function (response) {},
     error: function (xhr) {}
});

```

+ type：代表请求方式

+ url：代表请求地址

+ data：代表向服务器端发送的请求参数，它可以是一个对象，在内部会将其转化为参数字符串，除了传递对象以外，我们也可以传递字符串参数值,在内部都会将其转化为参数字符串进行发送

```js
{
    data: 'name=zhangsan&age=20'
}
```

+ contentType：告诉服务器端客户端要向服务器端传递的参数格式类型，默认是application/x-www-form-urlencoded,也就是 参数名 = 参数值，多个参数之间用 & 分隔的参数字符串，如果传递的是 json 格式的请求参数,需要将 contentType 进行如下替换

```js
{
     contentType: 'application/json'
}
```

​		然后在 data 中传递 json 格式字符串,需要通过 JSON.stringify 将json对象转换为字符串		

```js
JSON.stringfy({ name: 'zhangsan', age: '20' })
```

+ beforeSend：允许我们在请求发送之前做一些事，是一个函数，比如在请求发送之前我们可以对请求参数进行格式验证，格式不正确，return false 请求便不会发送了

+ success：是一个函数，请求发送成功之后就会被调用，有一个形参，这个形参就是服务器端返回的数据

+ error：是一个函数，请求失败之后就会被调用，接收一个 ajax 对象，我们可以在对象中获取错误信息，并且根据错误信息做出错误处理

例如，点击按钮发送 ajax 请求,客户端代码如下：

```html
<body>
	<button id="btn">发送请求</button>
	<script src="/js/jquery.min.js"></script>
	<script>
		$('#btn').on('click', function () {
			$.ajax({
				// 请求方式
				type: 'get',
				// 请求地址
				url: 'http://localhost:3000/base',
				// 请求成功以后函数被调用
				success: function (response) {
					// response为服务器端返回的数据
					// 方法内部会自动将json字符串转换为json对象
					console.log(response);
				},
				// 请求失败以后函数被调用
				error: function (xhr) {
					console.log(xhr)
				}
			})
		});
	</script>
</body>
```


服务器端代码如下：

```js
// 引入express框架
const express = require('express');
// 路径处理模块
const path = require('path');
// 创建web服务器
const app = express();
// 静态资源访问服务功能
app.use(express.static(path.join(__dirname, 'public')));
app.get('/base', (req, res) => {
    res.send({
        name: 'zhangsan',
        age: 30
    })
});

// 监听端口
app.listen(3000);
```

