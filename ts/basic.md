### 1.基本数据类型

>  原始数据类型包括：布尔值、数值、字符串、`null`、`undefined` 以及 ES6 中的新类型 [`Symbol`](http://es6.ruanyifeng.com/#docs/symbol) 和 ES10 中的新类型 [`BigInt`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)。 



#### 类型声明

- 类型声明是TS非常重要的一个特点；

- 通过类型声明可以指定TS中变量（参数、形参）的类型；

- 指定类型后，当为变量赋值时，TS编译器会自动检查值是否符合类型声明，符合则赋值，否则报错；

- 简而言之，类型声明给变量设置了类型，使得变量只能存储某种类型的值；

- 语法：

  ```
  let 变量: 类型;
  
  let 变量: 类型 = 值;
  
  function fn(参数: 类型, 参数: 类型): 类型{
      ...
  }
  ```

  

#####   Boolean 

- ```ts
  let isDone: boolean = false;
  ```

##### number

- ```ts
  let decLiteral: number = 6;
  let hexLiteral: number = 0xf00d;
  // ES6 中的二进制表示法
  let binaryLiteral: number = 0b1010;
  // ES6 中的八进制表示法
  let octalLiteral: number = 0o744;
  let notANumber: number = NaN;
  let infinityNumber: number = Infinity;
  ```

##### string

- ```ts
  let myName: string = 'Tom';
  let myAge: number = 25;
  
  // 模板字符串
  let sentence: string = `Hello, my name is ${myName}.
  I'll be ${myAge + 1} years old next month.`;
  ```

##### void

- ​        在 TypeScript 中，可以用 `void` 表示没有任何返回值的函数：

- ```ts
  function alertName(): void {
      alert('My name is Tom');
  }
  ```

##### Null 和 Undefined

- ```ts
  let u: undefined = undefined;
  let n: null = null;
  ```

-  与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋   值给 `number` 类型的变量：

- ```ts
  // 这样不会报错
  let num: number = undefined;
  // 这样也不会报错
  let u: undefined;
  let num: number = u;
  ```

​        而 `void` 类型的变量不能赋值给 `number` 类型的变量：

- ```ts
  let u: void;
  let num: number = u;
  
  // Type 'void' is not assignable to type 'number'.
  ```

### 2.any

 任意值（Any）用来表示允许赋值为任意类型。 

- ```ts
  let myFavoriteNumber: any = 'seven';
  myFavoriteNumber = 7;
  ```

变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型

- ```ts
  let something;
  something = 'seven';
  something = 7;
  
  something.setName('Tom');
  ```



### 3.unknow

- unknown 可以定义任何类型的值

  ```
  let value: unknown;
   
  value = true;             // OK
  value = 42;               // OK
  value = "Hello World";    // OK
  value = [];               // OK
  value = {};               // OK
  value = null;             // OK
  value = undefined;        // OK
  value = Symbol("type");   // OK
  
  ```



#### any和unknow区别

- unknow类型不能作为子类型只能作为父类型 any可以作为父类型和子类型

- unknown类型不能赋值给其他类型

- ```
  //报错
  let names:unknown = '123'
  let names2:string = names
   
  //这样就没问题 any类型是可以的
  let names:any = '123'
  let names2:string = names   
   
  //unknown可赋值对象只有unknown 和 any
  let b:unknown = '123'
  let a:any= '456'
   
  a = b
  ```

  ```
  如果是any类型在对象没有这个属性的时候还在获取是不会报错的
  let obj:any = {b:1}
  obj.a
   
   
  如果是unknow 是不能调用属性和方法
  let obj:unknown = {b:1,ccc:():number=>213}
  obj.b
  obj.ccc()
  ```

  

### 4.类型推论

-  TypeScript 会在没有明确的指定类型的时候推测出一个类型，这就是类型推论。 

- ```ts
  let myFavoriteNumber = 'seven';
  myFavoriteNumber = 7;
  
  // index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
  ```



### 5.联合类型

-  联合类型（Union Types）表示取值可以为多种类型中的一种。 

- 联合类型使用 `|` 分隔每个类型。

  - ```ts
    let myFavoriteNumber: string | number;
    myFavoriteNumber = 'seven';
    myFavoriteNumber = 7;
    let myFavoriteNumber: string | number;
    myFavoriteNumber = true;
    
    // index.ts(2,1): error TS2322: Type 'boolean' is not assignable to type 'string | number'.
    //   Type 'boolean' is not assignable to type 'number'.
    ```

  - 这里的 `let myFavoriteNumber: string | number` 的含义是，允许 `myFavoriteNumber` 的类型是 `string` 或者 `number`，但是不能是其他类型。

- type

  - ```
    type mytype = string | number;
    let myFavoriteNumber: mytype;
    ```

  

#### 访问联合类型的属性或方法

- 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**：
  - ```ts
    function getLength(something: string | number): number {
        return something.length;
    }
    
    // index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
    //   Property 'length' does not exist on type 'number'.
    ```

  - 上例中，`length` 不是 `string` 和 `number` 的共有属性，所以会报错。

- 访问 `string` 和 `number` 的共有属性是没问题的：
  - ```ts
    function getString(something: string | number): string {
        return something.toString();
    }
    ```

- 联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型：
  - ```ts
    let myFavoriteNumber: string | number;
    myFavoriteNumber = 'seven';
    console.log(myFavoriteNumber.length); // 5
    myFavoriteNumber = 7;
    console.log(myFavoriteNumber.length); // 编译时报错
    
    // index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'.
    ```

  - 上例中，第二行的 `myFavoriteNumber` 被推断成了 `string`，访问它的 `length` 属性不会报错。

  - 而第四行的 `myFavoriteNumber` 被推断成了 `number`，访问它的 `length` 属性时就报错了。



### 6.类型别名

-  我们使用 `type` 创建类型别名 

  ```
  type Name = string;
  type NameResolver = () => string;
  type NameOrResolver = Name | NameResolver;
  function getName(n: NameOrResolver): Name {
      if (typeof n === 'string') {
          return n;
      } else {
          return n();
      }
  }
  ```

  

### 7.枚举

-  枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。 

- 枚举使用 `enum` 关键字来定义：

  ```ts
  enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
  ```

- 枚举成员会被赋值为从 `0` 开始递增的数字，同时也会对枚举值到枚举名进行反向映射：

  ```ts
  enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
  
  console.log(Days["Sun"] === 0); // true
  console.log(Days["Mon"] === 1); // true
  console.log(Days["Tue"] === 2); // true
  console.log(Days["Sat"] === 6); // true
  
  console.log(Days[0] === "Sun"); // true
  console.log(Days[1] === "Mon"); // true
  console.log(Days[2] === "Tue"); // true
  console.log(Days[6] === "Sat"); // true
  ```

#### 手动赋值

- 我们也可以给枚举项手动赋值：

  ```ts
  enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};
  
  console.log(Days["Sun"] === 7); // true
  console.log(Days["Mon"] === 1); // true
  console.log(Days["Tue"] === 2); // true
  console.log(Days["Sat"] === 6); // true
  ```

- 上面的例子中，未手动赋值的枚举项会接着上一个枚举项**递增。**



### 8.对象的类型——接口



#### 什么是接口

> 在面向对象语言中，接口（Interfaces）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implement）。
>
> TypeScript 中的接口是一个非常灵活的概念，除了可用于[对类的一部分行为进行抽象](https://ts.xcatliu.com/advanced/class-and-interfaces.html#类实现接口)以外，也常用于对「对象的形状（Shape）」进行描述。

#### 简单的例子

- ```ts
  interface Person {
      name: string;
      age: number;
  }
  
  let tom: Person = {
      name: 'Tom',
      age: 25
  };
  ```

  - 上面的例子中，我们定义了一个接口 `Person`，接着定义了一个变量 `tom`，它的类型是 `Person`。这样，我们就约束了 `tom` 的形状必须和接口 `Person` 一致。

- 接口一般首字母大写。**有的编程语言中会建议接口的名称加上 `I` 前缀**

- **定义的变量比接口少了一些属性是不允许的：**

  - ```ts
    interface Person {
        name: string;
        age: number;
    }
    
    let tom: Person = {
        name: 'Tom'
    };
    
    // index.ts(6,5): error TS2322: Type '{ name: string; }' is not assignable to type 'Person'.
    //   Property 'age' is missing in type '{ name: string; }'.
    ```

- 多一些属性也是不允许的：
  - ```ts
    interface Person {
        name: string;
        age: number;
    }
    
    let tom: Person = {
        name: 'Tom',
        age: 25,
        gender: 'male'
    };
    
    // index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
    //   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
    ```

- 可见，**赋值的时候，变量的形状必须和接口的形状保持一致**。



#### 可选属性

- 有时我们希望不要完全匹配一个形状，那么可以用可选属性：
  - ```ts
    interface Person {
        name: string;
        age?: number;
    }
    
    let tom: Person = {
        name: 'Tom'
    };
    interface Person {
        name: string;
        age?: number;
    }
    
    let tom: Person = {
        name: 'Tom',
        age: 25
    };
    ```

- 可选属性的含义是该属性可以不存在。

- 这时**仍然不允许添加未定义的属性**：
  - ```ts
    interface Person {
        name: string;
        age?: number;
    }
    
    let tom: Person = {
        name: 'Tom',
        age: 25,
        gender: 'male'
    };
    
    // examples/playground/index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
    //   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
    ```



#### 任意属性

-  我们在自定义类型的时候，有可能会希望一个接口允许有任意的属性签名，这时候 `任意属性` 就派上用场了。 
- 好处是接口只需要定义一个约束即可,不建议联合约束key ,一般对象是key用string,数组key用number。

##### string 类型任意属性

- 属性签名是 `string`，比如对象的属性：
  - ```ts
    interface A {
        [prop: string]: number | string;
    }
    
    const obj: A = {
        a: 1,
        b: 3,
    };
    ```

  - `[prop: string]: number` 的意思是，`A` 类型的对象可以有任意属性签名，`string` 指的是对象的键都是字符串类型的，`number` 则是指定了属性值的类型。

  - `prop` 类似于函数的形参，是可以取其他名字的。

##### number 类型任意属性

- 属性签名是 `number` 类型的，比如数组下标：
  - ```ts
    interface B {
        [index: number]: string | number;
    }
    
    const arr: B = ['suukii'];
    ```

  - `[index: number]: string` 的意思是，`B` 类型的数组可以有任意的数字下标，而且数组的成员的类型必须是 `string`。

  - 同样的，`index` 也只是类似于函数形参的东西，用其他标识符也是完全可以的。



#### 只读属性

- 有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性：
  - ```ts
    interface Person {
        readonly id: number;
        name: string;
        age?: number;
        [propName: string]: any;
    }
    
    let tom: Person = {
        id: 89757,
        name: 'Tom',
        gender: 'male'
    };
    
    tom.id = 9527;
    
    // index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
    ```

  - 上例中，使用 `readonly` 定义的属性 `id` 初始化后，又被赋值了，所以报错了。

- **注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**：

  - ```ts
    interface Person {
        readonly id: number;
        name: string;
        age?: number;
        [propName: string]: any;
    }
    
    let tom: Person = {
        name: 'Tom',
        gender: 'male'
    };
    
    tom.id = 89757;
    
    // index.ts(8,5): error TS2322: Type '{ name: string; gender: string; }' is not assignable to type 'Person'.
    //   Property 'id' is missing in type '{ name: string; gender: string; }'.
    // index.ts(13,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
    ```

  - 上例中，报错信息有两处，第一处是在对 `tom` 进行赋值的时候，没有给 `id` 赋值。

  - 第二处是在给 `tom.id` 赋值的时候，由于它是只读属性，所以报错了。

  



### 9.数组的类型

#### : 类型 + 方括号  表示法

- 最简单的方法是使用「类型 + 方括号」来表示数组：
  - ```ts
    let fibonacci: number[] = [1, 1, 2, 3, 5];
    ```

- 数组的项中**不允许**出现其他的类型：
  - ```ts
    let fibonacci: number[] = [1, '1', 2, 3, 5];
    
    // Type 'string' is not assignable to type 'number'.
    ```

- 数组的一些方法的参数也会根据数组在定义时约定的类型进行限制：
  - ```ts
    let fibonacci: number[] = [1, 1, 2, 3, 5];
    fibonacci.push('8');
    
    // Argument of type '"8"' is not assignable to parameter of type 'number'.
    ```

  - 上例中，`push` 方法只允许传入 `number` 类型的参数，但是却传了一个 `"8"` 类型的参数，所以报错了。这里 `"8"` 是一个字符串字面量类型，会在后续章节中详细介绍。



#### 数组泛型

- 我们也可以使用数组泛型（Array Generic） `Array` 来表示数组：
  - ```ts
    let fibonacci: Array<number> = [1, 1, 2, 3, 5];
    ```

关于泛型，可以参考[泛型](https://ts.xcatliu.com/advanced/generics.html)一章。



#### 用接口表示数组

- 接口也可以用来描述数组：
  - ```ts
    interface NumberArray {
        [index: number]: number;
    }
    let fibonacci: NumberArray = [1, 1, 2, 3, 5];
    ```

  - `NumberArray` 表示：只要索引的类型是数字时，那么值的类型必须是数字。

- 虽然接口也可以用来描述数组，但是我们一般不会这么做，因为这种方式比前两种方式复杂多了。

- 不过有一种情况例外，那就是它常用来表示类数组。



#### 类数组

- 类数组（Array-like Object）不是数组类型，比如 `arguments`：
  - ```ts
    function sum() {
        let args: number[] = arguments;
    }
    
    // Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 24 more.
    ```

- 上例中，`arguments` 实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口：
  - ```ts
    function sum() {
        let args: {
            [index: number]: number;
            length: number;
            callee: Function;
        } = arguments;
    }
    ```

- 在这个例子中，我们除了约束当索引的类型是数字时，值的类型必须是数字之外，也约束了它还有 `length` 和 `callee` 两个属性。

- 事实上常用的类数组都有自己的接口定义，如 `IArguments`, `NodeList`, `HTMLCollection` 等：
  - ```ts
    function sum() {
        let args: IArguments = arguments;
    }
    ```

- 其中 `IArguments` 是 TypeScript 中定义好了的类型，它实际上就是：
  - ```ts
    interface IArguments {
        [index: number]: any;
        length: number;
        callee: Function;
    }
    ```

关于内置对象，可以参考[内置对象](https://ts.xcatliu.com/basics/built-in-objects.html)一章。



#### any 在数组中的应用

- 一个比较常见的做法是，用 `any` 表示数组中允许出现任意类型：
  - ```ts
    let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
    ```



#### 元组( Tuple )

- 定义一对值分别为 `string` 和 `number` 的元组：

  ```ts
  let tom: [string, number] = ['Tom', 25];
  ```

- 当赋值或访问一个已知索引的元素时，会得到正确的类型：

  ```ts
  let tom: [string, number];
  tom[0] = 'Tom';
  tom[1] = 25;
  
  tom[0].slice(1);
  tom[1].toFixed(2);
  ```

- 但是当直接对元组类型的变量进行初始化或者赋值的时候，需要提供所有元组类型中指定的项。

  ```ts
  let tom: [string, number];
  tom = ['Tom', 25];
  let tom: [string, number];
  tom = ['Tom'];
  
  // Property '1' is missing in type '[string]' but required in type '[string, number]'.
  ```

##### 越界的元素

- 当添加越界的元素时，它的类型会被限制为元组中每个类型的联合类型：

- ```ts
  let tom: [string, number];
  tom = ['Tom', 25];
  tom.push('male');
  tom.push(true);
  
  // Argument of type 'true' is not assignable to parameter of type 'string | number'.
  ```



### 10.函数的类型



#### 函数声明

- 在 JavaScript 中，有两种常见的定义函数的方式——函数声明（Function Declaration）和函数表达式（Function Expression）：

  ```js
  // 函数声明（Function Declaration）
  function sum(x, y) {
      return x + y;
  }
  
  // 函数表达式（Function Expression）
  let mySum = function (x, y) {
      return x + y;
  };
  ```

- 一个函数有输入和输出，要在 TypeScript 中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义较简单：

  ```ts
  function sum(x: number, y: number): number {
      return x + y;
  }
  ```

- 注意，**输入多余的（或者少于要求的）参数，是不被允许的**：

  ```ts
  function sum(x: number, y: number): number {
      return x + y;
  }
  sum(1, 2, 3);
  
  // index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
  function sum(x: number, y: number): number {
      return x + y;
  }
  sum(1);
  
  // index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
  ```



#### 函数表达式

- 如果要我们现在写一个对函数表达式（Function Expression）的定义，可能会写成这样：

  ```ts
  let mySum = function (x: number, y: number): number {
      return x + y;
  };
  ```

- 这是可以通过编译的，不过事实上，上面的代码只对等号右侧的匿名函数进行了类型定义，而等号左边的 `mySum`，是通过赋值操作进行类型推论而推断出来的。如果需要我们手动给 `mySum` 添加类型，则应该是这样：

  ```ts
  let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
      return x + y;
  };
  ```

- 等号右边返回值number可以不写
- 注意不要混淆了 TypeScript 中的 `=>` 和 ES6 中的 `=>`。
- 在 TypeScript 的类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。



#### 用接口定义函数的形状

- 我们也可以使用接口的方式来定义一个函数需要符合的形状：

  ```ts
  interface SearchFunc {
      (source: string, subString: string): boolean;
  }
  
  let mySearch: SearchFunc;
  mySearch = function(source: string, subString: string) {
      return source.search(subString) !== -1;
  }
  ```

- 采用函数表达式|接口定义函数的方式时，对等号左侧进行类型限制，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变。



#### 可选参数

- 前面提到，输入多余的（或者少于要求的）参数，是不允许的。那么如何定义可选的参数呢？

- 与接口中的可选属性类似，我们用 `?` 表示可选的参数：

  ```ts
  function buildName(firstName: string, lastName?: string) {
      if (lastName) {
          return firstName + ' ' + lastName;
      } else {
          return firstName;
      }
  }
  let tomcat = buildName('Tom', 'Cat');
  let tom = buildName('Tom');
  ```

- 需要注意的是，可选参数必须接在必需参数后面。换句话说，**可选参数后面不允许再出现必需参数了**：

  ```ts
  function buildName(firstName?: string, lastName: string) {
      if (firstName) {
          return firstName + ' ' + lastName;
      } else {
          return lastName;
      }
  }
  let tomcat = buildName('Tom', 'Cat');
  let tom = buildName(undefined, 'Tom');
  
  // index.ts(1,40): error TS1016: A required parameter cannot follow an optional parameter.
  ```



#### 参数默认值

- 在 ES6 中，我们允许给函数的参数添加默认值，**TypeScript 会将添加了默认值的参数识别为可选参数**：

  ```ts
  function buildName(firstName: string, lastName: string = 'Cat') {
      return firstName + ' ' + lastName;
  }
  let tomcat = buildName('Tom', 'Cat');
  let tom = buildName('Tom');
  ```

- 此时就不受「可选参数必须接在必需参数后面」的限制了：

  ```ts
  function buildName(firstName: string = 'Tom', lastName: string) {
      return firstName + ' ' + lastName;
  }
  let tomcat = buildName('Tom', 'Cat');
  let cat = buildName(undefined, 'Cat');
  ```

> 关于默认参数，可以参考 [ES6 中函数参数的默认值](http://es6.ruanyifeng.com/#docs/function#函数参数的默认值)。



#### 剩余参数

- ES6 中，可以使用 `...rest` 的方式获取函数中的剩余参数（rest 参数）：

  ```js
  function push(array, ...items) {
      items.forEach(function(item) {
          array.push(item);
      });
  }
  
  let a: any[] = [];
  push(a, 1, 2, 3);
  ```

- 事实上，`items` 是一个数组。所以我们可以用数组的类型来定义它：

  ```ts
  function push(array: any[], ...items: any[]) {
      items.forEach(function(item) {
          array.push(item);
      });
  }
  
  let a = [];
  push(a, 1, 2, 3);
  ```

- 注意，rest 参数只能是最后一个参数，关于 rest 参数，可以参考 [ES6 中的 rest 参数](http://es6.ruanyifeng.com/#docs/function#rest参数)。

  



#### 重载

- 重载允许一个函数接受不同数量或类型的参数时，作出不同的处理,和返回值无关.

- 比如，我们需要实现一个函数 `reverse`，输入数字 `123` 的时候，输出反转的数字 `321`，输入字符串 `'hello'` 的时候，输出反转的字符串 `'olleh'`。

- 利用联合类型，我们可以这么实现：

  ```ts
  function reverse(x: number | string): number | string | void {
      if (typeof x === 'number') {
          return Number(x.toString().split('').reverse().join(''));
      } else if (typeof x === 'string') {
          return x.split('').reverse().join('');
      }
  }
  ```

- **然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。**

- 这时，我们可以使用重载定义多个 `reverse` 的函数类型：

  ```ts
  function reverse(x: number): number;
  function reverse(x: string): string;
  function reverse(x: number | string): number | string | void {
      if (typeof x === 'number') {
          return Number(x.toString().split('').reverse().join(''));
      } else if (typeof x === 'string') {
          return x.split('').reverse().join('');
      }
  }
  ```

- 上例中，我们重复定义了多次函数 `reverse`，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示。
- 注意，TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。