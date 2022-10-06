### 1.构建项目

```
 npm init vue@latest 
```

此操作会引领我们安装最新版的vue-router

### 2.配置

src/router/index.ts

```
//引入路由对象
import { createRouter, createWebHistory, createWebHashHistory, createMemoryHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'

//vue2 mode history vue4 createWebHistory
//vue2 mode  hash  vue4  createWebHashHistory
//vue2 mode abstact vue4  createMemoryHistory
 
//路由数组的类型 RouteRecordRaw
// 定义一些路由
// 每个路由都需要映射到一个组件。
const routes: Array<RouteRecordRaw> = [{
    path: '/',
    component: () => import('../components/a.vue')
},{
    path: '/register',
    component: () => import('../components/b.vue')
}]
 
 
 
const router = createRouter({
    history: createWebHistory(),
    routes
})
 
//导出router
export default router
```



main.ts

```
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
createApp(App).use(router).mount('#app')
```

### 3.命名路由

除了 path 之外，你还可以为任何路由提供 name。这有以下优点：

- 没有硬编码的 URL
- params 的自动编码/解码。
- 防止你在 url 中出现打字错误。
- 绕过路径排序（如显示一个）

```
const routes:Array<RouteRecordRaw> = [
    {
        path:"/",
        name:"Login",
        component:()=> import('../components/login.vue')
    },
    {
        path:"/reg",
        name:"Reg",
        component:()=> import('../components/reg.vue')
    }
]
```

- router-link跳转方式需要改变 变为对象并且有对应name


    <h1></h1>
    <div>
      <router-link :to="{name:'Login'}">Login</router-link>
      <router-link style="margin-left:10px" :to="{name:'Reg'}">Reg</router-link>
    </div>
    <hr />
#### 编程式导航

除了使用 <router-link> 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。

1.字符串模式

```
import { useRouter } from 'vue-router'
const router = useRouter()

const toPage = () => {
  router.push('/reg')
}
```

2.对象模式

```
import { useRouter } from 'vue-router'
const router = useRouter()

const toPage = () => {
  router.push({
    path: '/reg'
  })
}
4
```

4.命名式路由模式

```
import { useRouter } from 'vue-router'
const router = useRouter()

const toPage = () => {
  router.push({
    name: 'Reg'
  })
}
```

4. a标签跳转
直接通过a href也可以跳转但是会刷新页面

```
 <a href="/reg">rrr</a>
```

### 4. 路由传参

#### 4.1 Query路由传参

编程式导航 使用router push 或者 replace 的时候 改为对象形式新增query 必须传入一个对象

```
const toDetail = (item: Item) => {
    router.push({
        path: '/reg',
        query: item
    })
}
```

##### 接受参数

使用 useRoute 的 query

```
import { useRoute } from 'vue-router';
const route = useRoute()
const item = route.query.item
```

#### 4.2 Params路由传参

编程式导航 使用router push 或者 replace 的时候 改为对象形式并且只能使用name，path无效，然后传入params

```
const toDetail = (item: Item) => {
    router.push({
        name: 'Reg',
        params: item
    })
}
```

##### 接受参数

使用 useRoute 的 params

```
import { useRoute } from 'vue-router';
const route = useRoute()
```

```
<div>品牌：{{ route.params?.name }}</div>
<div>价格：{{ route.params?.price }}</div>
<div>ID：{{ route.params?.id }}</div>
```



#### 4.4 动态路由传参

很多时候，我们需要将给定匹配模式的路由映射到同一个组件。例如，我们可能有一个 User 组件，它应该对所有用户进行渲染，但用户 ID 不同。在 Vue Router 中，我们可以在路径中使用一个动态字段来实现，我们称之为 路径参数 

路径参数 用冒号 : 表示。当一个路由被匹配时，它的 params 的值将在每个组件

```
const routes:Array<RouteRecordRaw> = [
    {
        path:"/",
        name:"Login",
        component:()=> import('../components/login.vue')
    },
    {
        //动态路由参数
        path:"/reg/:id",
        name:"Reg",
        component:()=> import('../components/reg.vue')
    }
]
```

```
const toDetail = (item: Item) => {
    router.push({
        name: 'Reg',
        params: {
            id: item.id
        }
    })
}
```

```
import { useRoute } from 'vue-router';
import { data } from './list.json'
const route = useRoute()

const item = data.find(v => v.id === Number(route.params.id))
```

#### 4.4 二者的区别

- query 传参配置的是 path，而 params 传参配置的是name，在 params中配置 path 无效
- query 在路由配置不需要设置参数，而 params 必须设置

- query 传递的参数会显示在地址栏中

- params传参刷新会无效，但是 query 会保存传递过来的值，刷新不变 ;

- 路由配置
  

### 5.嵌套路由

一般来说  <router-view></router-view> 会写在App.vue

嵌套路由就是在其他子组件里面写  <router-view></router-view> ,且当前子组件是App.vue的子组件,也就是说在子组件对应的路由后面再加上children对应的path.

