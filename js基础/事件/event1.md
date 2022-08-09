## 什么是事件

- 一个事件由什么东西组成

  触发谁的事件：事件源

  触发什么事件：事件类型

  触发以后做什么：事件处理函数

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
复制代码
* 当你点击的时候，只会执行第二个，第一个就没有了
复制代码
```

- 我们还有一种事件监听的方式去给元素绑定事件
- 使用 `addEventListener` 的方式添加
- 这个方法不兼容，在 IE 里面要使用 `attachEvent`

## 事件监听

#### addEventListener :  非 IE 7 8 下使用

- 语法： `元素.addEventListener('事件类型'， 事件处理函数， 冒泡还是捕获)`

```javascript
oDiv.addEventListener('click', function () {
 console.log('我是第一个事件')
}, false)

oDiv.addEventListener('click', function () {
 console.log('我是第二个事件')
}, false)
复制代码
 * 当你点击 div 的时候，两个函数都会执行，并且会按照你注册的顺序执行
复制代码
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

## 两个方式的区别

注册事件的时候事件类型参数的书写 

- `addEventListener` ： 不用写 on
- `attachEvent` ： 要写 on

参数个数 

- `addEventListener` ： 一般是三个常用参数
- `attachEvent` ： 两个参数

执行顺序 

- `addEventListener` ： 顺序注册，顺序执行
- `attachEvent`： 顺序注册，倒叙执行

适用浏览器 

- `addEventListener` ： 非 IE 7 8 的浏览器
- `attachEvent` ： IE 7 8 浏览器


