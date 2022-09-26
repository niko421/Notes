1.setup语法糖简介

直接在script标签中添加setup属性就可以直接使用setup语法糖了。 使用setup语法糖后，

- 不用写setup函数；

- 组件只需要引入不需要注册, 可以直接在template模板中使用。

- 属性和方法也不需要再返回,可以直接在template模板中使用。

- <script setup> 中可以使用await, 结果代码会被编译成  `async setup( )`

```xml
<template>
    <myComponent @click="handleClick" :num="num"></myComponent>
</template>
<script lang="ts" setup>
    import {ref} from 'vue';
    import myComponent from '@/component/myComponent.vue';
    //此时注册的变量或方法可以直接在template中使用而不需要导出
    let num = ref(0);
    const handleClick = ()=>{
        num.value++;
    }
</script>
```



## 2.setup语法糖中新增的api

- defineProps：子组件接收父组件中传来的props
- defineEmits：子组件调用父组件中的方法
- defineExpose：子组件暴露属性，可以在父组件中拿到

#### 2.1 defineProps （ 父传子 ）

defineProps ({ })  里面要用对象包裹住父组件传来的变量. 采用ts约束时也是用interface 包裹住变量,

```
<template>
{{obj}}
</template>
<script setup lang="ts">

interface IObj {
    name: string;
    age: number;
    list: any[],
}

interface data{
    obj:IObj
}


defineProps<data>(
   
)
</script>

```

父组件代码

```xml
<template>
    <myComponent @click="handleClick" :num="num"></myComponent>
</template>
<script lang="ts" setup>
    import {ref} from 'vue';
    import myComponent from '@/components/myComponent.vue';
    let num = ref(0);
    const handleClick = ()=>{
        num.value++;
    }
</script>
```

子组件代码

```xml
<template>
    <div>{{num}}</div>
</template>
<script lang="ts" setup>
    import {defineProps,toRefs} from 'vue';
    const props = defineProps({
        num:{
            type:Number,
            default:0
        }
    })
    const {num} =toRefs(props)
</script>
```

默认值

```
interface data {
    obj?: IObj
}
interface IObj {
    name: string;
    age: number;
    list: any[],
}
const props = withDefaults(defineProps<data>(), {
    obj: () => {
        return {
            name: "78",
            age: 89,
            list: []
        }
    }
})
```



#### 2.2 defineEmits（ 子传父 ）

子组件代码

```xml
<template>
    <div>{{num}}</div>
    <button @click="onClickButton">数值加1</button>
</template>
<script lang="ts" setup>
    import {defineProps,defineEmits} from 'vue';
    defineProps({
        num:{
            type:Number,
            default:0
        }
    })
    const emit = defineEmits(['addNumb']);
    const onClickButton = ()=>{
        // emit(父组件中的自定义方法,参数一,参数二,...)
        emit("addNumb");
    }
</script>
```

父组件代码

```xml
<template>
    <myComponent @addNumb="handleClick" :num="num"></myComponent>
</template>
<script lang="ts" setup>
    import {ref} from 'vue';
    import myComponent from '@/components/myComponent.vue';
    let num = ref(0);
    const handleClick = () => {
        num.value++;
    }
</script>
```

#### 2.3 defineExpose （ 父获取子属性 ）

子组件代码

```xml
<template>
    <div>子组件中的值{{num}}</div>
    <button @click="onClickButton">数值加1</button>
</template>
<script lang="ts" setup>
    import {ref,defineExpose} from 'vue';
    let num = ref(0);
    const onClickButton = () => {
        num.value++;    
    }
    // 使用defineExpose暴露子组件内的的属性
    defineExpose({
        num
    })
</script>
```

父组件代码

