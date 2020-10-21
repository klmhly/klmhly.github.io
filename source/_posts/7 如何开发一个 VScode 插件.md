---
title: '如何开发一个 VScode 插件'
categories: vscode
tags: 
- node
- vscode 
- tech
---

记录如何开发一个 VS code 插件的过程
<!-- more -->

## 目录

安装环境与常用命令

开发一个简单插件

常用API

参考文献




## 安装环境

##### 环境是进行开发的前置条件

1. 安装 `yeoman` 脚手架工具：`npm i yo -g`

2. 安装 VS Code 代码生成器：`npm i generator-code -g`

3. 安装打包工具 `vsce`：`npm i vsce -g`

   

##### 创建发布账号

1. 创建 Microsoft 账号：https://login.live.com/     

2. 通过 Microsoft 账号注册 Azure 账号：https://aka.ms/SignupAzureDevOps

3. 创建个人令牌：在Azure 导航栏右上角菜单创建 **个人访问令牌（Personal Access Tokens）** 

   - `Organization`要选择`all accessible organizations`
   - `Scopes`要选择`Full access`，否则后面发布会失败
   - 创建令牌后需要本地记下来，网站不会进行保存

4. 创建发布账号，在命令行输入`vsce create-publisher your-publisher-name`

   - your-publisher-name: 必须是数字、字母、下划线，全网唯一

   - 会依次要求输入邮箱、令牌

     

##### 开发和发布命令

1. 创建项目脚手架

   ```
   yo code
   ```

   脚手架包含清晰的插件开发目录和简单示例

   

2. 本地打包

   ```
   vsce package
   ```

   打包之后会生成 `.vsix` 类型的包， 此时可以在vscode里面选择安装`.vsix` 包进行本地验证，如果验证ok，即可发布到插件市场，供其他人使用。（需要修改过readme的内容）

   

3. 发布命令

   ```
   vsce publish
   ```

   将插件发布到插件市场，如果已发布的版本，有修改，需要修改版本号之后再发布，发布完成，则可以在





## 开发一个简单插件

##### 首先，安装脚手架

```
yo code     // 目录结构如下图
```

![yo 安装的脚手架开发插件](http://jiaxiaoyu.cn/vscode_extention_dic)

##### 主要文件：

- Extension.js 是项目的入口文件

  ```javascript
  // vscode模块包含了 
  const { exec } = require('child_process');
  const vscode = require('vscode');
  
  /**
   * @param {vscode.ExtensionContext} context
   * 仅插件被激活时调用一次
   */
  function activate(context) {
  	// 注册命令:helloWorld,  这个就是插件的功能，弹出一个提示框
  	let disposable = vscode.commands.registerCommand('directory.helloWorld', function () {
  		vscode.window.setStatusBarMessage('你好，前端艺术家！');
  		vscode.window.showInformationMessage('拍了拍你的肱二头肌，并询问是否要打开博客？', '是', '否', '不再提示').then(result => {
  			if(result === '是') {
  				exec(`open https://klmhly.github.io`)
  			} else {
  				vscode.window.showInformationMessage('祝你有个美好的工作日！')
  			}
  		})
  	})
  
  	context.subscriptions.push(disposable);
  }
  
  
  exports.activate = activate;
  function deactivate() {}
  
  module.exports = {
  	activate,
  	deactivate
  }
  ```

  

- Package.json定义相关配置

  ```javascript
  // 扩展的激活事件
  "activationEvents": [
    "onCommand:directory.helloWorld"
  ],
    
  //入口文件
  "main": "./extension.js",     
    
  // 在这里定义一系列插件命令
  "contributes": {
    
    	// 命令触发事件
      "commands": [
        {
          "command": "directory.helloWorld",
          "title": "Hello World"
        }
      ],
      
      // 键盘触发事件
      "keybindings":{
           "command": "directory.helloWorld",
           "key": "ctrl+f1",
           "mac": "cmd+f1",
           "when": "editorTextFocus"
       },
         
       // 菜单触发事件
       "menus":{
           "editor/context": [
             {
               "when": "editorFocus",
               "command": "directory.helloWorld",
               "group": "navigation"
             }
          ]
      }
  }
  ```

- 修改README.md(只需简单描述内容即可)，就可以进行本地打包了

- `vsce package` 生成 directory-0.0.2.vsix 是本地构建生成的可安装文件

- 到此，即完成一个插件的开发

- `vsce publish` 将插件发布到vs 插件应用市场，则可以正式安装使用了



##### 开发总结

- 主要配置在`package.json`
- 对`package.json`里面定义的事件，进行注册，处理函数即可实现功能



## 常用API

#### 通知和状态栏类

#### 修改当前激活编辑器内容















## 参考

- https://juejin.im/entry/6844903640826642440

- https://www.cnblogs.com/liuxianan/p/vscode-plugin-overview.html

- https://github.com/LiangJunrong/all-for-one/tree/master/005-VS%20Code%20%E6%8F%92%E4%BB%B6

