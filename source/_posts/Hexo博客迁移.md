---
title: Hexo博客迁移
tags:
  - hexo迁移备份
categories: Hexo
abbrlink: b97fcdf6
date: 2018-12-04 17:46:14
---
##### 前言
Hexo是一个简洁、高效的静态博客框架，而Hexo框架使用是基于当前电脑的安装和配置，本文讲述的是如果我们有一天更换电脑或者重装系统时应该如何优雅的迁移博客系统。


##### 说明
这里默认您已成功使用Hexo和GitHub发布博客，所以使用GitHub进行存储备份博客系统文件，当然有其他更好的存储方式也可以替换。  
具体思路：利用GitHub的存储库特性，使用GitHub存储Hexo博客系统文件，这样我们不管在那台电脑上都可以随时更新发布博客。


##### 操作
创建一个新GitHub存储库，然后clone到本地进行操作，当然也可以直接使用博客发布的静态库生成新的分支。
``` bash
$ git clone https://github.com/12c3/MyBlog.git
```

添加Hexo博客文件到GitHub仓库，这里要注意一点，需要忽略一些指定文件。这里需要说明一下，因为博主改过一些源生的文件所以这里是连带主题和插件这些一起备份，以求达到无缝迁移备份效果，也就是下载下来就可以直接使用不做任何改动。  
因为我们这里做的是备份博客源文件，所以像动态生成的文件这些都是需要忽略的。如根目录下.deploy_git、public生成的静态html文件，以及db.json、.gitignore文件，处理完根目录文件之后，我们还需要来到主题下面删除不必要的文件，需要注意的是，对于git配置文件一定要删除，不然会造成一些文件提交不上去的问题。

修改前Hexo博客文件列表：
![Hexo博客文件列表](http://image.12c3.com/Hexo-Blog/Hexo博客迁移/Hexo博客文件列表.png)

修改后Hexo博客文件列表：
![Hexo博客文件列表](http://image.12c3.com/Hexo-Blog/Hexo博客迁移/忽略后Hexo博客文件列表.png)

修改前主题文件列表：
![Hexo博客文件列表](http://image.12c3.com/Hexo-Blog/Hexo博客迁移/主题文件列表.png)

修改后主题文件列表：
![Hexo博客文件列表](http://image.12c3.com/Hexo-Blog/Hexo博客迁移/忽略后主题文件列表.png)

把修改后的所有文件上传到我们新建的GitHub存储库，由于文件比较多所以提交的时候需要稍微等待一会。
``` bash
$ git add .
$ git commit -m 'MyBlog init'
$ git push origin master
```
为避免我们提交不必要的文件，添加GitHub过滤规则文件.gitignore具体规则内容如下:

``` bash
#静态文件过滤
public/
#推送文件过滤
.deploy*/
#数据日志过滤
db.json
*.log
.DS_Store
Thumbs.db
```
上传到GitHub存储库之后，我们的迁移备份基本就完成了，在新环境使用的时只需在安装好Hexo之后把备份clone下来即可运行，注意不需要hexo init,直接在clone的文件下运行即可。  
