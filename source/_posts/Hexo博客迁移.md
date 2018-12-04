---
title: Hexo博客迁移
date: 2018-12-04 17:46:14
tags:
- Hexo
categories: 迁移备份
description: Hexo博客系统更换电脑时迁移备份
---
### 前言
Hexo是一个简洁、高效的静态博客框架，而Hexo框架使用是基于当前电脑的安装和配置，本文讲述的是如果我们有一天更换电脑或者重装系统时应该如何优雅的迁移博客系统。


### 说明
这里默认您已成功使用Hexo和GitHub发布博客，所以使用GitHub进行存储备份博客系统文件，当然有其他更好的存储方式也可以替换。  
具体思路：利用GitHub的存储库特性，使用GitHub存储Hexo博客系统文件，这样我们不管在那台电脑上都可以随时更新发布博客。


## 操作
创建一个新GitHub存储库，当然也可以直接使用博客发布的静态库生成分支。
``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### 第二步

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### 第三步

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### 第四步

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)

![blogIcon](http://image.12c3.com/blogIcon.png)
