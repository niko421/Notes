## 什么是事件

​	一个事件由什么东西组成

- ​	触发谁的事件：事件源

- ​	触发什么事件：事件类型

- ​	触发以后做什么：事件处理函数

## 点击事件的光标坐标点获取

#### offsetX , offsetY

- #### 是相对于你点击的元素的边框内侧开始计算

```
<style>
  * {
    margin: 0;
    padding: 0;
  }

  div {
    width: 300px;
    height: 300px;
    padding: 20px;
    border: 10px solid #333;
    margin: 20px 0 0 30px;
  }
</style>
<body>
  <div></div>

  <script>
    var oDiv = document.querySelector('div')

    // 注册点击事件
    oDiv.onclick = function (e) {
      // 事件对象兼容写法
      e = e || window.event

      console.log(e.offsetX)
      console.log(e.offsetY)
    }
  </script>
</body>
```

![](C:\Users\86189\Desktop\笔记\js基础\事件\imgs\offsetXY.png)

#### clientX , clientY

- 是相对于浏览器窗口来计算的，不管你页面滚动到什么情况，都是根据窗口来计算坐标

```
<style>
  * {
    margin: 0;
    padding: 0;
  }

  body {
    width: 2000px;
    height: 2000px;
  }

  div {
    width: 300px;
    height: 300px;
    padding: 20px;
    border: 10px solid #333;
    margin: 20px 0 0 30px;
  }
</style>
<body>
  <div></div>

  <script>
    var oDiv = document.querySelector('div')

    // 注册点击事件
    oDiv.onclick = function (e) {
      // 事件对象兼容写法
      e = e || window.event

      console.log(e.clientX)
      console.log(e.clientY)
    }
  </script>
</body>
```

#### pageX , pageY

- 是相对于整个页面的坐标点，不管有没有滚动，都是相对于页面拿到的坐标点

  ```
  <style>
    * {
      margin: 0;
      padding: 0;
    }
  
    body {
      width: 2000px;
      height: 2000px;
    }
  
    div {
      width: 300px;
      height: 300px;
      padding: 20px;
      border: 10px solid #333;
      margin: 20px 0 0 30px;
    }
  </style>
  <body>
    <div></div>
  
    <script>
      var oDiv = document.querySelector('div')
  
      // 注册点击事件
      oDiv.onclick = function (e) {
        // 事件对象兼容写法
        e = e || window.event
  
        console.log(e.pageX)
        console.log(e.pageY)
      }
    </script>
  </body>
  ```

  根据页面左上角来说

  - margin-left 是 30
  - 左边框是 10
  - 左右 padding 各是 20
  - 内容区域是 300
  - **pageX : 300 + 20 + 20 + 10 + 30 = 380**
  - margin-top 是 20
  - 上边框是 10
  - 上下 padding 各是 20
  - 内容区域是 300
  - **pageY : 300 + 20 + 20 + 10 + 20 = 270**

  
  

## 常见的事件

### 事件分类

####   1. 资源事件

- load 页面元素(包括图像多媒体等)资源及其相关资源已完成加载。
- beforunload 用户退出页面

```
例子：
<script>
//通过onload事件等待页面或图像加载完成后调用。
这样可以保证 HTML 文档加载完毕，在函数内部操作dom保证DOM 树是完整的
window.onload = function(){
获取页面上的元素
var row = document.getElementById('row');
var col = document.getElementById('col');
	}
</script>

<body>
<input type="text" id="row"> 
<input type="text" id="col">
</body>
```

#### 2.视图事件

- resize 窗口或者框架被重新调整大小时触发
- scroll 档视图或元素已被滚动。

```
例子：
//给window绑定事件scroll
window.onscroll=function () {
  console.log("窗口发生滚动")

  // 获取页面文档元素的滚动距离
  console.log(document.documentElement.scrollTop)
}
```

#### 3.鼠标事件

- click 当用户点击某个对象时调用的事件句柄
- dblclick 当用户双击某个对象时调用的事件句柄
- mouseover 鼠标移动到某个元素之上
- mouseout 鼠标从某元素离开
- mousedown 鼠标按钮按下
- mouseup 鼠标按键被松开
- mousemove 鼠标在某个元素内移动
- mouseenter 鼠标光标从元素外部移动到元素内部时触发,这个事件不冒泡
- mouseleave 位于元素内部的鼠标光标移动到元素范围之外触发,这个事件不冒泡
- contextmenu 鼠标右键菜单展开时

#### 4.键盘事件

- keydown 某个键盘按键被按下时触发.(不区分大小写)
- keyup 某个键盘按键被松开时触发
- keypress 除 Shift、Fn、CapsLock 外的任意键被按住。而且按住不放(长按),会连续触发。

#####           键盘事件对象

​            键盘事件对象属性 

-  e.keyCode 返回该键的ASCII值
- e.key 返回用户按下的物理按键的键名

#### 5.表单事件

