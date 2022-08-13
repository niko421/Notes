## fs文件系统模块

### readFile读取文件内容



> fs.readFile()
>
> 参数1 读取文件的存放路径
>
> 参数2 读取文件的编码格式 默认为utf-8
>
> 参数3 回调函数 拿到失败和成功的结果 err dataStr

例子

```
//导入fs模块 来操作文件

const fs=require('fs');

fs.readFile('./files/1.txt','utf8',function(err,dataStr){

  //打印失败结果

  //如果成功 err为null 如果读取成功 err值为错误对象 dataStr的值为undefined

  console.log(err);

  //打印成功结果

  console.log(dataStr);

})

```

![](C:\Users\86189\Desktop\笔记\node\imgs\readfiles.png)

### 判断文件是否读取成功

```
const fs = require('fs')
fs.readFile('./files/1.txt', 'utf8', function (err, dataStr) {
    if (err) {
        return console.log("读取文件失败" + err.message)
        //如果err为空 说明文件读取成功 不为null 则存在即能转为true则读取失败

    }
    console.log("文件读取成功" + dataStr)
})
```

### writeFile写入文件内容

> fs.writeFile( )
>
> 参数1 文件的存放路径
>
> 参数2 写入的内容
>
> 参数3 回调函数
>
> 重复调用writeFile会覆盖上一次的内容

```
const fs=require('fs');
fs.writeFile('./files/2.txt','abcd',function(err,dataStr){
console.log(err);
})
```

### readdir( )

使用 fs.readdir() 方法，可以读取指定目录下所有文件的名称，语法格式如下：

```css
 fs.readdir(path[, options], callback)
```

- 参数1：必选参数，表示要读取哪个目录下的文件名称列表。
- 参数2：可选参数，以什么格式读取目录下的文件名称，默认值是utf8。
- 参数3：必选参数，读取完成以后的回调函数。

例子

```
 const fs = require('fs');
 fs.readdir('./', (err, data) => {
     // 错误处理
     if (err) return console.log(err);
     console.log(data);
 });

```



### fs 模块-路径动态拼接的问题

在使用 fs 模块操作文件时，如果提供的操作路径是以./ 或 ../ 开头的相对路径时，很容易出现路径动态拼接错误的问题。 这是因为代码在运行的时候，会以执行node 命令时所处的目录，动态拼接出被操作文件的完整路径。

解决方案：在使用fs 模块操作文件时，直接提供绝对路径，不要提供./ 或 ../ 开头的相对路径，从而防止路径动态拼接的问题。

注意：使用__dirname 获取当前文件所在的**绝对路径**

 

```
const fs = require('fs');
 // 拼接要读取文件的绝对路径
 let filepath = __dirname +'/hello.txt'
 fs.readFile(filepath, 'utf-8', (err, data) => {
     // 判断是否读取成功
     if (err) return console.log(err);
     console.log(data); 
 });
```

## path路径模块

### 路径拼接path.join()

 使用 path.join() 方法，可以把多个路径片段拼接为完整的路径字符串，语法格式如下： 

```
 path.join([...paths])
```

 使用 path.join() 方法，可以把多个路径片段拼接为完整的路径字符串： 

```
 const path = require('path');
 console.log( path.join('a', 'b', 'c') ); // a/b/c
 console.log( path.join('a', '/b/', 'c') ); // a/b/c
 console.log( path.join('a', '/b/', 'c', 'index.html') ); // a/b/c/index.html
 console.log( path.join('a', 'b', '../c', 'index.html') ); // a/c/index.html
 console.log(__dirname); // node自带的全局变量，表示当前js文件所在的绝对路径
 // 拼接成绩.txt的绝对路径
 console.log( path.join(__dirname, '成绩.txt') ); // ------ 最常用的
```

### 获取路径中的文件名**path.basename**( )

##### 

使用 path.basename() 方法，可以获取路径中的最后一部分，经常通过这个方法获取路径中的文件名，语法格式如下：

```lua
 path.basename(path[,ext])
```

- path  必选参数，表示一个路径的字符串
- ext  可选参数，表示可选的文件扩展名
- 返回:  表示路径中的最后一部分