```xml
<template>
    <myComponent ref="myComponent"></myComponent>
    <button @click="onClickButton">获取子组件中暴露的值</button>
</template>
<script lang="ts" setup>
    import {ref} from 'vue';
    import myComponent from '@/components/myComponent.vue';
    // 获取 myComponent 组件 DOM对象，myComponent 和 ref属性的值保持一致
    const myComponent = ref();
    const onClickButton = () => {
        // 获取子组件暴露的属性
        console.log(myComponent.value.num)  // 0
    }
    // 注意：在生命周期中使用或事件中使用都可以获取到值，
    // 但在setup中立即使用为undefined，因为setup 类似于vue2的 created 生命周期
    console.log(myComponent.value.numb)  // undefined
    const init = ()=>{
        console.log(myComponent.value.num)  // undefined
    }
    init()
    // vue3 onMounted 等价于 vue2 mounted
    onMounted(()=>{
        console.log(myComponent.value.num)  //0
    })
</script>
```



## 3.Ref

-  Ref TS对应的接口 

  ```
  interface Ref<T> {
    value: T
  }
  ```

- 使用TS约束ref

  ```
  import { ref, Ref,isRef } from 'vue'
  let message: Ref<string | number> = ref("我是message")
  ```



## 4.customRef

- customRef用来自定义ref 

- customRef 是个工厂函数要求我们返回一个对象 并且实现 get 和 set 适合去做防抖之类的

  ```
  <template>
   
    <div ref="div">小满Ref</div>
    <hr>
    <div>
      {{ name }}
    </div>
    <hr>
    <button @click="change">修改 customRef</button>
   
  </template>
   
  <script setup lang='ts'>
  import { ref, reactive, onMounted, shallowRef, customRef } from 'vue'
   
  function myRef<T = any>(value: T) {
    let timer:any;
    return customRef((track, trigger) => {
      return {
        get() {
          track()
          return value
        },
        set(newVal) {
          clearTimeout(timer)
          timer =  setTimeout(() => {
            console.log('触发了set')
            value = newVal
            trigger()
          },500)
        }
      }
    })
  }
   
   
  const name = myRef<string>('小满')
   
   
  const change = () => {
    name.value = '大满'
  }
   
  </script>
  <style scoped>
  </style>
  ```



## 5.可选链操作符

 vue里无论在模板或者JS代码处书写可选链，均可以使用。 

通过连接的对象的引用或函数可能是 `undefined` 或 `null` 时，可选链运算符提供了一种方法来简化被连接对象的值访问。

比如，思考一个存在嵌套结构的对象 `obj`。不使用可选链的话，查找一个深度嵌套的子属性时，需要验证之间的引用，例如：

```
let nestedProp = obj.first && obj.first.second;
```

为了避免报错，在访问`obj.first.second`之前，要保证 `obj.first` 的值既不是 `null`，也不是 `undefined`。如果只是直接访问 `obj.first.second`，而不对 `obj.first` 进行校验，则有可能抛出错误。

有了可选链运算符（`?.`），在访问 `obj.first.second` 之前，不再需要明确地校验 `obj.first` 的状态，再并用短路计算获取最终结果：

```
let nestedProp = obj.first?.second;
```

通过使用 `?.` 运算符取代 `.` 运算符，JavaScript 会在尝试访问 `obj.first.second` 之前，先隐式地检查并确定 `obj.first` 既不是 `null` 也不是 `undefined`。如果`obj.first` 是 `null` 或者 `undefined`，表达式将会短路计算直接返回 `undefined`。

  

#### 空值合并运算符

可以在使用可选链时设置一个默认值：

**只有当  ??  左边返回`undefined` 或 `null` 时,才会取默认值.**

```
let customer = {
  name: "Carl",
  details: { age: 82 }
};
let customerCity = customer?.city ?? "暗之城";
console.log(customerCity); // “暗之城”
```





## 6.异步组件&代码分包&suspense

####  defineAsyncComponent

- 父组件引用子组件 通过defineAsyncComponent加载异步配合import 函数模式便可以分包 

  ```
  import { defineAsyncComponent } from 'vue'
  const AsyncCategory = defineAsyncComponent(() => import("./AsyncCategory.vue"))
  ```