- reset 点击重置按钮时
- submit 点击提交按钮
- focus 元素获得焦点（不会冒泡）。
- blur 元素失去焦点（不会冒泡）。
- input 表单元素的输入事件当value发生变化时
- change 对于表单元素的值被用户提交时。与输入事件不同的是，对元素值的每次更改不一定会触发更该事件。

## 事件流

​    事件流描述的是从页面中接收事件的顺序。
   事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程即DOM事件流
​       DOM事件流分为3个阶段

- 1.捕获阶段
- 2.当前目标阶段
- 3.冒泡阶段

![](C:\Users\86189\Desktop\笔记\js基础\DOM\imgs\eventflow.png)

#### 事件捕获

由DOM最顶层节点开始，然后逐级向下传播到到最具体的元素接收的过程。

#### 事件冒泡

事件开始时由最具体的元素接收，然后逐级向上传播到到DOM最顶层节点的过程。

## 事件的绑定方式

- 我们现在给一个注册事件都是使用 onxxx 的方式
- 但是这个方式不是很好，只能给一个元素注册一个事件
- 一旦写了第二个事件，那么第一个就被覆盖了

```javascript
oDiv.onclick = function () {
 console.log('我是第一个事件')
}

oDiv.onclick = function () {
 console.log('我是第二个事件')
}

当你点击的时候，只会执行第二个，第一个就没有了
```

- 我们还有一种事件监听的方式去给元素绑定事件
- 使用 addEventListener的方式添加
- 这个方法不兼容，在 IE 里面要使用 attachEvent

## 事件监听

#### addEventListener :  非 IE 7 8 下使用

- 语法： 元素.addEventListener('事件类型'， 事件处理函数， 冒泡还是捕获)

```javascript
oDiv.addEventListener('click', function () {
 console.log('我是第一个事件')
}, false)

oDiv.addEventListener('click', function () {
 console.log('我是第二个事件')
}, false)

 * 当你点击 div 的时候，两个函数都会执行，并且会按照你注册的顺序执行

```

- 先打印 我是第一个事件 再打印 我是第二个事件
- 注意： 事件类型的时候不要写 on，点击事件就是 click，不是 onclick

#### attachEvent ：IE 7 8 下使用

- 语法： 元素.attachEvent('事件类型'， 事件处理函数)

```javascript
oDiv.attachEvent('onclick', function () {
 console.log('我是第一个事件')
})

oDiv.attachEvent('onclick', function () {
 console.log('我是第二个事件')
})
复制代码
 * 当你点击 div 的时候，两个函数都会执行，并且会按照你注册的顺序倒叙执行
复制代码
```

- 先打印 我是第二个事件 再打印 我是第一个事件
- 注意： 事件类型的时候要写 on，点击事件就行 onclick

#### 两个方式的区别

注册事件的时候事件类型参数的书写 

- addEventListener： 不用写 on
- attachEvent ： 要写 on

参数个数 

- addEventListener ： 一般是三个常用参数
- attachEvent： 两个参数

执行顺序 

- addEventListener ： 顺序注册，顺序执行
- attachEvent： 顺序注册，倒叙执行

适用浏览器 

- addEventListener： 非 IE 7 8 的浏览器
- attachEvent： IE 7 8 浏览器

## 事件对象

事件发生后，跟事件相关的一系列信息数据的集合都放到这个对象里面，这个对象就是事件对象event,它有很多属性和方法。比如

- 谁绑定了这个事件
- 鼠标触发事件的话，会得到鼠标的相关信息，如鼠标位置
- 键盘触发事件的话，会得到键盘的相关信息，如按了哪个键

```
// event 可简写成 e 或者 evt
eventTarget.onclick = function(event) {} 
eventTarget.addEventListener('click', function(event) {})
```

![](C:\Users\86189\Desktop\笔记\js基础\DOM\imgs\event.png)



####    e.target

​     是事件触发的元素

#### e.preventDefault( )

​    阻止事件默认行为

#### e.stopPropagation( )

​     阻止冒泡

## 事件委托

事件委托也称为事件代理，在jQuery里面称为事件委派。

事件委托的原理
不在每个子节点单独设置事件监听器，
设置在其父节点上，然后利用冒泡原理影响设置每个子节点

```
<ul>
  <li>知否知否，点我应有弹框在手1</li>
  <li>知否知否，点我应有弹框在手2</li>
  <li>知否知否，点我应有弹框在手3</li>
  <li>知否知否，点我应有弹框在手4</li>
  <li>知否知否，点我应有弹框在手5</li>
</ul>

<script>
  // 事件委托的核心原理：给父节点添加侦听器， 利用事件冒泡影响每一个子节点
  var ul = document.querySelector('ul');
  ul.addEventListener('click', function(e) {
    // alert('知否知否，点我应有弹框在手！');
    // e.target 这个可以得到我们点击的对象
    e.target.style.backgroundColor = 'pink';
  })
</script>
```

