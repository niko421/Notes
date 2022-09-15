## 接口（Interface）

- 接口的作用类似于抽象类，不同点在于：接口中的所有方法和属性都是没有实值的，换句话说接口中的所有方法都是抽象方法；

- 接口主要负责定义一个类的结构，接口可以去限制一个对象的接口：对象只有包含接口中定义的所有属性和方法时才能匹配接口；

- 同时，可以让一个类去实现接口，实现接口时类中要保护接口中的所有属性；




#### 检查对象类型

```typescript
interface Person{
    name: string;
    sayHello():void;
}

function fn(per: Person){
    per.sayHello();
}

fn({name:'孙悟空', sayHello() {console.log(`Hello, 我是 ${this.name}`)}});
```



#### 类实现接口

 ```typescript
interface Person{
    name: string;
    sayHello():void;
}

class Student implements Person{
    constructor(public name: string) {
    }

    sayHello() {
        console.log('大家好，我是'+this.name);
    }
}
 ```

#####  

#### 一个类可以实现多个接口

```
interface Alarm {
    alert(): void;
}

interface Light {
    lightOn(): void;
    lightOff(): void;
}

class Car implements Alarm, Light {
    alert() {
        console.log('Car alert');
    }
    lightOn() {
        console.log('Car light on');
    }
    lightOff() {
        console.log('Car light off');
    }
}
```

-  上例中，`Car` 实现了 `Alarm` 和 `Light` 接口，既能报警，也能开关车灯。 





#### 接口继承接口

- 类似于class继承,继承之后新interface拥有前辈所有的属性和方法,可以随时调用或者重写.

- 接口与接口之间可以是继承关系：

  ```ts
  interface Alarm {
      alert(): void;
  }
  
  interface LightableAlarm extends Alarm {
      lightOn(): void;
      lightOff(): void;
  }
  ```

- 这很好理解，`LightableAlarm` 继承了 `Alarm`，除了拥有 `alert` 方法之外，还拥有两个新方法 `lightOn` 和 `lightOff`。

