---
title: 虚拟 DOM 和模拟事件
date: 2019-04-28 22:48:23
tags: [Frontend, VirtualDom, SimulatedEvent]
category: 'code'
---

许多情况下，我们肯能并不方便使用一个实际节点去做一些事情，比如需要根据服务端返回数据来确定是打开一个新界面还是弹出一个弹出框，此时并不是用户实际点击 DOM 节点触发的新界面打开，但是如果使用模拟的`a`标签，同时使用模拟点击事件触发点击效果，很方便就可以解决这个问题。

### 虚拟 DOM 并触发点击

**模拟 DOM 并触发事件**

```js
export function triggerHtmlTagMouseEvent({ tag, eventName, attr = {}, actionName, action }) {
  const dom = document.createElement(tag);
  const event = document.createEvent('MouseEvents');

  action && actionName && dom.addEventListener(actionName, action);
  event.initEvent(eventName, true, true, document.defaultView);

  Object.keys(attr).forEach(name => {
    dom[name] = attr[name];
  });

  dom.dispatchEvent(event);
}
```

**参数讲解**

`tag`: 标签名，如`a`，`input`等；
`eventName`：想要模拟触发的事件名，如`click`，`change`等；
`attr`：虚拟 DOM 的属性，如`input`的`type`属性，`a`的`href`、`target`等；
`actionName`：想要额外监听的事件类型，如`click`，`change`等；
`action`：`function`，代码可以看到，`doc.addEventListener`触发的事件，即想要额外监听的事件。

**代码解析**

此方法是通用方法，不仅能够模拟`<input type="file" >`，而是针对所有 DOM 节点，比如`a`链接，`from`等。

1. 创建虚拟 DOM 与模拟事件；
2. 事件是否需要添加额外事件监听，如果需要则添加（后面`<input type="file">`的模拟会用到）；
3. 初始化事件；
4. 为虚拟 DOM 添加所需要的属性；
5. 触发虚拟事件。

### 使用

**模拟`a`标签**

```js
export function triggerAnchorClick(attr = {}, actionName, action) {
  triggerHtmlTagMouseEvent({ tag: 'a', eventName: 'click', attr, actionName, action });
}
```

**使用**

```js
triggerAnchorClick({ href: 'https://www.baidu.com', target: '_black' }, 'click', e => {
  console.log(e);
});
```

### 此外

可能此处会发现`actionName`和`action`参数并没有实际作用，但是如果是`<input type="file" >`，触发`click`事件，再监听`change`事件来获取用户选择的文件，就变得很有用了。

此外还可以扩展出其他标签相关使用，此处不再多记录。
