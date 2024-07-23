---
title: 8 vue2源码目录结构解析
date: 2024-07-23 21:30:32
updated: 2024-07-23 21:30:32
tags: vue2 源码
---

vue2源码学习之从目录结构开始
<!-- more -->

## 源码目录结构设计

其中核心代码在src目录下, src下有6层目录

```js
src
├── compiler        # 编译相关 
├── core            # 核心代码 
├── platforms       # 不同平台的支持
├── server          # 服务端渲染
├── sfc             # .vue 文件解析
├── shared          # 共享代码
```

```js
├─ src                         // 主要源码所在位置，核心内容
│   ├─ compiler                // 模板编译相关文件，将 template 编译为 render 函数
│       ├─ codegen             // 把AST(抽象语法树)转换为Render函数
│       ├─ directives          // 生成Render函数之前需要处理的东西
│       ├─ parser              // 模板编译成AST
│   ├─ core                    // Vue核心代码，包括了内置组件、全局API封装、Vue实例化、响应式原理、vdom(虚拟DOM)、工具函数等等。
│       ├─ components          // 组件相关属性，包含抽象出来的通用组件 如：Keep-Alive
│       ├─ global-api          // Vue全局API，如Vue.use(),Vue.nextTick(),Vue.config()等，包含给Vue构造函数挂载全局方法(静态方法)或属性的代码。 链接：https://012-cn.vuejs.org/api/global-api.html
│       ├─ instance            // 实例化相关内容，生命周期、事件等，包含Vue构造函数设计相关的代码
│       ├─ observer            // 响应式核心目录，双向数据绑定相关文件。包含数据观测的核心代码
│       ├─ util                // 工具方法
│       └─ vdom                // 虚拟DOM相关的代码，包含虚拟DOM创建(creation)和打补丁(patching)的代码
│   ├─ platforms               // vue.js和平台构建有关的内容 不同平台的不同构建的入口文件也在这里 （Vue.js 是一个跨平台的MVVM框架）
│       ├── web                // web端 （渲染，编译，运行时等，包括部分服务端渲染）
│       │   ├── compiler       // web端编译相关代码，用来编译模版成render函数basic.js
│       │   ├── entry-compiler.js               // vue-template-compiler 包的入口文件
│       │   ├── entry-runtime-with-compiler.js  // 独立构建版本的入口，它在 entry-runtime 的基础上添加了模板(template)到render函数的编译器
│       │   ├── entry-runtime.js                // 运行时构建的入口，不包含模板(template)到render函数的编译器，所以不支持 `template` 选项，我们使用vue默认导出的就是这个运行时的版本。
│       │   ├── entry-server-basic-renderer.js  // 输出 packages/vue-server-renderer/basic.js 文件
│       │   ├── entry-server-renderer.js        // vue-server-renderer 包的入口文件
│       │   ├── runtime        // web端运行时相关代码，用于创建Vue实例等
│       │   ├── server         // 服务端渲染（ssr）
│       │   └── util           // 工具类相关内容
│       └─ weex                // 混合运用 weex框架 (一端开发，三端运行: Android、iOS 和 Web 应用) 2016年9月3日~4日 尤雨溪正式宣布以技术顾问的身份加盟阿里巴巴Weex团队， 做Vue和Weex的整合 让Vue的语法能跨三端
│   ├─ server                  // 服务端渲染相关内容（ssr）
│   ├─ sfc                     // 转换单文件组件（*.vue）
│   └─ shared                  // 共享代码 全局共享的方法和常量
```

**compiler:源码编译相关：**

- 将template编译为render函数
  
- 将AST转化成Render
  
- 将模板解析成ast语法树
  
- ast语法树优化
  

**core目录包含了vue的核心代码**

是Vue.js的灵魂， 需要重点分析的地方

- 内置组件
  
- 全局api封装
  
- vue实例化
  
- 观察者
  
- 虚拟dom
  
- 工具函数
  

**platform是vue的入口**

- 2个目录2个入口， 分别打包成运行在web和weex上的vue.js

**server**: 所有服务端渲染的相关逻辑

**sfc**

通常我们开发vue.js 都会借助webpack构建， 然后通过.vue文件来编写组件。

这个目录下的代码逻辑会把.vue文件内容解析成一个js对象

**shared**

被浏览器端的vue.js 和 服务端vue.js 共享的工具方法

## 编译入口怎么找到

1. 从package的启动命令scripts里， 一步步追溯到入口文件
  
  ![](../images/2024-06-17-13-06-01-image.png?msec=1721738747744)
  
2. 进入config文件看
  
  ![](../images/2024-06-17-13-06-47-image.png?msec=1721738747740)
  
3. entry指向的路径，就是入口文件， resolve上面定义了， 就是拼接前面的目录，定位出入口文件就是platforms下面， 这里也包含了不同类型的入口文件
  
  ![](../images/2024-06-17-13-08-03-image.png?msec=1721738747740)
  

## Runtime Only 对比 Runtime+Compiler

- Runtime Only
  
  我们在项目里使用vue时， 通常会借助**webpack的vue-loader工具把.vue文件编译成js**，这个过程是在编译阶段做的。 所以实际我们代码引入的vue只包含运行时的Vue.js代码
  
- Runtime + Compiler
  
  **如果没有对代码进行预编译， 但是又使用了Vue的template属性传入字符串**， 则需要在客户端编译模板。 编译过程发生在运行时， 所以需要带有编译器的版本。
  
  ```js
  // 需要编译器的版本
  new Vue({
    template: '<div>{{ hi }}</div>'
  })
  
  // 这种情况不需要
  new Vue({
    render (h) {
      return h('div', this.hi)
    }
  })
  ```
  

## Vue入口找么找到

我们经常在项目里使用Vue， 会导入 import Vue from 'vue'，那么我们导入的Vue是什么， 在源码怎么找到它呢

1. 上一节找到编译入口是 src\platforms\web\entry-runtime-with-compiler.js
  
2. 然后在runtime/index里面找到下一层
  
  ![](../images/2024-06-17-16-47-45-image.png?msec=1721738747744)
  
3. 在core/index, 发现实际上就是一个用 Function 实现的类，我们只能通过 `new Vue` 去实例化它
