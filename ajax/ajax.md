## AJAX概述

####  AJAX简介

AJAX全称为Asynchronous JavaScript And XML,就是异步的JS和XML
通过AJAX可以在浏览器中向服务器发送异步请求，最大的优势：**无刷新获取数据**

#### AJAX特点

 优点 

- 可以无需刷新页面而与服务器端进行通信
-  允许你根据用户事件来更新部分页面内容

 缺点 

- 没有浏览历史，不能回退 
- 存在跨域问题（同源） 
- SEO 不友好 

##  原生AJAX 的基本使用 XHR 

```
// 1. 创建Ajax对象
        var xhr = new XMLHttpRequest();
        
//2.设置请求信息(请求方式和url)
 // 请求方式
        xhr.open(method, url);

//可以设置请求头，一般不设置
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        
//3.发送请求
        xhr.send(body) 
        //get请求不传 body 参数，只有post请求使用

//4.处理响应
       xhr.onreadystatechange = function(){
  // 判断 (服务端返回了所有的结果)
           if(xhr.readyState === 4){
           if(xhr.status >= 200 && xhr.status < 300){
           var res = JSON.parse( xhr.responseText );
           //返回数据
    } else {

    }
  }
}
```

#### AJAX请求状态

xhr.readyState 可以查看请求当前的状态

