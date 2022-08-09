#### 语法
- 内容类型（ContentType）

  HTML5的文件扩展符与内容类型保持不变，仍然为".html"或".htm"
-  DOCTYPE声明

  不区分大小写

- 指定字符集编码

  <meta charset="UTF-8">

- 可省略标记的元素

  不允许写结束标记的元素：br、col、embed、hr、img、input、、link、meta
- 可以省略结束标记的元素：

  li、dt、dd、p、option、colgroup、thead、tbody、tfoot、tr、td、th

- 可以省略全部标记的元素：

  html、head、body、colgroup、tbody

- 属性值可以使用双引号，也可以使用单引号。
#### 语义化标签
> 在HTML5出来之前，我们用div来表示页面章节，但是这些div都没有实际意义。（即使我们用css样式的id和class形容这块内容的意义）。这些标签只是我们提供给浏览器的指令，只是定义一个网页的某些部分。但现在，那些之前没“意义”的标签因为因为html5的出现消失了，这就是我们平时说的“语义”。

- section元素 表示页面中的一个区块
- article元素 表示一块与上下文无关的独立的内容
-  aside元素 在article之外的，与article内容相关的辅助信息
- header元素 表示页面中一个内容区块或整个页面的标题
- footer元素 表示页面中一个内容区块或整个页面的脚注
- nav元素 表示页面中导航链接部分
- figure元素 表示一段独立的内容，使用figcaption元素为其添加标题(第一个或最后一个子元素的位置)
- main元素 表示页面中的主要的内容(ie不兼容)
- hgroup标题的一个分组
- mark定义高亮显示的文本(span)time时间dialog标记定义一个对话框(会话框)类似微信 
- embed 标记定义外部的可交互的内容或插件 比如flash

#### 多媒体标签

```
<video src=""></video> 
<audio src=""></audio>
```

- 属性
  - controls属性：如果出现该属性，则向用户显示控件，比如播放按钮。
  - autoplay属性：如果出现该属性，则视频在就绪后马上播放。
  - loop属性：重复播放属性。
  - muted属性：静音属性。
  - poster属性：规定视频正在下载时显示的图像，直到用户点击播放按钮。

<br>

#### HTML5表单新增
- type类型     
> Type=“email”   限制用户必须输入email类型
> Type=“url”       限制用户必须输入url类型
> Type=“range”   产生一个滑动条表单
> Type=“number”   限制用户必须输入number类型
> Type=“search”   产生一个搜索意义的表单
> Type=“color”     生成一个颜色选择的表单
> Type=“time”      限制用户必须输入时间类型
> Type=“month”        限制用户必须输入月类型
> Type=“week” 限制用户必须输入周类型
> Type=“datetime-local”        选取本地时间  
> Type=”date”        
>
> - 新增表单属性     
> required     监测是否为空。
> min      最小
> max      最大
> step     步幅  确定一个法定值。3 0 3 6 9 
> list     必须结合datalist标签，绑定datalist  id名称。
> autocomplete    是否自动提示信息    属性值   on    off
> placeholder    文本框的提示信息
> autofocus      自动聚焦。一个页面只能由一个。
> pattern    后面的属性值是一个正则表达式。
> novalidate     取消验证
> multiple     选择（上传）多个

- 新增表单标签
  - output

```
<form action="" oninput="x.value=parseInt(a.value)+parseInt(b.value)">
	<input id="a" type="range" min="0" max="100">100+
	<input id="b" type="text" value="50">=
	<output name="x" for="a b"></output>
</form>
```

- ​        datalist   必须和list属性结合使用。做提示信息。

```
<input type="url"     list="url_list"    name="link" />
<datalist    id="url_list"> 
    <option label="W3School" value="http://www.W3School.com.cn" /> 
    <option label="Google" value="http://www.google.com" /> 
    <option label="Microsoft" value="http://www.microsoft.com" /> 
</datalist>
```

