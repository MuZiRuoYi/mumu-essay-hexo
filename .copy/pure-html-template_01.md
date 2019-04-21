---
title: 纯前端导出HTML探索
date: 2018-04-13 00:40:16
tags: [html-template, html-report]
---

[baidu_template]: http://baidufe.github.io/BaiduTemplate/
[ali_template]: https://aui.github.io/art-template/zh-cn/docs/

### 写在前面

问题描述：

1.  前端使用 SPA 框架进行开发，并且与后端完全分离，仅通过`Ajax`进行通信
2.  现需要导出报表，报表形式要求是 HTML

方案整理：

1.  前端做报表模板（数据部分使用占位符），由服务端绑定数据并提供下载接口
2.  前端做报表模板（数据部分使用占位符），从服务端获取`json`数据，`js`模板引擎进行数据替换，模拟`a`链接下载

为了方（zhuang）便（bi），选择方案 2 进行处理。

### 思路整理

整理思路如下图：

![方案整理](/img/pure_html_export_thinking.svg)

所以，现在主要问题是使用工具来快速构建 HTML 模板，然后项目中可以为 HTML 模板占位符填充数据。

！！！注意：

1.  构建模板过程中需要预览导出的 HTML，意味着构建模板过程中需要能够使用模板和模拟的数据进行预览
2.  项目中需要能够将模板和服务端数据进行占位符填充数据，再模拟`a`链接导出
3.  导出单个 HTML：意味着构建工具需要把 HTML/CSS/js/img 必须打包到一起

以上可以得出构建工具和项目的平衡点 —— 通过共同的 js 模板引擎对占位符进行处理。

### 方案实现

#### 模板引擎

为了尽量在项目前端性能问题，需要找一个**尽量精简、能够在浏览器和 node 运行**的模板引擎，因此 node 常用的一些大型模板引擎首先排除。

##### 基本要求

1.  占位符一般数据替换
2.  支持循环占位符（列表、表格等需要`for`等循环逻辑）
3.  支持判断占位符（即能在模板中使用`if...else`）

##### 引擎选择

理想引擎是从模板中读出的占位符，按照编译原理进行编译，生成可执行函数：Input String -Lexer-> Tokens -AST Builder-> Abstract Syntax Tree -AST Compile-> Expression function

但是找了好久找不到理想引擎，退而求其次，能够**解析目标并且安全**的 js 模板引擎。

这次找了[阿里][ali_template]/[百度][baidu_template]/腾讯等各家的 js 模板引擎，实现代码简单，能够解析目标，做了编码达到防止 XSS，基本能够满足需求，以下以`baiduTemplate`为例（哪一个都不复杂）。

#### 模拟下载

##### 获取 HTML 模板

##### 模板引擎替换数据

##### 模拟下载

#### HTMl 模板开发工具构建

##### 启动服务

##### 模板引擎处理模板和模拟数据

##### 打包工具进行打包

由于需要将所以资源打包到一起，所以需要以下步骤：

1.  css 中 img 引用需要替换为 base64 数据
2.  HTML 中引入的 css 全部合并到`<style></style>`标签里
3.  HTML 中引入的 js 全部合并到和 HTML 中`<script></script>`标签里
