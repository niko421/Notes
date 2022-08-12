# 课程目标
- nodejs 的基本使用
- commonjs 模块化规范
- npm 的基本使用


# nodejs 的基本使用
## nodejs 简介
> nodejs 是一个软件，安装该软件后，可以为JavaScript提供一个执行环境，说白了 nodejs 软件安装后，会在命令行提供一个 node 的命令，使用该命令可以解析执行JavaScript代码。

## nodejs 安装
- http://nodejs.cn/download/

![](.nodjs基础_images/81f5da3b.png)


整个安装全程下一步，前往不要该安装的目录，不然到时候会出很多稀奇古怪的问题。

成功后，会在命令行提供一个 `node` 和 `npm` 的命令

![](.nodjs基础_images/58bed8e9.png)

## nodejs 可以做什么？
- 为JavaScript提供一个解析执行的环境，一般叫做宿主环境，常见的浏览器也是JavaScript的一个宿主环境。
    - 如果JavaScript是在浏览器解析执行的，我们称之为JavaScript的前端开发。
    - 如果JavaScript是在nodejs这个宿主环境里面解析执行，一般我们叫做后端开发，主要是原因是nodejs会为开发提供很多的核心api，这些核心api可以做web服务器的开发。
    - 核心api是nodejs软件安装后自带了

### nodejs 解析执行JavaScript代码
- 语法：
> node 待执行的JavaScript文件

![](.nodjs基础_images/829e8c03.png)

待执行的JavaScript文件里面可以写哪些语法的代码？

- 之前学习的 JavaScript的 ECMAScript 语法全部可以使用

- 使用 nodejs 本身提供的核心的 api
    - http://nodejs.cn/api/
    - https://www.nodeapp.cn/http.html

- 使用 npm 管理的第三方的包
    - https://www.npmjs.com/

###  nodejs 核心模块 http 模块
> nodejs 里面的模块都是遵循 commonjs 模块化规范进行开发，如果要使用核心的模块，则必须先学习 commonjs 模块化规范，然后在使用。。。


# nodejs 的 commonjs 模块化规范

## 没有模块化的时代？
> JavaScript设计之初是没有模块化的概念的。最先在写JavaScript的代码的时候，都是写在全局的，如果写在全局，有啥弊端没有？

- 命名冲突
- 单个文件过大，后期的维护成本很高
    - 文件根据功能做拆分，形成单个独立文件，每个文件提供最小化的功能
    - 根据这种以大拆小的思想，我们就可以把这些根据功能拆分出来的文件叫做模块
    - 提供一种规范可以拆分模块，同时还要组织模块
    - 这种组织模块的方式我们叫做叫做模块化规范
    
如何解决呢？

- 使用 匿名函数自执行形成一个函数作用域（局部）


匿名函数自执行不是最好的办法？

- let 解决



## 什么是模块化？
> 将功能代码拆分成单个独立的文件，该文件里面的成员（变量、函数、类）都是私有的（这个成员在自己的文件使用），彼此之间（多个文件，模块）不会相互影响，这种叫做模块化。

## 为什么要使用模块化？
- 方便维护和管理

## 常见的模块化规范有哪些？
- AMD 
    - require.js
- CMD
    - sea.js
    
- es6 模块化 （export、export default、import {name} from './a.js'）
    - https://es6.ruanyifeng.com/

- commonjs 模块化    
    
## commonjs 模块化规范
- 一个文件就是一个模块，模块里面的成员都是私有的，彼此之间相互独立，不会影响
- 如果要要把当前这个模块里面的成员给到其他的模块使用，则必须使用 `exports` 或者 `module.exports` 进行导出，实际导出的是 `module.exports` 这个对象，如果两种导出方式一起使用的话，则以 `module.exports` 为准
- 如果要使用其他模块里面的成员，我们必须先使用 `require` 进行导入

# nodejs 的核心模块
- path
- fs
- http

## path 负责路径的处理模块
- join
- resolve

## fs 负责和操作系统的 文件系统打交道
- fs file system 文件系统
- fs 可以实现文件的 读取，追加，删除等操作

## http 可以搭建web服务器
- http.createServer()

### express
- https://www.expressjs.com.cn/

> 基于 Node.js 平台，快速、开放、极简的 Web 开发框架。 express 是基于 http 模块，封装的一个 web 服务器 js 库。


如果要使用 express 去做开发，则我们需要去了解另外一个工具，叫做 npm。

# npm 简介
## npm 是什么？
> node package manager，nodejs 的包管理器。nodejs 本身提供的核心包的能力也是有限的，这个时候很多的互联网爱好者，遵循 commonjs 规范为 nodejs 提供很多的代码（造轮子），一般把这些遵循 commonjs 规范写代码一般叫做包（package），当开发者写好这些包后，官方那边就把这些包收集后放置在 npmjs.org 网站上面，然后提供一个专门的工具进行包的管理（1. 包的下载 2. 包的升级 3. 包的删除），这个包管理器就叫做 `npm`。


