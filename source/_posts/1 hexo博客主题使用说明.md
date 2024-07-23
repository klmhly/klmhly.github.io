---
title: '1 Hexo博客搭建说明'
categories: 博客搭建
tags: 
- Hexo
- Git
- Node
---

记录hexo搭建所需的环境，以及主要过程。

<!-- more -->

## 安装环境

> 需要保证电脑安装了以下环境，才能搭建和使用hexo博客

- Node      安装地址：[官网](https://nodejs.org/zh-cn/)

- Git  安装地址：[官网](https://git-scm.com/)

- Hexo
  
  1. 创建文件夹，例如BLOG（用来存放博客）
  2. 在git bash终端输入安装命令：`$ npm i -g hexo`
  3. 查看安装版本：`npm -v`

## 本地配置过程

> 确保第2步需要安装的环境均已安装，然后开始搭建博客。

- 进入博客目录初始化hexo：`hexo init`
- 清空缓存：`hexo cl`
- 编译生成：`hexo g`
- 开启服务：`hexo s`

这一步做完，可以在浏览器输入http://localhost:4000,如果可以看到页面则已经在本地搭建好了博客

## 使用github托管

- 获取本电脑的ssh： 
  `bash
  $ ssh-keygen -t rsa -C "youremail@example.com `
- 查看公开密钥：`$ cat ~/.ssh/id_rsa.pub ` 
- 在github的设置->SSH and GPG keys -> SSH keys 里面添加公开密钥
- 创建知识库：命名 [github帐号名].github.io
- Git 和 GitHub 进行认证(仅第一次)，输入 `ssh -T git@github.com`
- 安装部署依赖：`npm install hexo-deployer-git --save`
- 修改Hexo站点配置文件
  
  ```bash
  deploy:
        type: git
        repo: https://github.com/user_name/user_name.github.io.git
        branch: master
  ```
- 清除缓存：`hexo clean`
- 生成静态文件：`hexo generate`
- 启动服务器：`hexo server`
- 部署网站：`hexo deploy`

## 多终端管理

### 1.在第一台电脑博客根目录设置

- 删除 theme 子文件夹下的 .git 文件夹
- 在 Git Bash 上操作：
  
  ```
  // git 初始化
  git init
  // 添加远程仓库地址
  git remote add origin https://github.com/user_name/user_name.github.io.git
  // 新建分支并切换到该分支
  git checkout -b hexo
  // 添加所有文件
  git add .
  // 提交
  git commit -m ""
  // 推送到远程 hexo 分支
  git push origin hexo
  ```
- 在 GiHub 设置 hexo 分支为默认分支

### 2.在另一台电脑设置

- 克隆 hexo 分支到本地：git clone https://github.com/user_name/user_name.github.io.git
- 安装 Hexo：npm install -g hexo-cli
- 不需要 Hexo 初始化，否则之前的 Hexo 配置参数将会重置
- 安装依赖库：npm install
- 安装部署依赖：npm install hexo -deployer-git --save

### 3.更新文章

使用 Git Bash 在 hexo 分支下上传源文件：

- 拉取更新：git pull
- 添加文件：git add .
- 提交：git commit -m ""
- 推送：git push origin hexo

使用 hexo 生成静态文件并部署网站：

- 清除缓存：hexo clean
- 生成静态文件：hexo generate
- 部署网站：hexo deploy

## 容易遇到的问题

1. 换电脑拉取代码，重新hexo deploy， 之前的所有文章创建日期会变成当日。
   
   这是因为之前这台电脑没有记录，拉下来的被认为是本地新建
   
   
2. 执行`hexo deploy` 报错
   
   node版本和hexo版本不匹配， 这里将node从16降到12即可以
