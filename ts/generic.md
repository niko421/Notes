## 泛型（Generic）

定义一个函数或类时，有些情况下无法确定其中要使用的具体类型（返回值、参数、属性的类型不能确定）；

此时泛型便能够发挥作用；

举个例子：

```typescript
function test(arg: any): any{
    return arg;
}
```

上例中，test函数有一个参数类型不确定，但是能确定的时其返回值的类型和参数的类型是相同的；

由于类型不确定所以参数和返回值均使用了any，但是很明显这样做是不合适的：

首先使用any会关闭TS的类型检查，其次这样设置也不能体现出参数和返回值是相同的类型；

### 泛型函数

#### 创建泛型函数

```typescript
function test<T>(arg: T): T{
    return arg;
}
```

这里的`<T>`就是泛型；

T是我们给这个类型起的名字（不一定非叫T），设置泛型后即可在函数中使用T来表示该类型；

所以泛型其实很好理解，就表示某个类型；

那么如何使用上边的函数呢？

#### 使用泛型函数

##### 方式一（直接使用）：

```typescript
test(10)
```

使用时可以直接传递参数使用，类型会由TS自动推断出来，但有时编译器无法自动推断时还需要使用下面的方式

##### 方式二（指定类型）：

```typescript
test<number>(10)
```

也可以在函数后手动指定泛型；

#### 函数中声明多个泛型

可以同时指定多个泛型，泛型间使用逗号隔开：

```typescript
function test<T, K>(a: T, b: K): K{
  return b;
}

test<number, string>(10, "hello");
```

使用泛型时，完全可以将泛型当成是一个普通的类去使用；

#### 处理多个函数参数

我们用 T 代表第 0 项的类型，用 U 代表第 1 项的类型。

```TypeScript
function swap<T, U>(tuple: [T, U]): [U, T]{
    return [tuple[1], tuple[0]]
}
```

传入的参数里，第 0 项为 string 类型，第 1 项为 number 类型。

在交换函数的返回值里，第 0 项为 number 类型，第 1 项为 string 类型。



### 约束泛型

除此之外，也可以对泛型的范围进行约束

```typescript
interface MyInter{
  length: number;
}

function test<T extends MyInter>(arg: T): number{
  return arg.length;
}
```

 这其中的关键就是<T extends MyInter >，这样就能约束泛型。 

使用T extends MyInter表示泛型T必须是MyInter的子类，不一定非要使用接口类和抽象类同样适用；





## 泛型的一些应用

### 泛型约束类

定义一个栈，有入栈和出栈两个方法，如果想入栈和出栈的元素类型统一，就可以这么写：

```TypeScript
class Stack<T> {
    private data: T[] = []
    push(item:T) {
        return this.data.push(item)
    }
    pop():T | undefined {
        return this.data.pop()
    }
}
```

在定义实例的时候写类型，比如，入栈和出栈都要是 number 类型，就这么写：

```TypeScript
const s1 = new Stack<number>()
```



### 泛型约束接口

使用泛型，也可以对 interface 进行改造，让 interface 更灵活。

```TypeScript
interface IKeyValue<T, U> {
    key: T
    value: U
}

const k1:IKeyValue<number, string> = { key: 18, value: 'lin'}
const k2:IKeyValue<string, number> = { key: 'lin', value: 18}
```



### 泛型定义数组

定义一个数组，我们之前是这么写的：

```TypeScript
const arr: number[] = [1,2,3]

```

现在这么写也可以：

```TypeScript
const arr: Array<number> = [1,2,3]
```



### 