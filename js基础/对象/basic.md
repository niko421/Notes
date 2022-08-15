## 创建对象的三种方式

### 字面量创建对象

#### 对象字面量

就是花括号{}里面包含了表达这个具体事物（对象）的属性和方法。
{}里面采取键值对的形式表示.

- 键：相当于属性名.

- 值：相当于属性值，可以是任意类型的值（数字类型、字符串类型、布尔类型，函数类型等）

```
var star = {
    name : 'cess',
    age : 18,
    sex : '男',
    sayHi : function() {
        alert('大家好啊~');
    }
};
```

#### 对象调用

- 对象里面的属性调用：对象.属性名，
- 对象里面属性的另一种调用方式：对象["属性名']，注意方括号里面的属性必须加引号，
- 对象里面的方法调用：对象.方法名( )，注意这个方法名字后面一定加括号.

```
// 1.调用对象的属性 我们采取 对象名.属性名 . 我们理解为 的
console.log(obj.uname);

// 2.调用属性还有一种方法 对象名['属性名']
console.log(obj['age']);

// 3.调用对象的方法 sayHi   对象名.方法名() 千万别忘记添加小括号
obj.sayHi();
```



### new Object

- Object( ):第一个字母大写.
- new Object0:需要new关键字.
- 利用等号=赋值的方法添加对象的属性和方法，使用格式：对象.属性=值；

```
var cess = new Obect();
cess.name = 'cess';
cess.age = 18;
cess.sex = '男';
cess.sayHi = function() {
    alert('大家好啊~');
}
```



### 构造函数

​	构造函数：是一种特殊的函数，主要用来初始化对象，即为对像成员变量赋初始值，它总与new运算符一起使
用。我们可以把对象中一些公共的属性和方法抽取出来，然后封装到这个函数里面。

​           注意

- 构造函数约定首字母大写.
- 函数内的属性和方法前面需要添加this,表示当前对象的属性和方法.
- 构造函数中不需要return返回结果.
- 当我们创健对象的时候，必须用new来调用构造函数。

```
function Person(uname, age, sex) {
     this.name = name;
     this.age = age;
     this.sex = sex;
     this.sayHi = function() {
      alert('我的名字叫：' + this.name + '，年龄：' + this.age + '，性别：' + this.sex);
    }
}
var bigbai = new Person('大白', 100, '男');
var smallbai = new Person('小白', 21, '男');
console.log(bigbai.name);
console.log(smallbai.name);
```

#### new关键字

1. new在执行时会做四件事情
2. 在内存中创建一个新的空对象
3. 让this指向这个新的对象
4. 执行构造函数里面的代码，给这个新对象添加属性和方法
5. 返回这个新对象（所以构造函数里面不需要return)。