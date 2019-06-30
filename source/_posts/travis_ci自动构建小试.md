---
title: travis_ci自动构建小试
date: 2019-07-01 00:28:18
categories:
- 博客
tags: [travis-ci,博客]
---

最终目标：[Hexo 博客终极玩法：云端写作，自动部署](https://segmentfault.com/a/1190000017797561?utm_source=tag-newest)

参考资料：

1. [使用Hexo+Github+TravisCI搭建自动发布的静态博客系统](https://segmentfault.com/a/1190000013266001)
2. [用 Travis CI 自动部署 hexo](https://segmentfault.com/a/1190000004667156)
3. [使用Travis CI持续部署Hexo博客](https://www.jianshu.com/p/5691815b81b6)
4. [hexo+travis自动构建github page](https://www.jianshu.com/p/3885555f07c0)

遇到的坑:

1. ssh-keygen， 使用travis加密。确保 `.travis.yml`中`openssl aes-256-cbc -K $encrypted_xxxxxxxxxxx_key -iv $encrypted_xxxxxxxxxxx_iv`一行文件位置所在。
2. 使用ssh方式git需修改hexo博客`_config.yml`文件中的deploy部分为ssh形式：`repo: git@github.com:XXXXXX/XXXXXX.github.io.git`。
3. BlogSource库中注意查看theme中是否有git submodule, 如果存在，注意同时git到BlogSource库或删除相应git数据。

同时更新了主题[icarus](http://github.com/ppoffice/hexo-theme-icarus)到较新版本。
