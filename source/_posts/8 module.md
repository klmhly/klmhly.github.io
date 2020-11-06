ES6

`export`  

`export default`



Node

`module.exports`

`exports`



模块化规范

`CommonJs`

`AMD`

`CMD`





###  ES6模块化和Node模块化对比

1. **ES6 模块输出的是值的引用，CommonJS 模块输出的是一个值的拷贝。**
2. **ES6 模块是编译时输出接口，CommonJS 模块是运行时加载**
3. 



### ES6 两种模块化区别

 `export` 方式，

- 在使用的时候，要用具体导出的模块里的变量名来接收导入的内容
- 一个文件支持多个export

 `export default` 方式，

- 在使用的时候，不需要知道具体导出的模块里的变量名
- 一个文件仅支持一个export default





### Node模块化

重要  `module.exports = exports`

```
// test.js
function log(){
  console.log('1-----')
}


// 3种导出形式区别
module.exports = log     
module.exports = {
	log
} 
exports.log = log
```

推荐使用module.exports,  而不要单独使用exports。否则容易混淆

1. module.exports = func        

   ```
   module.exports = log
   
   // 在index.js调用模块
   const log =  require('./config/test')
   ```

2. module.exports =  { log, add, ....}

   适合将整个站点的公用函数放到一个模块里，然后整体导出，业务文件根据需要导入对应的函数

   ```
   module.exports =  { 
   	log,
   	add,
   	cookie
   }
   
   // 在index.js调用模块
   const { log } =  require('./config/test')
   ```

3. Exports 这里注意下引入的时候是对象

   ```
   exports.log = log
   
   // 在index.js调用模块
   const { log } =  require('./config/test')
   ```

   

test.js 里面打印module的结果:  可以看到 module.exports 是一个空对象

```
{
  id:
   '/Users/jiaxiaoyu/Documents/Murphy/xiaoyu_node_test/config/test.js',
  exports: {},
  parent:
   Module {
     id: '.',
     exports: {},
     parent: null,
     filename:
      '/Users/jiaxiaoyu/Documents/Murphy/xiaoyu_node_test/index.js',
     loaded: false,
     children: [ [Circular] ]  //...
  }
```



