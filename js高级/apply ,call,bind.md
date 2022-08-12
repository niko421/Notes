### this指向

 在 ES5 中，其实 this 的指向，始终坚持一个原理：**this 永远指向最后调用它的那个对象** 

例子1

    var name = "windowsName";
    function a() {
        var name = "Cherry";
    
        console.log(this.name);        //windowsName
    
        console.log("inner:" + this); //inner:Window
    }
    a();
    console.log("outer:" + this)       //outer:Window
 最后调用 a的地方 `a();`，前面没有调用的对象那么就是全局对象 window，这就相当于是 `window.a()` 

例子2

    var name = "windowsName";
    var a = {
        name: "Cherry",
        fn : function () {
            console.log(this.name);      // Cherry
        }
    }
    a.fn();
 函数 fn 是对象 a 调用的，所以打印的值就是 a 中的 name 的值。

例子3

```
var name = "windowsName";
    var a = {
        name: "Cherry",
        fn : function () {
            console.log(this.name);      // Cherry
        }
    }
    window.a.fn();
```

 这里打印 Cherry 的原因也是因为刚刚那句话“**this 永远指向最后调用它的那个对象**”，最后调用它的对象仍然是对象 a。 

例子4

    var name = "windowsName";
    var a = {
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // undefined
        }
    }
    window.a.fn();
这里为什么会打印 `undefined` 呢？这是因为正如刚刚所描述的那样，调用 fn 的是 a 对象，也就是说 fn 的内部的 this 是对象 a，而对象 a 中并没有对 name 进行定义，所以 log 的 `this.name` 的值是 `undefined`。


例子5

    var name = "windowsName";
    var a = {
        name : null,
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // windowsName
        }
    }
    
    var f = a.fn;
    f();
 由于刚刚的 f 并没有调用，所以 `fn()` 最后仍然是被 window 调用的。所以 this 指向的也就是 window。 

例子6

    var name = "windowsName";
    
    function fn() {
        var name = 'Cherry';
        innerFunction();
        function innerFunction() {
            console.log(this.name);      // windowsName
        }
    }
    
    fn()
在函数内调用 没写就是window对象

### 改变 this 的指向

改变 this 的指向我总结有以下几种方法：

- 使用 ES6 的箭头函数
- 在函数内部使用 `_this = this`
- 使用 `apply`、`call`、`bind`
- new 实例化一个对象

例子1

    var name = "windowsName";
    
    var a = {
        name : "Cherry",
    
        func1: function () {
            console.log(this.name)     
        },
    
        func2: function () {
            setTimeout(  function () {
                this.func1()
            },100);
        }
    
    };
    
    a.func2()     // this.func1 is not a function
 在不使用箭头函数的情况下，是会报错的，因为最后调用 `setTimeout` 的对象是 window，但是在 window 中并没有 func1 函数。 

#### 箭头函数

例子2

    var name = "windowsName";
    
    var a = {
        name : "Cherry",
    
        func1: function () {
            console.log(this.name)     
        },
    
        func2: function () {
            setTimeout( () => {
                this.func1()
            },100);
        }
    
    };
    
    a.func2()     // Cherry
#### 在函数内部使用 `_this = this`

 先将调用这个函数的对象保存在变量 `_this` 中，然后在函数中都使用这个 `_this`，这样 `_this` 就不会改变了。 

例子3

    var name = "windowsName";
    
    var a = {
    
        name : "Cherry",
    
        func1: function () {
            console.log(this.name)     
        },
    
        func2: function () {
            var _this = this;
            setTimeout( function() {
                _this.func1()
            },100);
        }
    
    };
    
    a.func2()       // Cherry
这个例子中，在 func2 中，首先设置 `var _this = this;`，这里的 `this` 是调用 `func2` 的对象 a，为了防止在 `func2` 中的 setTimeout 被 window 调用而导致的在 setTimeout 中的 this 为 window。我们将 `this(指向变量 a)` 赋值给一个变量 `_this`，这样，在 `func2` 中我们使用 `_this` 就是指向对象 a 了。

#### 

#### apply

    var a = {
        name : "Cherry",
    
        func1: function () {
            console.log(this.name)
        },
    
        func2: function () {
            setTimeout(  function () {
                this.func1()
            }.apply(a),100);
        }
    
    };
    
    a.func2()            // Cherry
#### call

    var a = {
        name : "Cherry",
    
        func1: function () {
            console.log(this.name)
        },
    
        func2: function () {
            setTimeout(  function () {
                this.func1()
            }.call(a),100);
        }
    
    };
    
    a.func2()            // Cherry
#### bind

    var a = {
        name : "Cherry",
    
        func1: function () {
            console.log(this.name)
        },
    
        func2: function () {
            setTimeout(  function () {
                this.func1()
            }.bind(a)(),100);
        }
    
    };
    
    a.func2()            // Cherry
### apply、call、bind 区别

##### apply

>  apply() 方法调用一个函数, 其具有一个指定的this值，以及作为一个数组（或类似数组的对象）提供的参数 

语法

>  fun.apply(thisArg, [argsArray]) 

thisArg：在 fun 函数运行时指定的 this 值。需要注意的是，指定的 this 值并不一定是该函数执行时真正的 this 值，如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的自动包装对象。

argsArray：**一个数组或者类数组对象**，其中的数组元素将作为单独的参数传给 fun 函数。如果该参数的值为null 或 undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。浏览器兼容性请参阅本文底部内容。

##### call

语法:

> fun.call(thisArg[, arg1[, arg2[, ...]]])

所以 apply 和 call 的区别是 call 方法接受的是若干个参数列表，而 apply 接收的是一个包含多个参数的数组。

    var a ={
        name : "Cherry",
        fn : function (a,b) {
            console.log( a + b)
        }
    }
    
    var b = a.fn;
    b.apply(a,[1,2])     // 3
    var a ={
        name : "Cherry",
        fn : function (a,b) {
            console.log( a + b)
        }
    }
    
    var b = a.fn;
    b.call(a,1,2)       // 3
##### bind

    var a ={
        name : "Cherry",
        fn : function (a,b) {
            console.log( a + b)
        }
    }
    
    var b = a.fn;
    b.bind(a,1,2)
>  bind()方法创建一个新的函数, 当被调用时，将其this关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列。 

 所以我们可以看出，bind 是创建一个新的函数，我们必须要手动去调用： 

    var a ={
        name : "Cherry",
        fn : function (a,b) {
            console.log( a + b)
        }
    }
    
    var b = a.fn;
    b.bind(a,1,2)()           // 3