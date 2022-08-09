### 一些话

`document.getElementById()` 获取到的结果既是 Node 也是 Element。

Element 一定是 Node，但 Node 不一定是 Element，也可能是文本、空格和换行符。

NodeList 里的换行符是因为原始代码中， HTML 标签与标签、内容与标签之间换行而产生的。

单个的 HTML 标签算是一个单独的 Node。

针对非 HTML标签（比如文本、空格等），从一个 HTML 标签开始，到碰到的第一个 HTML 标签为止，如果中间由内容（文本、空格等），那这部分内容算是一个 Node。

- DOM（Document Object Model）： 文档对象模型
- 其实就是操作 html 中的标签的一些能力
- 我们可以操作哪些内容 
  - 获取一个元素
  - 移除一个元素
  - 创建一个元素
  - 向页面里面添加一个元素
  - 给元素绑定一些事件
  - 获取元素的属性
  - 给元素添加一些 css 样式
  - ...
- DOM 的核心对象就是 docuemnt 对象
- document 对象是浏览器内置的一个对象，里面存储着专门用来操作元素的各种方法
- DOM： 页面中的标签，我们通过 js 获取到以后，就把这个对象叫做 DOM 对象

## 获取元素

- 通过 js 代码来获取页面中的标签
- 获取到以后我们就可以操作这些标签了

### getElementById

- `getElementById` 是通过标签的 id 名称来获取标签的
- 因为在一个页面中 id 是唯一的，所以获取到的就是一个元素

```xml
<body>
 <div id="box"></div>
 <script>
   var box = document.getElementById('box')
   console.log(box) // <div></div>
 </script>
</body>
复制代码
```

- 获取到的就是页面中的那个 id 为 box 的 div 标签

### getElementsByClassName

- `getElementsByClassName` 是用过标签的 class 名称来获取标签的
- 因为页面中可能有多个元素的 class 名称一样，所以获取到的是一组元素
- 哪怕你获取的 class 只有一个，那也是获取一组元素，只不过这一组中只有一个 DOM 元素而已

```xml
<body>
 <div calss="box"></div>
 <script>
   var box = document.getElementsByClassName('box')
    console.log(box) // [<div></div>]
    console.log(box[0]) // <div></div>
 </script>
</body>
复制代码
```

- 获取到的是一组元素，是一个长得和数组一样的数据结构，但是不是数组，是伪数组
- 这个一组数据也是按照索引排列的，所以我们想要准确的拿到这个 div，需要用索引来获取

### getElementsByTagName

- `getElementsByTagName` 是用过标签的 标签 名称来获取标签的
- 因为页面中可能有多个元素的 标签 名称一样，所以获取到的是一组元素
- 哪怕真的只有一个这个标签名，那么也是获取一组元素，只不过这一组中只有一个 DOM 元素而已

```xml
<body>
 <div></div>
 <script>
    var box = document.getElementsByTagName('div')
    console.log(box) // [<div></div>]
    console.log(box[0]) // <div></div>
 </script>
</body>
复制代码
```

- 和 getElementsByClassName 一样，获取到的是一个长得很像数组的元素
- 必须要用索引才能得到准确的 DOM 元素

### querySelector

- `querySelector` 是按照选择器的方式来获取元素
- 也就是说，按照我们写 css 的时候的选择器来获取
- 这个方法只能获取到一个元素，并且是页面中第一个满足条件的元素

```javascript
console.log(document.querySelector('div')) // 获取页面中的第一个 div 元素 
console.log(docuemnt.querySelector('.box')) // 获取页面中第一个有 box 类名的元素
console.log(document.querySelector('#box')) // 获取页面中第一个 id 名为 box 的元素
复制代码
```

### querySelectorAll

- `querySelectorAll` 是按照选择器的方式来获取元素
- 这个方法能获取到所有满足条件的元素，以一个伪数组的形式返回

```javascript
console.log(document.querySelectorAll('div')) // 获取页面中的所有的 div 元素 
console.log(docuemnt.querySelectorAll('.box')) // 获取页面中所有有 box 类名的元素
复制代码
```

- 获取到的是一组数据，也是需要用索引来获取到准确的每一个 DOM 元素

## 操作属性

- 通过我们各种获取元素的方式获取到页面中的标签以后
- 我们可以直接操作 DOM 元素的属性，就能直接把效果展示在页面上

### innerHTML

- 获取元素内部的 HTML 结构

```xml
<body>
 <div>
 <p>
 <span>hello</span>
 </p>
 </div>

 <script>
 var div = document.querySelector('div')
 console.log(div.innerHTML)
  /*
   
      <p>
        <span>hello</span>
      </p>
  
  */
 </script>
</body>
复制代码
```

- 设置元素的内容

```xml
<body>
 <div></div>

 <script>
   var div = document.querySelector('div')
   div.innerHTML = '<p>hello</p>'
 </script>
</body>
复制代码
```

- 设置完以后，页面中的 div 元素里面就会嵌套一个 p 元素

### innerText

- 获取元素内部的文本（只能获取到文本内容，获取不到 html 标签）

```xml
<body>
 <div>
 <p>
 <span>hello</span>
 </p>
 </div>

 <script>
   var div = document.querySelector('div')
   console.log(div.innerText) // hello
 </script>
</body>
复制代码
```

- 可以设置元素内部的文本

```xml
<body>
 <div></div>

 <script>
   var div = document.querySelector('div')
   div.innerText = '<p>hello</p>'
 </script>
</body>
复制代码
```

- 设置完毕以后，会把 `hello` 当作一个文本出现在 div 元素里面，而不会把 p 解析成标签

### getAttribute

- 获取元素的某个属性（包括自定义属性）

```xml
<body>
 <div a="100" class="box"></div>

 <script>
   var div = document.querySelector('div')
   console.log(div.getAttribute('a')) // 100
   console.log(div.getAttribute('class')) // box
 </script>
</body>
复制代码
```

### setAttribute

- 给元素设置一个属性（包括自定义属性）

```xml
<body>
 <div></div>

 <script>
 var div = document.querySelector('div')
   div.setAttribute('a', 100)
   div.setAttribute('class', 'box')
   console.log(div) // <div a="100" class="box"></div>
 </script>
</body>
复制代码
```

### removeAttribute

- 直接移除元素的某个属性

```xml
<body>
 <div a="100" class="box"></div>

 <script>
   var div = document.querySelector('div')
   div.removeAttribute('class')
   console.log(div) // <div a="100"></div>
 </script>
</body>
复制代码
```

### style

- 专门用来给元素添加 css 样式的
- 添加的都是行内样式

```css
<body>
 <div></div>

 <script>
   var div = document.querySelector('div')
   div.style.width = "100px"
   div.style.height = "100px"
   div.style.backgroundColor = "pink"
   console.log(div) 
 // <div style="width: 100px; height: 100px; background-color: pink;"></div>
 </script>
</body>
复制代码
```

- 页面中的 div 就会变成一个宽高都是100，背景颜色是粉色

### className

- 专门用来操作元素的 类名的

```xml
<body>
 <div class="box"></div>

 <script>
   var div = document.querySelector('div')
   console.log(div.className) // box
 </script>
</body>
复制代码
```

- 也可以设置元素的类名，不过是全覆盖式的操作

```xml
<body>
 <div class="box"></div>

 <script>
   var div = document.querySelector('div')
   div.className = 'test'
   console.log(div) // <div class="test"></div>
 </script>
</body>
复制代码
```

- 在设置的时候，不管之前有没有类名，都会全部被设置的值覆盖

