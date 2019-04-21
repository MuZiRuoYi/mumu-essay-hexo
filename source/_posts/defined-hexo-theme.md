---
title: 自定义 Hexo 主题
date: 2018-06-03 13:37:53
tags: [GitHubPages, Hexo]
category: 'hexo'
---

[hexo_doc_themes]: https://hexo.io/zh-cn/docs/themes.html
[hexo_doc_variables]: https://hexo.io/zh-cn/docs/variables.html
[hexo_doc_helper]: https://hexo.io/zh-cn/docs/helpers.html
[mine_hexo_theme]: https://github.com/MuZiRuoYi/hexo-theme-indigo
[indigo_url]: https://github.com/yscoder/hexo-theme-indigo
[build_mine_github_pages]: /GitHubPages/build-mine-githubpages/#其他文件管理
[gitment]: https://github.com/imsun/gitment
[gitment_doc]: https://imsun.net/posts/gitment-introduction/
[oauth_apps]: https://github.com/settings/developers

<!-- [hexo_theme_indigo]: https://github.com/yscoder/hexo-theme-indigo/wiki -->

与其说自定义主题，不如过怎么改吧其他人的主题，找到了现在用的这款主题，但是有些东西不满意，就像自己改吧改吧。

如果想真的自己开发主题，请查看[官方文档][hexo_doc_themes]，我的身上的艺术细菌数量不足以让我去尝试。

### 写在前面

我使用的主题也同样可以在 page footer 找到原主题[indigo](indigo_url)，我改后的主题在[这里][mine_hexo_theme]，当然，这个自定义化太强，仅适用于我自己了。

#### 开启其他导航

先说一下怎么开启`标签（tags）`和`分类（categories）`吧。

命令行进入项目文件夹，输入：

    $ hexo new page categories
    $ hexo new page tags

`/source/_post/`文件夹里会出现`categories`和`tags`文件夹，并且文件夹里只有一个`index.md`，分别在文件标题部分添加对应的一行：

```md
layout: tags
```

```md
layout: categories
```

然后`/themes/<theme name>/_config.yml`里对`menu`设置一下就行了，然后每篇文章的标题部分添加`tags`和`category`，以下是我的某篇文档题目：

```md
---
title: '探索ReasonML_02 - 什么是ReasonML'
date: 2018-03-31 01:11:08
tags: [ReasonML]
category: 'ReasonML'
---
```

#### 开启评论

[indigo](indigo_url)支持添加评论，只需要设置`/themes/<theme name>/_config.yml`中评论部分就行了，其支持多种评论，这里仅介绍使用 [gitment][gitment] (具体这是什么可以查看其[中文文档][gitment_doc]）添加评论的方式。

首先需要去 GitHub 新建一个项目，使用其 issues 存储评论。

然后去[OAuth Apps][oauth_apps]，点击 OAuth Apps，其中 Homepage URL（网站域名） 和 Authorization callback URL（一般是评论界面对应的域名） 需要注意，比如我的填写的都是 https://www.muziruoyi.com。

之后会生成 Client ID 和 Client Secret，这两个需要配置到`/themes/<theme name>/_config.yml`中`gitment`部分。

以下是我的配置：

```yml
gitment:
  owner: MuZiRuoYi
  repo: comments.github.io
  client_id: 879*********58
  client_secret: 3ce4**************************54be4
```

owner：GitHub 帐号
repo：使用其 issues 的项目名
client_id/client_secret 是 OAuth Apps 生成出来的。

##### 注意

1.  Authorization callback URL 填写错误会造成不能评论
2.  每篇文章出来后，不能直接添加评论，必须先使用自己的 GitHub 帐号登录评论区，会有一个**Initialize Comments**按钮，点击初始化评论区，然后其他人就可以使用 GitHub 帐号登录评论了

### 自己的主题

由于我自己是对[indigo][indigo_url]的修改，所以我第一件事：关闭主题里`_config.yml`中的`cdn`。

本人为了学习 ReasonML，并且英语水平不咋地，就把学习过程做了一个翻译，那么，这些相关的我都想用一个 menu 分类怎么办？

#### 添加 menu

`/themes/<theme name>/_config.yml`里修改配置。

`menu`:

```yml
menu:
  ···
  reasonml:
    text: ReasonML
    url: /reasonml
```

页面标题：

```yml
# 页面标题
···
reasonml_title: ReasonML
```

#### 新建`/reasonml`路由界面

`hexo server`后查看结果，报错：

    Cannot GET /reasonml/

因为没有这个路由嘛，命令行输入：

    $ hexo new page reasonml

发现与添加 tags 和 categories 相同套路，可是`reasonml/index.md`里是不能添加`layout: reasonml`的，因为很明显默认没有`reasonml`这种布局方式。

然后随便在这个文件里写点东西吧。

重跑`hexo server`后进入`/reasonml`，发现不报错了，并且刚才在`reasonml/index.md`写的东西出来了。

可以看一下[构建自己的 Github Pages][build_mine_github_pages]，可以知道，`/source/_post/`里的子文件、文件夹在`generate`的时候，会按照相对路径跑到`public`文件夹里，所以访问`/reasonml/`路由相当于访问`public`（里边是构建结果）下`/reasonml/index.html`，所以以上操作也就顺理成章了。

#### 自己的布局

当`reasonml/index.md`里写东西多了就会发现，这个界面和一般的文章界面一样，反正我是有点淡淡的嫌弃。

然后，跟着已经有的`categories`和`tags`布局抄吧，比如`tags`：

进入`/themes/<theme name>/layout/`文件夹，里边有`tag.ejs`和`tags.ejs`，看一下内容，肯定是`tags.ejs`是我需要模仿的对象。

##### 新建布局与使用

首先，`reasonml/index.md`题头添加布局标识`layout: reasonml`，`/themes/<theme name>/layout/`文件夹下新建`reasonml.ejs`。

然后编辑`reasonml.ejs`，先拿`tags.ejs`一些东西看看效果：

```ejs
<%- partial('_partial/header', {
    title: locals.title || theme.tags_title,
    hdClass: 'tags-header'
}) %>
```

布局标签：

```html
<div class="container body-wrap fade">
  ···
</div>
```

这两部分很容易看得出来是干啥的，但是跑完后发现`/reasonml`界面空了。

##### 添加内容

去`tags.ejs`界面，发现它使用了`is_tag()`，`partial()`方法，`page.posts`，`site.tags`变量，这个都是什么呢？我们先去官方文档上看看[变量][hexo_doc_variables]和[方法][hexo_doc_helper]都什么意思。

一看文档，我简直发现了新大陆，但是还是不那么直接，于是，我就在`reasonml.ejs`里添加了一些打印函数：

```html
<div class="container body-wrap fade">
  <% console.log(page) %>
  <% console.log(site) %>
</div>
```

重新`hexo server`，发现`page`和`site`变量被打印到了命令行里（比文档直观了好些有木有），我忽然发现，我在`reasonml/index.md`写的东西跑到了`page.content`里，然后我就直接试试：

```html
<div class="container body-wrap fade">
  <%- page.content %>
</div>
```

结果发现首页成功了，并且比原来好看多了。

### 写在后面

其实改动也是浪费了好久的时间，刚开始的时候没有想到官方文档，也没想到打印，一直根据`tags.ejs`和`categories.ejs`几个文件改来改去，总是达不到效果，这里只不过记录了一个最终正确的道路，看来以后需要习惯首先找找别人的文档了。
