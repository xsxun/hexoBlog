---
title: Hexo更换工作环境后的解决方法
date: 2018-07-23 12:59:43
tags: [hexo,博客]
---
1. 将原有工作目录中的以下文件复制到新的工作环境：
    + _config.yml
    + package.json
    + scaffolds/
    + source/
    + themes/
2. 在新的环境下配置好`Node.js`环境，并安装`hexo`
    + `npm install -g hexo`
3. 在新的工作环境中，安装hexo相关模块：
    + `npm install`
    + `npm install hexo-deployer-git --save`
    + `npm install hexo-generator-feed --save`
    + `npm install hexo-generator-sitemap --save`
4. 部署hexo：
    + `hexo g`
    + `hexo s`
    + `hexo deploy`