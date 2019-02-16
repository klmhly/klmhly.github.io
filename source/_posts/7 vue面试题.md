---
title: '7 vue面试题'
categories: 
- 面试
tags: 
- VUE
---
> 可能的面试题
<!-- more --> 

### 1. 谈谈对vue的认识
特点：
- Vue是一套构建用户界面的渐进式框架,也可以理解为是一个视图模板引擎,强调的是状态到界面的映射。
- 数据驱动
- 组件化开发
- 聚焦于视图层，容易学习，适合单页应用
- MVVM类型的框架

组成部件：
- 声明式渲染 Render
- 组件系统 Component
- 客户端路由 Router
- 大规模状态管理 Vuex
- 构建工具 Build System

Vue的核心的功能，是一个视图模板引擎，但这不是说Vue就不能成为一个框架。如下图所示，这里包含了Vue的所有部件，在声明式渲染（视图模板引擎）的基础上，我们可以通过添加组件系统、客户端路由、大规模状态管理来构建一个完整的框架。更重要的是，这些功能相互独立，你可以在核心功能的基础上任意选用其他的部件，不一定要全部整合在一起。

可以看到，所说的“渐进式”，其实就是Vue的使用方式，同时也体现了Vue的设计的理念。

> 声明式渲染的基础上，组件系统 -客户端路由 -大规模状态管理的构建工具。

### 2. MVVM的理解
> MVVM 是 Model-View-ViewModel 的缩写。

- Model代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑。
- View 代表UI 组件，它负责将数据模型转化成UI 展现出来。
- ViewModel 监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步View 和 Model的对象，连接Model和View。

在MVVM架构下，View 和 Model 之间并没有直接的联系，而是通过ViewModel进行交互，Model 和 ViewModel 之间的交互是双向的， 因此View 数据的变化会同步到Model中，而Model 数据的变化也会立即反应到View 上。
ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。


### 3. vue实现双向数据绑定的原理
- 数据劫持
- 发布者-订阅者模式
- Object.defineProperty()

vue实现数据双向绑定主要是：采用**数据劫持**结合**发布者-订阅者模式**的方式，通过**Object.defineProperty（）**来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应监听回调。当把一个普通 Javascript 对象传给 Vue 实例来作为它的 data 选项时，Vue 将遍历它的属性，用 Object.defineProperty 将它们转为 getter/setter。用户看不到 getter/setter，但是在内部它们让 Vue 追踪依赖，在属性被访问和修改时通知变化。

vue的数据双向绑定 将MVVM作为数据绑定的入口，整合Observer，Compile和Watcher三者，通过Observer来监听自己的model的数据变化，通过Compile来解析编译模板指令（vue中是用来解析 {{}}），最终利用watcher搭起observer和Compile之间的通信桥梁，达到数据变化 —>视图更新；视图交互变化（input）—>数据model变更双向绑定效果。

js实现简单的双向绑定
```html
<body>
    <div id="app">
    <input type="text" id="txt">
    <p id="show"></p>
</div>
</body>
<script type="text/javascript">
    var obj = {}
    Object.defineProperty(obj, 'txt', {
        get: function () {
            return obj
        },
        set: function (newValue) {
            document.getElementById('txt').value = newValue
            document.getElementById('show').innerHTML = newValue
        }
    })
    document.addEventListener('keyup', function (e) {
        obj.txt = e.target.value
    })
</script>
```

### 4. 生命周期的理解
### 5. vue模版编译过程
>关于 Vue 编译原理这块的整体逻辑主要分三个部分，也可以说是分三步，这三个部分是有前后关系的：
- 解析器：第一步是将 模板字符串 转换成 element ASTs（解析器）
- 优化器：第二步是对 AST 进行静态节点标记，主要用来做虚拟DOM的渲染优化（优化器）
- 生成器：第三步是 使用 element ASTs 生成 render 函数代码字符串（代码生成器）

