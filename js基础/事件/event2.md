## 事件的传播

就像上面那个图片一样，我们点击在红色盒子身上的同时，也是点击在了粉色盒子上

这个是既定事实，那么两个盒子的点击事件都会触发

这个就叫做 **事件的传播**

- **当元素触发一个事件的时候，其父元素也会触发相同的事件，父元素的父元素也会触发相同的事件**
- 就像上面的图片一样
- 点击在红色盒子上的时候，会触发红色盒子的点击事件
- 也是点击在了粉色的盒子上，也会触发粉色盒子的点击事件
- 也是点击在了 body 上，也会触发 body 的点击事件
- 也是点击在了 html 上，也会触发 html 的点击事件
- 也是点击在了 document 上，也会触发 document 的点击事件
- 也是点击在了 window 上，也会触发 window 的点击事件
- 也就是说，页面上任何一个元素触发事件，都会一层一层最终导致 window 的相同事件触发，前提是各层级元素得有注册相同的事件，不然不会触发

在事件传播的过程中，有一些注意的点：

1.只会传播同类事件

2.只会从点击元素开始按照 html 的结构逐层向上元素的事件会被触发

3.内部元素不管有没有该事件，只要上层元素有该事件，那么上层元素的事件就会被触发

到现在，我们已经了解了事件的传播，我们再来思考一个问题

- 事件确实会从自己开始，到 window 的所有相同事件都会触发
- 是因为我们点在自己身上，也确实逐层的点在了直至 window 的每一个元素身上
- 但是到底是先点在自己身上，还是先点在了 window 身上呢
- 先点在自己身上，就是先执行自己的事件处理函数，逐层向上最后执行 window 的事件处理函数
- 反之，则是先执行 window 的事件处理函数，逐层向下最后执行自己身上的事件处理函数



## 事件触发

- 点击子元素的时候，不管子元素有没有点击事件，只要父元素有点击事件，那么就可以触发子元素的点击事件

```html
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
  <script>
    var oUl = docuemnt.querySelector('ul')
    oUl.addEventListener('click', function (e) {
      console.log('我是 ul 的点击事件，我被触发了')
    })
  </script>
</body>
复制代码
```

- 像上面一段代码，当你点击 ul 的时候肯定会触发
- 但是当你点击 li 的时候，其实也会触发

## target

- target 这个属性是事件对象里面的属性，表示你点击的目标
- 当你触发点击事件的时候，你点击在哪个元素上，target 就是哪个元素
- 这个 target 也不兼容，在 IE 下要使用 srcElement

```html
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
  <script>
    var oUl = docuemnt.querySelector('ul')
    oUl.addEventListener('click', function (e) {
      e = e || window.event
      var target = e.target || e.srcElement
      console.log(target)
    })
  </script>
</body>
复制代码
```

- 上面的代码，当你点击 ul 的时候，target 就是 ul
- 当你点击在 li 上面的时候，target 就是 li

## 委托

- 这个时候，当我们点击 li 的时候，也可以触发 ul 的点事件
- 并且在事件内不，我们也可以拿到你点击的到底是 ul 还是 li
- 这个时候，我们就可以把 li 的事件委托给 ul 来做

```html
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
  <script>
    var oUl = docuemnt.querySelector('ul')
    
    oUl.addEventListener('click', function (e) {
      e = e || window.event
      var target = e.target || e.srcElement
     
      // 判断你点击的是 li
      if (target.nodeName.toUpperCase === 'LI') {
      	// 确定点击的是 li
        // 因为当你点击在 ul 上面的时候，nodeName 应该是 'UL'
        // 去做点击 li 的时候该做的事情了
        console.log('我是 li，我被点击了')
      }
    })
  </script>
</body>
复制代码
```

- 上面的代码，我们就可以把 li 要做的事情委托给 ul 来做

