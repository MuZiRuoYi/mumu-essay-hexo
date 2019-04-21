---
title: 构建自己的 Github Pages
date: 2018-06-03 10:39:13
tags: [GitHubPages, Hexo]
category: [GitHubPages]
---

[hexo_doc]: https://hexo.io/zh-cn/docs/index.html
[hexo_doc_git]: https://hexo.io/zh-cn/docs/deployment.html#Git
[hexo_themes]: https://hexo.io/themes/
[hexo-deploy-git]: https://github.com/hexojs/hexo-deployer-git
[github_new_profile]: https://github.com/new
[hexo_theme_indigo]: https://github.com/yscoder/hexo-theme-indigo/wiki

本人想构建自己的一个博客来做一下备忘，于是就找到了 GitHub Pages，我使用的是[Hexo](hexo_doc)。

构建过程按照其步骤走一遍就可以了，其中一些关键点进行以下记录：

### 主题

其实 Hexo 文档里有主题相关描述，但是我感觉太不直白了，看着累。

#### 获取主题

你要是不喜欢它的默认主题，可以去[查找](hexo_themes)一个自己看着顺眼的，然后点击去查看具体主题，重点来了，怎么下载并且使用这个主题呢？

进去主题后，**注意**看 page footer，基本都会有 GitHub 图标或者 by xxx，点击后一般都指向主题 GitHub 地址。

#### 替换主题

下载下来的主题文件夹，放到 Hexo 项目的 themes 文件夹里，修改`_config.yml` 里的`theme`字段，将值改为自己下载的主题**文件夹名字**（所以呢，可以自拿个现成主题改吧改吧就成自己的了，名字还可以随便取）就可以了。

### 部署到 GitHub Pages

安装[`hexo-deployer-git`][hexo-deploy-git]依赖：

    $ npm install hexo-deployer-git --save

去`_config.yml` [修改配置][hexo_doc_git]：

    deploy:
      type: git
      repo: <repository url>
      branch: [branch]
      message: [message]

所以，需要去 GitHub [新建][github_new_profile]项目（比如我创建的 muziruoyi.github.io）。

其中几个字段，`repo`就是项目 Url 地址，比如可以是`https://github.com/MuZiRuoYi/muziruoyi.github.io`，但是如果你不想每次部署都输入帐号密码，则可以填写：`git@github.com:MuZiRuoYi/muziruoyi.github.io.git`（GitHub 项目 ssh clone 地址，需要保证你 GitHub 的 SSH keys 已经设置好了）

### 其他文件管理

在`source/_post/`文件夹里添加的东西，在`hexo deploy`中，处理`.md`文件，都会按照相对路径移入`public`文件夹里。

经过测试表明，就是`.md`文件也可以按照自己的想法放到不同的文件夹里，只不过构建后的 url 里会带上文件夹名。

那么，图片、存放该项目地址的`READ.md`，直接丢到`source/_post/`文件夹里就行了。

### 替换域名

网上查了一下，替换域名是在存放 Pages 的项目里添加一个`CNAME`的文件，里边写上自己要绑定的域名就可以了，但是，但是，我这个 Pages 是 Hexo 自动构建发布的怎么办？总不能每次构建后去添加一遍吧？

仔细看看上面文件管理，说白了就是你将`CNAME`文件直接放到`source/_post/`文件夹里就行了。

### 添加评论

一切都 OK 了，但是是静态 pages，怎么让其他人评论呢？这个要根据不通主题而定，我使用的是[indigo][hexo_theme_indigo]主题，里边含有相关评论的配置，也可以[参考一下](/hexo/defined-hexo-theme/#开启评论)我是怎么做的。
