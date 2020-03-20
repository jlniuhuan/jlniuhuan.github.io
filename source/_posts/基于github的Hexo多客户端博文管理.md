---
title: 基于github的Hexo多客户端博文管理
body:
  - article
  - comments
categories: [教程]
tags: [github, hexo]
date: 2020-02-29 0‏‎2:23:10
---

基于github的Hexo多客户端博文管理

<!-- more -->

<meta name="referrer" content="no-referrer"/>

## 1. 文件环境准备

​	准备github建立好的hexo分支，并在合适的文件夹pull下hexo分支的文件

## 2. 配置当前客户端开发环境

	1. 安装Node.js环境
	2. 安装Git工具环境

## 3. 配置当前客户端Hexo环境

```bash
#安装Hexo
npm install hexo-cli -g
npm install hexo-deployer-git --save
#新建一个空的文件夹hexo
mkdir hexo
cd hexo
#在新的空文件夹内进行初始化Hexo
hexo init
#安装volantis主题必需插件
npm i -S hexo-generator-search hexo-generator-json-content
#2.0.0以前主题安装hexo-renderer-less
npm install hexo-renderer-less --save
#2.0.0以后主题安装hexo-renderer-stylus
npm install hexo-renderer-stylus --save
#安装其他所需插件
npm i --save hexo-wordcount #字数统计插件
npm i -S hexo-helper-qrcode #二维码支持插件
npm i -S hexo-related-popular-posts #相关文章支持插件
npm install -g cnpm --registry=https://registry.npm.taobao.org
npm install hexo-asset-image --save #博文插入图片插件，需hexo下_config.yml设置psot_asset_folder:true
npm i -S leancloud-storage #leancloud评论插件
#如果出现提示"run `npm audit fix` to fix them, or `npm audit` for details"，直接运行"npm audit fix"即可
#查看网站初始效果，登录 http://localhost:4000/ 可以查看
hexo generate  
hexo server
#发表博客
hexo g -d

```

