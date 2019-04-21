---
title: 前端杂项笔记 - setTimeout
date: 2018-08-12 12:33:12
tags: [Frontend]
---

“怎么在我的界面里不停调用你的接口？”。

“不应该啊，离开时候我销毁刷接口的定时器了啊？！”

——————————————————————————————————————

我回去看看代码，这样写的：

```js
let this.timer = setTimeout(() => {
  ...
}, 3000);
```

```js
componentWillUnmount () {
  if (this.timer) {
    clearTimeout(this.timer);
  }
}
```

解帮的时候销毁了啊，难道`setTimeout`的 ID 可能判断为`false`？

然后我在浏览器打开一个新标签 console 里：

    > setTimeout(() => {});
    < 1
    > setTimeout(() => {});
    < 2

原来`setTimeout`的 ID 是数字，应该是全局累加的吧，那么可能不可能是`0`呢？

尝试了好多次都最小是`1`。

翻一下[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout)吧：

> 返回值 timeoutID 是一个正整数，表示定时器的编号。这个值可以传递给 clearTimeout()来取消该定时。
>
> 需要注意的是 setTimeout()和 setInterval()共用一个编号池，技术上，clearTimeout()和 clearInterval() 可以互换。但是，为了避免混淆，不要混用取消定时函数。
>
> 在同一个对象上（一个 window 或者 worker），setTimeout()或者 setInterval()返回的定时器编号不会重复。但是不同的对象使用独立的编号池

也并没有说明什么，那么试一下去除判断条件试试：

    > clearTimeout(2)
    < undefined
    > clearTimeout(null)
    < undefined
    > clearTimeout(undefined)
    < undefined
    > clearTimeout(0)
    < undefined

所以，虽然搞不明白`timeoutID`到底会不会是`0`，但是没有必要去特殊判断。
