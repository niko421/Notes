### 1.使用 Pinia

##### main.ts

```
import { creatPinia } from 'pinia'

app.use(creatPinia())
```

### 2.store

#### 定义 Store

-  通过从`pinia`模块解构的`defineStore()`函数进行定义，该函数的第一个参数`name`，也称为`id`，是必要的 

- 这样建立出来的store.ts就是一个独立的模块,所以想要多个模块,只需要建立多个ts文件配置即可.不像vuex那样这么麻烦 ,去modules来实现模块化.

### 3.state

#### 定义state

```
//stores/userStore.js

import { defineStore } from 'pinia'

export const useUserStore = defineStore('userStore', {
    state: () => {
        return {
            name: "JimHome",
            age: 18
        }
    }
})

```

#### 使用state

1.实例化store模块.属性取值

```
<script setup>
import { useUserStore } from '@/stores/userStore'  //引入你想导入的store模块
const user = useUserStore()
console.log(user.age)    //18
user.age++
console.log(user.age)    //19

</script>

```

2. storeToRefs  解构

```
<template>
    <p>name: {{ test.name }}, age: {{ test.age }}</p>
    <p>name: {{ name }}, age: {{ age }}</p>
    <button class="px-4 border-2 border-solid border-black" @click="changeInfo">change</button>
</template>

<script setup lang="ts">
import { useTestStore } from '@/piniaStore/test';
import { storeToRefs } from 'pinia';
// import { toRefs } from 'vue';

const test = useTestStore()
const { name, age } = storeToRefs(test)
// const { name, age } = toRefs(test) // 也可以使用vue3的toRefs

const changeInfo = () => {
    // 1. 还是原对象改变
    // test.name = 'laoli';
    // 2. 直接使用结构对象改变
    name.value = 'laoli';
}
</script>
```



#### 重置 State

 通过调用`store`上的`$reset()`方法将`state`**重置**到其初始值 

```
import { useUserStore } from '@/stores/userStore' //引入你想导入的store模块
const user = useUserStore()
console.log(user.age)    //18
user.age = 30
console.log(user.age)    //30
user.$reset()
console.log(user.age)    //18

```

#### 改变 State

1. 通过调用`$patch()`方法修改`state`对象 ,可以修改state部分属性.

```
import { useUserStore } from '@/stores/userStore'  //引入你想导入的store模块
const userStore = useUserStore()
userStore.$patch({
    name: "Ayun",
    age: 16
})

```

 2.`$patch()`方法也接受一个函数来批量修改`state`内部分对象 

```
import { useUserStore } from '@/stores/userStore'  //引入你想导入的store模块
const userStore = useUserStore()
userStore.$patch((state) => {
    state.name = "Ayun"
    state.age = 16
    state.frends.push({
        name: "张三"
    })
})

```

 3.通过将`$state`属性设置为新对象来替换`Store`的整个状态 ,.必须全部都要赋值，不能单独一个 

```
import { useUserStore } from '@/stores/userStore'  //引入你想导入的store模块
const userStore = useUserStore()
userStore.$state = {
    name: "李四",
    age: 24
}

```

  4.pinia的actions方式 

```
import { useUserStore } from '@/stores/userStore'  //引入你想导入的store模块
const userStore = useUserStore()
userStore.setName("张三");
```

userStore.ts

```
// test.ts
import { defineStore } from 'pinia';

export const useUserStore = defineStore('userStore', {
    // ...
    actions: {
        setName(name: string) {
         //这里的this就是封装过的useUserStore
            this.name = name;
        }
    }
});
```

### 4.Getter

#### 定义 Getter

```
//stores/user.js

import { defineStore } from 'pinia'

export const useUserStore = defineStore('userStore', {
    state: () => {
        return {
            name: "JimHome",
            age: 18
        }
    },
    getter: {
        //通过state
        doubleAge(state) {
            return state.age * 2
        },
        //通过this，需要定义返回类型
        tripleAge(): number {
            return this.age * 3
        }
    }
})

```

#### 使用 Getter

```
<script setup>
import { useUserStore } from '@/stores/userStore'

const user = useUserStore()
console.log(user.doubleAge)    //36

</script>

```