## npm 如何使用？
- 是随着 nodejs 自带的，当我们安装 nodejs 软件后，自带一个 npm 命令

![](.nodejs基础_images/c3ead25e.png)

常见的命令

- 初始化命令，在一个项目里面只需要执行一次，执行完毕后，生成package.json 文件，该文件用于记录项目中的包的信息，例如项目通过 npm 安装了多少个包，以及包的版本信息。
    - npm init -y

- 安装包的命令，当执行该命令后，会去 npmjs.org 上面去查找安装的包，然后下载到本地的 node_modules 目录下，并且还会包下载的包的信息，同步到 package.json 文件
    - npm install packageName 或者 npm i packageName

- 删除包的命令，先去删除 node_modules 目录下的对应的包，同时同步删除 package.json 里面的信息
    - npm uninstall packageName

## npm 实操

- 需求1：希望可以把时间戳转换为 2022-8-1115:26:33
    - 解决：moment  dayjs 
    - https://tool.lu/timestamp/
    - 1660202911 -- 2022-08-11 15:28:31
    
- 需求2：希望可以把一个整数转化为千分符显示
    - numeral
    - https://ncov.dxy.cn/ncovh5/view/pneumonia
    - https://blog.csdn.net/weixin_34197488/article/details/88917800
    - http://numeraljs.com/
    - 12345678 ---- 12,345,678
    ![](.nodejs基础_images/19ddadb9.png)


## npm 注意实现
### npm 切源

npm 下载包的时候都是去国外下载，速度非常的慢，一般我们建议还是在国内去下载，这个专业的术语叫做切源。把下载的源头切换到国内的源头（国内的网站一般叫做镜像网站）

常见淘宝会做这样的镜像网站，说白了就是把 npmjs.org 网站里面的包 同步的打包到国内的服务器提供服务。

- https://npmmirror.com/

- 切换国内镜像，使用淘宝镜像

```shell

npm config set registry https://registry.npm.taobao.org

```

当我们成功切源后，npm 的命令不用改变


### cnpm 的使用 尽量不要使用

cnpm 是淘宝提供的一个命令，可以来替代 npm 命令做包管理。

如果要使用 cnpm，则必须以超级管理员的身份开启黑窗口，在安装

![](.nodejs基础_images/af49ebd4.png)

```shell

 npm install -g cnpm --registry=https://registry.npmmirror.com
 

```

- `-g` 是 `--global` 的简写，代表是全局安装，安装后，会在dos命令下提供 `cnpm` 的命令。一般把这个叫做全局安装。

使用的时候，只需要把原先的 `npm` 命令换行为 `cnpm`，例如
```shell

npm i moment

```
使用 `cnpm` 替换 `npm`
```
cnpm i moment

```

cnpm 不建议使用

### yarn 包管理器
> yarn 是新一代的包管理器，旨在取代 npm。yarn 是 facebook 开发出来的一个包管理器。

使用基本和 npm 类似，只是内部的管理思想比 npm 高级，高效。下载的速度也快很多，现在更多的是使用 yarn 做 npm 包的管理。

如果要使用 yarn，必须先全局安装 yarn。

```shell
npm i -g yarn

```
yarn 的安装得先使用 npm 进行安装，安装后，会在全局提供 yarn 的命令

![](.nodejs基础_images/671c960c.png)


#### yarn 常见的命令 推荐使用

- 初始化命令
    - yarn init -y

- 安装包的命令
    - 本地项目安装
        - yarn add packageName
        
    - 全局安装
    - yarn global add packageName

- 卸载包
    - yarn remove packageName


### npm 的协同开发

#### package.json 依赖包的下载
![](.nodejs基础_images/ead210e1.png)

#### 命令执行
![](.nodejs基础_images/7457086a.png)




# express
- https://www.expressjs.com.cn/

> 基于 Node.js 平台，快速、开放、极简的 Web 开发框架。 express 是基于 http 模块，封装的一个 web 服务器 js 库。

如果要使用 express 去做开发，则我们需要去了解另外一个工具，叫做 npm。

## ssr 渲染

## api 开发

## bsr 和 ssr 各有什么优缺点？
- seo 搜索引擎的优化
    - ssr 主要是为了更好的被搜索引擎收录网站
    
- 服务器的压力
    - bsr 客户端渲染，渲染器没有渲染操作

# art-template
- art-template 是前端的一个模板引擎，该包的作用是用于把数据插入到指定的模板页面里面去一般是用于后端开发做数据的插入。

- 由于这个数据的填充是在后端完成的，后端返回的内容就是已经填充好数据的网页，一般我们把这种渲染数据的模式叫做 ssr(server side render) 服务器端渲染。

- 我们之前使用的 ajax 这种更新数据是在浏览器端获取数据，dom操作插入到页面的，这种方式 bsr(brower side render) 客户端渲染，也可以叫做浏览器端渲染。

