---
title: Angular7 使用笔记
date: 2019-04-28 13:21:05
tags: [Frontend, Angular]
category: 'code'
---

前一段由于 Hexo 构建出来结果界面老是不对，就自己用 Angular7 实现了一套本站点（UI 来自于此站），实现过程中遇到许多问题，至于到最后也还没有完全复制出本站点（现在恢复正常，继续使用 Hexo 完成站点），此处记录实现中遇到问题及其解决。

### Can't resolve './app.module.ngfactory'

项目跑起来后报错：

```cmd
./src/main.ts
Module not found: Error: Can't resolve './app.module.ngfactory' in '****\mumu-essay-ng\src'
```

解决方式：

> /src/main.ts 中 module.export 改为 module.exports

### throw new Error('Cyclic dependency' + nodeRep)

`html-webpack-plugin`遇到报错`throw new Error('Cyclic dependency' + nodeRep)`。

目前解决方案可以使用 Alpha 版本:

    npm i --save-dev html-webpack-plugin@next

### 懒加载

只需将路由配置文件改为：

```ts
const routes: Routes = [{ path: 'home', loadChildren: '../home/home.module#HomeModule' }];
```

需要注意，是`home.module#HomeModule`而不是`home.module.ts#HomeModule`，之前我也是由于带了`.ts`后缀名，好久都调不出来。

[查看源码](https://github.com/MuZiRuoYi/mumu-essay-ng)