```
// 找文件名
 console.log( path.basename('index.html') ); // index.html
 console.log( path.basename('a/b/c/index.html') ); // index.html
 console.log( path.basename('a/b/c/index.html?id=3') ); // index.html?id=3
 console.log(path.basename('/api/getbooks')) // getbooks
```

### 获取路径中的文件扩展名path.extname( )

使用 path.extname() 方法，可以获取路径中的扩展名部分，语法格式如下：

```lua
 path.extname(path)
```

- path 必选参数，表示一个路径的字符串
- 返回:  返回得到的扩展名字符串

使用 path.extname() 方法，可以获取路径中的扩展名部分

```arduino
 // 找字符串中，最后一个点及之后的字符
 console.log( path.extname('index.html') ); // .html
 console.log( path.extname('a.b.c.d.html') ); // .html
 console.log( path.extname('asdfas/asdfa/a.b.c.d.html') ); // .html
 console.log( path.extname('adf.adsf') ); // .adsf
```



## http模块

### 创建服务器

```
//导入http模块
const http=require('http');
//创建web服务器实例
const server=http.createServer()
//为服务器实例绑定request事件 监听客户端请求 调用回调函数处理事件
server.on('request',function(req,res){
    console.log("someone visit server");
})
//启动服务器
server.listen(8080,function(){
    console.log("server running at localhost:8080");
})

//按住ctrl C 停止服务
```

### 了解request请求对象

```
//导入http模块
const http=require('http');
//创建web服务器实例
const server=http.createServer()
//为服务器实例绑定request事件 监听客户端请求 调用回调函数处理事件

server.on('request',(req,res)=>{
    console.log("someone visit server");
    //url是客户端请求的URL地址 端口号后面的地址 可以手动加 没有这个界面也可以
    const url=req.url;
    //method是客户端请求的method类型
    const method=req.method;
    let str=`Your request url: ${url} and method: ${method}`
    //解决中文乱码
     res.setHeader('Content-Type','text/html;charset=utf-8');
    //调用res.end()方法 向客户端响应一些内容
    res.end(str)

})

//启动服务器
server.listen(8080,()=>{
    console.log("server running at localhost:8080");
})
```

### 根据不同的url响应不同的内容

这里需要注意的是url是客户端手动输入的地址,服务器可以没有该文件,如果客户端输入的地址在服务器没有定义就会报错

```
const http = require('http');
const server = http.createServer();

server.on('request', (req, res) => {
    //1.获取请求的url地址
    const url = req.url;
    //2.设置默认的响应内容为404 not found
    let content = '404 not found';
    //3.判断用户的请求是否为/index.html
    //4.判断用户是否为about.html
    if (url === '/' || url === '/index.html') {
         content = `<h1>首页</h1>`
    } else if (url === '/about.html') {
         content = `<h1>关于页面</h1>`
    }
    //5.设置响应头 防止中文乱码
    res.setHeader('Content-Type', 'text/html; charset=utf-8');

    //6.使用res.end()把内容响应给客户端
    res.end(content);
})

server.listen('8080', () => {
    console.log("server running at http://localhost:8080");
});
```

### 将本地文件映射到web服务器访问

```
const http = require('http');
const path = require('path');
const fs = require('fs');

const server = http.createServer();
server.on('request', (req, res) => {
    //1.获取请求的url地址
    const url = req.url;
    // //2.把请求的url地址映射为文件的存放路径
    // //url就是客户端输入的地址 去拼接文件的绝对路径来访问文件内容

    //需求:输入/也能访问index.html
    //(1)设定空白的文件路径
    let fpath = '';
    if (url === '/') {
        fpath = path.join(__dirname, '/files/index.html');
    } else {
        fpath = path.join(__dirname, '/files', url);
    }
    //3.根据映射过来的文件路径读取文件内容
    fs.readFile(fpath, 'utf8', function (err, dataStr) {
        //如果读取文件失败 向客户端响应固定德错误消息
        if (err) { return res.end('404 not found'); }
        res.end(dataStr);
    })

})

server.listen('8080', () => {
    console.log("server running at http://localhost:8080");
});
```

或许将html代码响应给客户端就能解析成页面了?