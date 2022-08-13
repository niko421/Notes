## 1.使用express搭建服务器

```
//导入express
const express=require('express');

//创建web服务器
const app=express();

//监听客户端的get和post请求 并向客户端响应相应内容
app.get('/user', (req, res) => {
    //调用express提供的res.send(),向客户端响应一个json对象
    res.send({name:"张三",age:"20",gender:"男"});
})

app.post('',(req,res)=> {
    //调用express提供的res.send() 向客户端响应一个文本对象
    res.send('请求成功')
})

app.get('/', function(req, res) {
    //http://localhost:8080/?name='张三'
    //通过req.query可以获取客户端发送过来的查询参数
    console.log('req.query'+req.query);
    res.send(req.query);//{ name: "'张三" }
})

app.get('/user/:id/:username', function(req, res){
    //http://localhost:8080/user/2/张三
    //req.params是动态匹配到的URL参数 默认是空对象
    console.log(req.params);
    res.send(req.params);//{ id: '2', username: '张三' }
})

//启动服务器
app.listen('8080',function(){
    console.log('express listening on port 8080');
})
```

## 2.使用express.static对外提供静态资源访问

```
const express=require("express");
const path=require("path");
const app= express();

// app.use(express.static(path.join(__dirname,'./clock')))
//如果托管多个静态资源 多次调用该方法即可

app.use(express.static('./clock'))
//注意clock不会出现在url地址中 直接访问clock下的任意文件即可

app.use('/abc',express.static('./files'))
//此时访问的不是clock下的index 而是files下的index 因为加了abc前缀

app.listen('8080',()=>{
    console.log("express server running at 8080");

})
```

## 3.最简单的路由用法

```
//导入express

const express=require('express');

//创建web服务器

const app=express();

//挂载路由

app.get('/', function(req, res) {

  res.send('hello world!');

})

app.post('/', function(req, res) {

  res.send('hello world!');

})

//启动服务器

app.listen('8080',function(){

  console.log('express listening on port 8080');

})
```

