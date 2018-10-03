---
title: Hexo博客的搭建
date: 2018-10-01 20:51:50
tags: 
categories: 其他
description: 
---

### 本地部署
1. 安装NodeJs  

```
http://nodejs.cn
```
2. 安装Git  

```
https://git-scm.com/downloads
```
3. 使用npm安装Hexo

```
npm install -g hexo-cli
```
4. 进入本地一个指定文件夹，初始化

```
hexo init
```
5. 运行

```
hexo generate
hexo server
```
6. 浏览器访问 http://localhost:4000

### 部署到GitHub
1. 在GitHub上创建自己的Repository

```
项目名必须为：用户名.gubub.io，如：wangchangjing.github.io
```
2. 修改配置(根目录下的_config.yml文件)

```
deploy:
  type: git
  repo: git@github.com:wangchangjing/wangchangjing.github.io.git
  branch: master
```
3. 安装部署插件  

```
npm install hexo-deployer-git --save
```
4. 部署

```
hexo clean
hexo generate
hexo deploy
```
5. 浏览器访问 https://wangchangjing.github.io

### Hexo常用命令

```
hexo init  #生成站点
hexo new "postName"  #新建文章
hexo new page "pageName"  #新建页面
hexo clean  #清除缓存
hexo generate  #生成静态页面至public目录
hexo server  #启动本地服务
hexo deploy  #部署到GitHub
hexo help  #查看帮助
hexo version  #查看Hexo的版本
```

> 官方文档：https://hexo.io