#### Getter 中使用其他 Getter

```
getter: {
    doubleAge(state) {
        return state.age * 2
    },
    //自动完成，不需要定义返回类型
    quadrupleAge() {
        return this.doubleAge * 2
    }
}

```

#### 使用其他 Store 的数据

```
import { defineStore } from 'pinia'
import { useOtherStore } from './otherStore'  //这里引入其他store模块,然后实例化就行

export const useUserStore = defineStore('userStore', {
    state: () => {
        return {
            name: "JimHome",
            age: 18
        }
    },
    getter: {
        otherGetter(state) {
            const otherStore = useOtherStore()
            return state.name + useOtherStore.data
        }
    }
})

```

### 5.Action

#### 定义Action

```
import { defineStore } from 'pinia';

interface User {
    name: string;
    age: number;
}

const asyncSetName = () => {
    return new Promise<User>((resolve) => {
        setTimeout(() => {
            resolve({
                name: 'laowang',
                age: 20
            })
        }, 2000);
    })
}

export const useTestStore = defineStore('userStore', {
    state: () => {
        return {
            name: 'xiaoming',
            age: 18
        }
    },
    getters: {
        personInfo({ name, age }) {
            return `name: ${name}, age: ${age}`;
        }
    },
    
    actions: {
        async setName() {
            const { name, age } = await asyncSetName();
            this.name = name;
            this.age = age;
        }
    }
});
```

#### 使用 Action

```
import { useUserStore } from '@/stores/userStore'

const user = useUserStore()
console.log(user.setName())    //JimHome

```

#### 使用其他 Store 的数据

```
import { defineStore } from 'pinia'
import { useOtherStore } from './otherStore'

export const useUserStore = defineStore('userStore', {
    state: () => {
        return {
            name: "JimHome",
            age: 18
        }
    },
    actions: {
        otherGetter() {
            const otherStore = useOtherStore()
            return this.name + useOtherStore.data
        }
    }
})

```

### 6.公共方法

##### $subscribe

 state改变时触发该函数内的回调函数 

 接收两个参数:

-  args:返回store配置信息包含store 的模块 **storeId**  等等
- state : state最新的值.

```
import { useUserStore } from '@/stores/userStore'

const user = useUserStore()
// state改变时触发该回调函数
user.$subscribe((args, state) => {
    console.log(args, state);
});
```



##### $onAction

actions触发时执行该函数内的回调函数

```
import { useUserStore } from '@/stores/userStore'

const user = useUserStore()
// actions改变时触发该回调函数
user.$onAction((args) => {
    console.log(args);
});
```



### 7.持久化

```
const __piniaKey = '__PINIAKEY__'
//定义兜底变量
 
 
type Options = {
   key?:string
}
//定义入参类型
 
 
 
//将数据存在本地
const setStorage = (key: string, value: any): void => {
 
localStorage.setItem(key, JSON.stringify(value))
 
}
 
 
//存缓存中读取
const getStorage = (key: string) => {
 //这里返回的就是state的全部
return (localStorage.getItem(key) ? JSON.parse(localStorage.getItem(key) as string) : {})
 
}
 
 
//利用函数柯里化接受用户入参
const piniaPlugin = (options: Options) => {
 
//将函数返回给pinia  让pinia  调用 注入 context
return (context: PiniaPluginContext) => {
 
 //只要你注册了store模块，一旦这个模块的state发生改变就会得到当前的store,然后就能执行$subscribe
const { store } = context;
 
const data = getStorage(`${options?.key ?? __piniaKey}-${store.$id}`)
 
store.$subscribe(() => {
 
setStorage(`${options?.key ?? __piniaKey}-${store.$id}`, toRaw(store.$state));
 
})
 
//返回值覆盖pinia 原始值
/*因为这里是入口文件,只要刷新就会执行这里的代码
*不知道是不是pinia内部实现如果data是空的，就会取store的默认值。我试了把localStorge清空，取的是Store的*默认值
*/
return {
 
...data
 
}
 
}
 
}
 
 
//初始化pinia
const pinia = createPinia()
 
 
//注册pinia 插件
pinia.use(piniaPlugin({
 
key: "pinia"
 
}))
```