#### suspense

- Suspense是一个内置的全局组件，该组件有两个插槽：

- default:如果default可以显示，那么显示default的内容；

- fallback:如果default无法显示，那么会显示fallback插槽的内容；

  ```
  <template>
   
     <Suspense>
      <template #default>
         
              <DataJson></DataJson>
         
      </template>
      
      <template #fallback>
         
              <div>loading...</div>
         
      </template>
     </Suspense>
     
  </template>
  ```





## 7.**内置组件keep-alive**

-   KeepAlive 包裹动态组件时，会缓存不活跃的组件实例，而不是销毁它们。 
- 任何时候都只能有一个活跃组件实例作为 KeepAlive的直接子节点。

- 开启keep-alive 生命周期的变化
  - 初次进入时： onMounted> onActivated
  - 退出后触发 deactivated
  - 再次进入：
    - 只会触发 onActivated
    - 事件挂载的方法等,只执行一次的放在 onMounted中;组件每次进去执行的方法放在 onActivated中.
      

- 与 `v-if` / `v-else` 分支一起使用时，同一时间只能有一个组件被渲染：

  ```
  <KeepAlive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </KeepAlive>
  ```

## 8.Vue3自动引入插件

- https://github.com/antfu/unplugin-auto-import

-  修改tsconfig.json 

- ```
  {
    "include": [
      "src/**/*.ts",
      "src/**/*.d.ts",
      "src/**/*.tsx",
      "src/**/*.vue",
      "./auto-imports.d.ts" // 引入到这里
    ]
  }
  ```

  

## 9.定义全局函数和变量

  由于Vue3 没有Prototype 属性 使用 **app.config.globalPropertie**s 代替 然后去定义变量和函数 

```
const app = createApp(App)
app.config.globalProperties.$http = () => {}
app.mount('#app')
```



#### 过滤器

在Vue3 移除了过滤器,可以使用全局函数代替Filters

```
app.config.globalProperties.$filters = {
  format<T extends any>(str: T): string {
    return `$${str}`
  }
}
```

 声明文件 不然TS无法正确类型 推导 

```
type Filter = {
  format: <T extends any>(str: T) => T
}
// 声明要扩充@vue/runtime-core包的声明.
// 这里扩充"ComponentCustomProperties"接口, 因为他是vue3中实例的属性的类型.
declare module '@vue/runtime-core' {
  export interface ComponentCustomProperties {
    $filters: Filter
  }
}
```

组件使用

```
<template>
  {{$filters.format(89)}}
</template>
```



## 10.自定义插件

Loading.Vue

```
<template>
    <div v-if="isShow" class="loading">
        <div class="loading-content">Loading...</div>
    </div>
</template>
    
<script setup lang='ts'>
import { ref } from 'vue';
const isShow = ref(false)//定位loading 的开关
 
const show = () => {
    isShow.value = true
}
const hide = () => {
    isShow.value = false
}
//对外暴露 当前组件的属性和方法
defineExpose({
    isShow,
    show,
    hide
})
</script>
 
 
    
<style scoped lang="less">
.loading {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    &-content {
        font-size: 30px;
        color: #fff;
    }
}
</style>
```



Loading.ts

```
import {  createVNode, render, VNode, App } from 'vue';
import Loading from './Loading.vue'
 
export default {
    install(app: App) {
        //createVNode vue提供的底层方法 可以给我们组件创建一个虚拟DOM 也就是Vnode
        const vnode: VNode = createVNode(Loading)
        //render 把我们的Vnode 生成真实DOM 并且挂载到指定节点
        render(vnode, document.body)
        // Vue 提供的全局配置 可以自定义
        
        //这里的app 类似main.ts里面的create(App)
        app.config.globalProperties.$loading = {
            show: () => vnode.component?.exposed?.show(),
            hide: () => vnode.component?.exposed?.hide()
        }
        
    }
}
```