- 0:表示XMLHttpRequest实例已经生成，但是open( )方法还没有被调用. 
- 1:表示send( )方法还没有被调用，仍然可以使用setRequestHeader(,设定HTTP请求的头信息.
- 2:表示send( )方法已经执行，并且头信息和状态码已经收到.
- 3:表示正在接收服务器传来的body部分的数据.
- 4:表示服务器数据已经完全接收，或者本次接收已经失败了.

#### GET请求

​      客户端html

```
      btn.onclick = function(){
      // 1. 创建对象 
      const xhr = new XMLHttpRequest();
      // 2. 初始化 设置请求方法和url
      xhr.open('GET', 'http://127.0.0.1:8000')
      // 3. 发送
      xhr.send();
      // 4. 事件绑定 处理服务端返回的结果
      xhr.onreadystatechange = function(){
        // readyState 是 xhr 对象中的属性, 表示状态 0 1 2 3 4
        //判断 (服务端返回了所有的结果)
        if(xhr.readyState === 4){
          //判断响应状态码 200  404  403 401 500
          if(xhr.status >= 200 && xhr.status < 300){
            // 处理结果 行 头 空行 体
            // 响应
            console.log('状态码', xhr.status); // 状态码
            console.log('状态字符串', xhr.statusText); // 状态字符串
            console.log('所有响应头', xhr.getAllResponseHeaders()); // 所有响应头
            console.log('响应体', xhr.response); // 响应体
            
            //设置 result 的文本
            result.innerHTML=xhr.response;
          }else{
          }
        }
      } 
    }
  
```

服务器端app.js

```
// 1. 引入express
const express = require('express');

// 2. 创建应用对象
const app = express();

// 3. 创建路由规则
app.get('/server', (request, response) => {
  // 设置响应头 设置允许跨域
  response.setHeader('Access-Control-Allow-Origin', '*');
  // 设置响应体
  response.send("Hello Ajax");
});

// 4. 监听服务
app.listen(8000, () => {
  console.log("服务已经启动, 8000 端口监听中...");
 })
```

get设置请求参数

```
xhr.open('GET', 'http://127.0.0.1:8000/server?a=100&b=200&c=300');
```



#### POST请求

```
  // 获取下拉框选中项的值
            var posValue = pos.value;

            // 获取文本框输入的值
            var keywordVal = keyword.value;

            // 准备传递参数数据
            var obj = {
                keyword: keywordVal,
                num: lvshi,
                type: posValue,
                token: "lJLxHK2NrnPVri457AaK"
            };

            // 参数字符串变量
            var paramStr = "";
            // for...in遍历obj对象
            for( var attr in  obj ){
                paramStr += "&" + attr + "=" + obj[attr];
            }
            // 去掉第一个&符号
            paramStr = paramStr.substr(1);                                                                 
  var xhr = new XMLHttpRequest();
            xhr.open("POST", "https://v2.alapi.cn/api/ai/poem");
            // post传统表单传值方式传参需要先设置请求头  告诉服务器端当前请求参数的格式是为传统表单传值方式
            xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
            // 发送请求
            xhr.send(paramStr);
            xhr.onreadystatechange = function(){
                if( xhr.readyState === 4 && xhr.status === 200 ){
                    var res = JSON.parse( xhr.responseText );
                   
                }
            }
```

请求超时和网络异常

```
const btn = document.getElementsByTagName('button')[0];
const result = document.querySelector('#result');

btn.addEventListener('click', function(){
  const xhr = new XMLHttpRequest();

  //超时设置 2s 设置🔴
  xhr.timeout = 2000;
  //超时回调
  xhr.ontimeout = function(){
    alert("网络异常, 请稍后重试!!");
  }
  //网络异常回调
  xhr.onerror = function(){
    alert("你的网络似乎出了一些问题!");
  }

  xhr.open("GET",'http://127.0.0.1:8080/delay');
  xhr.send();
  xhr.onreadystatechange = function(){
    if(xhr.readyState === 4){
      if(xhr.status >= 200 && xhr.status< 300){
        result.innerHTML = xhr.response;
      }
    }
  }
})
```

服务器端 app.js

```
//延时响应
app.all('/delay', (request, response) => {
    //设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    setTimeout(() => {
        //设置响应体
        response.send('延时响应');
    }, 3000)
});
```

#### API总结

- XMLHttpRequest( ) 创建XHR对象的构造函数
- status 响应状态码值，如200、404
- statusText 响应状态文本，如'ok、'not found
- readyState 标识请求状态的只读属性0-1-2-3-4
- response 响应体数据，类型取决于responseType的指淀
- timeout 指定请求超时时间，默认为0代表没有限制
- ontimeout 绑定超时的监听
- onerror 绑定请求网络错误的监听
- open(请求方式，url)初始化一个请求，参数为：
  (method,url[,async])
- setRequestHeader(name,value)设置请求头
- send(data)发送请求
- abort( )中断请求（发出到返回之间）
- onreadystatechange绑定readyState改变的监听
- responseType指定响应数据类型，如果是json',得到响应后自动解析响应
- getResponseHeader(name)获取指定名称的响应头值
- getAllResponseHeaders()获取所有响应头组成的字符串

## Jquery中的AJAX

#### GET请求

> $.get(url, [data], [callback], [type])

```
 $.get('http://127.0.0.1:8080/jquery-server', {a:100, b:200}, function(data){
      console.log(data);
    },'json');
```

#### POST请求

> $.post(url, [data], [callback], [type])

```
 $.post('http://127.0.0.1:8080/jquery-server', {a:100, b:200}, function(data){
      console.log(data);
    });
```

#### 通用方法$.ajax( )

```
$.ajax({
  url: 'http://127.0.0.1:8080/', // url
  data: {a:100, b:200}, // 参数 
  type: 'GET', // 请求类型
  dataType: 'json', // 响应体结果
  success: function(data){console.log(data);}, // 成功的回调
  timeout: 2000, // 超时时间
  error: function(){console.log('出错拉~');}, // 失败的回调
  headers: { // 头信息
    c: 300,
    d: 400
  }	
})
```



## 跨域

#### 同源策略

同源策略(Same-Origin Policy)是浏览器的一种安全策略

- 同源：协议、域名、端口号必须完全相同
- 跨域：违背同源策略就是跨域

#### 如何解决跨域问题

##### 1.jsonp

-   jsonp 是 json with padding 的缩写，它不属于 Ajax 请求，但它可以模拟 Ajax 请求。  JSONP只能模拟get请求
- JSONP(JSON with Padding)是JSON的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题。  解决跨域问题的一种技术一种方案
- JSONP的原理:
  - img中的src,script中的src以及其他HTML标签src属性引入跨域的资源是不受限制,可以直接拿到跨域网址对应的内容/

###### 简单例子

```
<script>
    function fn(data) {
        // 这里data参数就是响应的数据
        console.log("data=>", data);
    }
</script>
<script src="http://localhost:8080/check-username?callback=fn"></script>
```

服务器端app.js

```
//1. 引入express
const express = require('express');

//2. 创建应用对象
const app = express();

//用户名检测是否存在
app.all('/check-username',(request, response) => {
  const data = {
    exist: 1,
    msg: '用户名已经存在'
  };
  //将数据转化为字符串
  let str = JSON.stringify(data);
  //返回结果
  response.end(`fn(${str})`);
  //fn是callback的具体值.注意一下
});

//4. 监听端口启动服务
app.listen(8000, () => {
  console.log("服务已经启动, 8000 端口监听中....");
});
```



###### 原生js封装jsonp

```
 function jsonp( options ){
            // 参数字符串
            var paramStr = "";

            // 遍历options.data
            for(var attr in options.data ){
                // 拼接字符串
                paramStr += "&" + attr + "=" + options.data[attr];
            }

            // 创建script标签
            var newScript = document.createElement("script");

            // 随机函数名
            var funName = "myJsonp" + Math.random().toString(16).substr(2);

            // 在全局作用域下声明一个函数
            window[funName] = options.success;

            // 设置src属性
            newScript.src = options.url + "?callback=" + funName + paramStr;

            // 把新的script添加到body标签中
            document.body.appendChild( newScript );

            // 新的script标签加载完毕
            newScript.onload = function(){
                // 删除
                this.remove();
            }
        }
        
        //调用
          jsonp( {
                url: "https://api.asilu.com/phone",
                data: {
                    phone : telPhoneVal
                },
                success:function( data ){
                    console.log( data );
                }
            } );
```

###### Jquery使用jsonp

```
 $.ajax({
                type:"get",
                url:"http://localhost:8080/jquery-jsonp-server",
                data: {
                   
                },
                // 指定当前发送的请求为jsonp请求
                dataType:"jsonp",
                // 可选，向服务器端传递函数名字的参数名称
                jsonp: 'cb',
                // 可选，指定函数名称，若不想用 success 函数时指定，需要提前在全局定义好该函数
                 jsonCallback: 'fnName',
                success:function( res ){
                    
                },
                error:function( xhr ){
                  
                }
            })
```

服务器 app.js

```
app.all('/jquery-jsonp-server',(request, response) => {
    // response.send('console.log("hello jsonp")');
    const data = {
        name:'尚硅谷',
        city: ['北京','上海','深圳']
    };
    //将数据转化为字符串
    let str = JSON.stringify(data);
    //接收 callback 参数
    let cb = request.query.callback;

    //返回结果
    response.end(`${cb}(${str})`);
});
```

##### 2.CORS

https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS

- CORS(Cross-Origin Resource Sharing,跨域资源共享)。CORS是官方的跨域解决方案，它的特点是不需要在客户端做任何特殊的操作，**完全在服务器中进行处理**，支持get和post请求。跨域资源共享标准新增了一组HTTP首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源.
- COS是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行。
- Access-Control-Allow-Origin:<origin>| *    origin参数的值指定了允许访问该资源的外域URI

###### CORS的使用

服务器app.js设置

```
app.all('/cors-server', (request, response)=>{
    //设置响应头 *表示所有网页都可以，限定就填限定的IP
    response.setHeader("Access-Control-Allow-Origin", "*");		// 允许哪些IP可以访问
    response.setHeader("Access-Control-Allow-Headers", '*');	// 允许哪些自定义请求头
    response.setHeader("Access-Control-Allow-Method", '*');		// 允许哪些请求方式
    // response.setHeader("Access-Control-Allow-Origin", "http://127.0.0.1:5500");
    response.send('hello CORS');
});
```

