---
title: HTML5 语义化标签
date: 2019-05-03 14:16:34
tags: [Frontend, H5]
category: 'code'
---

HTML5 已经出来好久了，语义化标签也提了好久了，也许是公司产品业务的关系，工作中从来没有真正的好好使用过语义化标签。偶然计划看到*winter*的课程，才真正了解到语义化标签的意义:

> 语义类标签对开发者更为友好，使用语义类标签增强了可读性，即便是在没有 CSS 的时候，开发者也能够清晰地看出网页的结构，也更为便于团队的开发和维护。

> 除了对人类友好之外，语义类标签也十分适宜机器阅读。它的文字表现力丰富，更适合搜索引擎检索（SEO），也可以让搜索引擎爬虫更好地获取到更多有效信息，有效提升网页的搜索量，并且语义类还可以支持读屏软件，根据文章可以自动生成目录等等。

> **“用对”比“不用”好，“不用”比“用错”好。当然了，我觉得有理想的前端工程师还是应该去追求“用对”它们。**

为了更好理解，此处从[MDN 标签列表](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list)中把语义化标签实现一遍试试。

### 脚本

#### template

> 通过 JavaScript 在运行时实例化内容的容器。

标签里做为模板使用，不会渲染，可用 js 来获取后动态渲染。

[MDN 内容模板元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/template)

### 章节

#### section

> 定义文档中的一个章节。

> 表示文档中的一个区域（或节），比如，内容中的一个专题组，一般来说会有包含一个标题（heading）。一般通过是否包含一个标题 (`<h1>`-`<h6>` element) 作为子节点 来 辨识每一个`<section>`。

> 一个`<section>`应该出现在文档大纲中。

[MDN `<section>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/section)

#### nav

> 定义只包含导航链接的章节。

> 导航栏 (`<nav>`) 描绘一个含有多个超链接的区域，这个区域包含转到其他页面，或者页面内部其他部分的链接列表。

[MDN 导航栏](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/nav)

#### article

> 定义可以独立于内容其余部分的完整独立内容块。

[MDN article](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/article)

#### aside

> 定义和页面内容关联度较低的内容——如果被删除，剩下的内容仍然很合理。

> 其通常表现为侧边栏或者标注框（call-out boxes）。

[MDN aside](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/aside)

#### header

> 定义页面或章节的头部。它经常包含 logo、页面标题和导航性的目录。

[MDN header](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/header)

#### footer

> 定义页面或章节的尾部。它经常包含版权信息、法律信息链接和反馈建议用的地址。

[MDN footer](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/footer)

#### main

> 定义文档中主要或重要的内容。

> 这部分内容在文档中应当是独一无二的，不包含任何在一系列文档中重复的内容，比如侧边栏，导航栏链接，版权信息，网站 Logo，搜索框（除非搜索框为文档的主要功能）。

[MDN main](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/main)

### 组织内容

#### figure

> 代表一个和文档有关的图例。

> 这个标签经常是在主文中引用的图片，插图，表格，代码段等等，当这部分转移到附录中或者其他页面时不会影响到主体。

[MDN figure](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/figure)

#### figcaption

> 代表一个图例的说明。

> 是与其相关联的图片的说明/标题，用于描述其父节点 `<figure>` 元素里的其他数据。

[MDN figcaption](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/figcaption)

### 文字形式