children 里面写path不需要加 '/ '.

```
const routes: Array<RouteRecordRaw> = [
    {
        path: "/user",
        component: () => import('../components/footer.vue'),
        children: [
            {
                path: "",
                name: "Login",
                component: () => import('../components/login.vue')
            },
            {
                path: "reg",
                name: "Reg",
                component: () => import('../components/reg.vue')
            }
        ]
    },
 
]
```

```
    <div>
        <router-view></router-view>
        <div>
            <router-link to="/">login</router-link>
            <router-link to="/user/reg">reg</router-link>
        </div>
    </div>
```

### 6.命名视图

 命名视图可以在同一级（同一个组件）中展示更多的路由视图，而不是嵌套显示。 命名视图可以让一个组件中具有多个路由渲染出口，即   <router-view></router-view>.这对于一些特定的布局组件非常有用。 命名视图的概念非常类似于“具名插槽”，并且视图的默认名称也是 `default`。 

```
const routes: Array<RouteRecordRaw> = [
    {
        path: "/",
        components: {
            default: () => import('../components/layout/menu.vue'),
            header: () => import('../components/layout/header.vue'),
            content: () => import('../components/layout/content.vue'),
        }
    },
]

```

```
    <div>
        <router-view></router-view>  //default组件显示在这里
        <router-view name="header"></router-view>
        <router-view name="content"></router-view>
    </div>
```

### 7.重定向

 访问/ 重定向到 /user （地址栏显示/,内容为/user路由的内容） 

```
const routes: Array<RouteRecordRaw> = [
    {
        path:'/',
        component:()=> import('../components/root.vue'),
        redirect:'/user1',
        
        children:[
            {
                path:'/user1',
                components:{
                    default:()=> import('../components/A.vue')
                }
            },
            {
                path:'/user2',
                components:{
                    bbb:()=> import('../components/B.vue'),
                    ccc:()=> import('../components/C.vue')
                }
            }
        ]
    }
]
```

### 8.滚动行为

 使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。vue-router 可以自定义路由切换时页面如何滚动。 

 scrollBehavior 方法接收 to 和 from 路由对象。第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。 

 scrollBehavior 返回滚动位置的对象信息 ,可以保存上一次路由的滚动位置.

```
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes,
  scrollBehavior (to, from, savedPosition) {
    if(savedPosition){
      return savedPosition
    }else{
      return {
        top:0
      }
    }
  }
})
```

### 9.动态路由

我们一般使用动态路由都是后台会返回一个路由表前端通过调接口拿到后处理(后端处理路由)

```
route: [
                {
                    path: "/demo1",
                    name: "Demo1",
                    component: 'demo1.vue'
                },
                {
                    path: "/demo2",
                    name: "Demo2",
                    component: 'demo2.vue'
                },
                {
                    path: "/demo3",
                    name: "Demo3",
                    component: 'demo3.vue'
                }
            ]

```

主要使用的方法就是router.addRoute

#### 添加路由

`router.addRoute() `.只能同时注册一个新的路由. 如果新增加的路由与当前位置相匹配，就需要你用 router.push() 或 router.replace() 来手动导航，才能显示该新路由

```
router.addRoute({ path: '/about', component: About })
```

#### 删除路由

通过使用 `router.removeRoute()`

```php
router.addRoute({ path: '/about', name: 'about', component: About })// 删除路由router.removeRoute('about')
```

#### 查看现有路由

- [router.hasRoute()](https://router.vuejs.org/zh/api/#hasroute)：检查路由是否存在。
- [router.getRoutes()](https://router.vuejs.org/zh/api/#getroutes)：获取一个包含所有路由记录的数组。

#### 例子

```
const initRouter = async () => {
    const result = await axios.get('http://localhost:9999/login', { params: formInline });
    result.data.route.forEach((v: any) => {
        router.addRoute({
            path: v.path,
            name: v.name,
                                    //这儿不能使用@
            component: () => import(`../views/${v.component}`)
        })
        router.push('/index')
    })
    console.log(router.getRoutes());
 
}
```

后端

```
import express, { Express, Request, Response } from 'express'
 
const app: Express = express()
 
app.get('/login', (req: Request, res: Response) => {
    res.header("Access-Control-Allow-Origin", "*");
    if (req.query.user == 'admin' && req.query.password == '123456') {
        res.json({
            route: [
                {
                    path: "/demo1",
                    name: "Demo1",
                    component: 'demo1.vue'
                },
                {
                    path: "/demo2",
                    name: "Demo2",
                    component: 'demo2.vue'
                },
                {
                    path: "/demo3",
                    name: "Demo3",
                    component: 'demo3.vue'
                }
            ]
        })
    }else{
        res.json({
            code:400,
            mesage:"账号密码错误"
        })
    }
})
 
app.listen(9999, () => {
    console.log('http://localhost:9999');
 
})
```

