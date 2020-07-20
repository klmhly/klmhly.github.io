---
title: '【Bug】动态引入组件的打包报错问题'
categories: 技术
tags: 
- Webpack
- 动态引入
---

记一个本地运行正常，打包后报错的问题解决。
<!-- more -->

## 背景
在项目里需要动态引入一个组件，在componentDidMount事件中
```
import('xxx').then((x) => {
  // 传入组件配置
})
```

引入组件后，本地运行，可以正常使用，然后进行打包，发到测试环境。

浏览器报一些 函数未定义等的bug。

### 解决方案
S1,  webpeck.prod.config 的mode改为 development; 
			
S2, 安装这个插件，版本要在2.2.0以上， "babel-plugin-dynamic-import-node": "^2.2.0"
		
S3, 在webpack里面配置对应的babel-loader的设置

```
module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules|bower_components/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['env', 'stage-0', 'react'],
            plugins: [
              ['dynamic-import-node', { 'noInterop': true }]    //webpack设置
            ]
          }
        }
      },
      ...
    ]
}
```


目前觉得这个修改了mode值，是不太好的做法。还需要后续继续跟进更深的原因
暂时先这样记录一下了。