Main.ts

```typescript
import Loading from './components/loading.ts'
//注意这里引入的是ts文件不是vue文件
 
let app = createApp(App)
 
app.use(Loading)
 
 
type Lod = {
    show: () => void,
    hide: () => void
}
//编写ts loading 声明文件放置报错 和 智能提示
declare module '@vue/runtime-core' {
    export interface ComponentCustomProperties {
        $loading: Lod
    }
}
 
 
 
app.mount('#app')
```



组件setup使用

```
<script setup lang="ts">
import { ComponentInternalInstance } from 'vue'

const app = getCurrentInstance() as ComponentInternalInstance
//初次调用
app.appContext.config.globalProperties.$loading.show();
setTimeout(() => {
  app.appContext.config.globalProperties.$loading.hide();
}, 5000)


</script>
```

 

#### 手写app.use( )

```
import type { App } from 'vue'
import { app } from './main'  //获取create(App)
 
interface Use {
    install: (app: App, ...options: any[]) => void
}
 
const installedList = new Set()
 
export function MyUse<T extends Use>(plugin: T, ...options: any[]) {
    if(installedList.has(plugin)){
      return console.warn('重复添加插件',plugin)
    }else{
        plugin.install(app, ...options)
        installedList.add(plugin)
    }
}
```



## 11.Event Loop 

#### JS 执行机制

-  js 是单线程的 , 单线程就意味着所有的任务都需要排队，后面的任务需要等前面的任务执行完才能执行，如果前面的任务耗时过长，后面的任务就需要一直等，一些从用户角度上不需要等待的任务就会一直等待，这个从体验角度上来讲是不可接受的，所以`JS`中就出现了异步的概念。 

#### 同步任务

代码从上到下按顺序执行

#### 异步任务

##### 1.宏任务

- script(整体代码)、setTimeout、setInterval、UI交互事件、postMessage、Ajax

##### 2.微任务

- Promise.then catch finally、MutaionObserver、process.nextTick(Node.js 环境)

#### 运行机制

- 所有的同步任务都是在主进程执行的形成一个执行栈，同步任务执行完,再执行异步任务 .主线程之外，还存在一个"任务队列"，异步任务执行队列中先执行宏任务，然后清空当次宏任务中的所有微任务，然后进行下一个tick如此形成循环。
  

## 12.nextTick

-  `nextTick` 就是创建一个异步任务，那么它自然要等到同步任务执行完成后才执行。 

```
<template>
   <div ref="xiaoman">
      {{ text }}
   </div>
   <button @click="change">change div</button>
</template>
   
<script setup lang='ts'>
import { ref,nextTick } from 'vue';
 
const text = ref('妹妹在这呢')
const xiaoman = ref<HTMLElement>()
 
const change = async () => {
   text.value = '妹妹该走了'
   console.log(xiaoman.value?.innerText) //妹妹在这呢
   await nextTick();
   console.log(xiaoman.value?.innerText) //妹妹该走了
}
</script>
 
```

- 由于vue 更新dom是异步的 ,如果此时在函数修改了响应式数据, 则需要等函数执行完成,dom才会去更新.

  nextTick 会等待函数执行完成,才会去执行下面的代码.这样就避免了dom 还没来得及更新就去获取最新的dom.

## 13.unocss原子化

## 14.函数式编程

## 15.性能优化

https://blog.csdn.net/qq1195566313/article/details/126811832?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166417859016782414913437%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=166417859016782414913437&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_ecpm_v1~rank_v31_ecpm-2-126811832-null-null.nonecase&utm_term=%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96&spm=1018.2226.3001.4450

##### 减小打包体积

比如从cdn引入vue

webpack.config.js配置

```
 externals: {
        vue: "Vue" //CDN 引入 打包时忽略vue这个包
    },
```

最后要在index.html 引入cdn 

 <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

