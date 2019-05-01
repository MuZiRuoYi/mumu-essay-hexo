---
title: React 非受控节点
date: 2019-04-30 13:32:34
tags: [Frontend, React]
category: 'code'
---

[form_data_cdn]: https://developer.mozilla.org/zh-CN/docs/Web/API/FormData

此处代码片段都是从完整代码（使用另外一个角度进行的封装，不仅仅此文描述的一个点）中摘出来的，所以此文就没有完整的可运行代码块了，仅记录思路及其一些相关的关键代码。

### 发现背景

工作中有两次牵扯到了非受控组件：

1. 基于`mobx`，使用`<input type="text">`的时候，`value`绑定了一个未提前在`observe`对象上声明过的属性，使用中浏览器会报警告（其实这个问题及其解决方案我也记忆的不是特别清晰了，有大概印象，有时间再次复现后再进行修改，如果忘了就只能如此咯）；

2. 当使用`<input type="file">`时候，发现不能通过数据控制来清空输入框已选择文件；

### 查阅资料

去 React 官网查看后发现关于非受控组件有相关说明：

> 非受控制组件的表单元素在 React 之外工作。当用户将数据输入到表单域（输入框，下拉菜单等）时，不需要 React 做任何事情，更新的数据就会被呈现出来。这也意味着你不能强迫表单域都有一个确定的值。

[原文档地址 1](https://react.docschina.org/docs/glossary.html#%E5%8F%97%E6%8E%A7--%E9%9D%9E%E5%8F%97%E6%8E%A7%E7%BB%84%E4%BB%B6-controlled-vs-uncontrolled-components)

> 在 HTML 中，`<input type="file">` 可以让用户从其设备存储中选择一个或多个文件上传到服务器，或通过 [File API](https://developer.mozilla.org/en-US/docs/Web/API/File/Using_files_from_web_applications) 进行操作。

[原文档地址 2](https://react.docschina.org/docs/uncontrolled-components.html#___gatsby)

文档中特殊提到了`<input type="file">`，发现其需要通过调用 DOM 节点 API 来处理，但是我又感觉这些这样操作并不算方便，然后就想到虚拟 DOM，每次点击生成一个虚拟 DOM，**通过虚拟 DOM 和模拟事件来实现点击效果，再通过`FormData`存储、更新、删除文件数据**。

### 解决方案

回顾上面两个问题，第一个问题很好解决，使用前生命一些，此处需要注意数据类型的正确性，提前声明为`undefined`或`null`同样可能碰到问题。

至于第二个问题，想要做到主要分两步，第一步是虚拟 DOM 并且触发点击效果，第二步根据不同操作存储文件数据。

#### 虚拟 DOM 和模拟事件

关于 DOM 触发点击效果，具体可以在[虚拟 DOM 和模拟事件](/code/frontend/virtual-dom_event/)中查看，已经有现成方法，所以模拟`<input type="file">`可以写成：

```js
export function triggerInputClick (attr, action) {
  triggerHtmlTagMouseEvent({ tag: 'input', actionName: 'change', action, attr });
},
```

只需要在需要时候调用该方法，当用户选择完文件后，就可以从`action`的参数`event`中拿到文件。

#### FormData 存储文件

[`FormData`][form_data_cdn]可以通过`Ajax`对`multipart/form-data`类型数据进行传输，我更多用于模拟`form`表单，此处就不多记录。对于[`FormData`][form_data_cdn]API 我就不讲述，可以自行查阅，此处直接记录使用：

```jsx
class FormDataForInput extends React.Component {
  formData = new FormData();

  constructor(props) {
    super(props);
    this.state = { filename: '' };
  }

  deleteFile(name) {
    this.formData.delete(name);
  }

  saveFile(name, file) {
    this.formData.set(name, file);
    this.setState({ filename: file.name });
  }

  handleTriggerInputFile() {
    const name = 'file';

    triggerInputClick({ type: 'file', id: field, accept, name }, e => this.saveFile(name, e.target.files[0]));
  }

  render() {
    return (
      <input type="text" value={filename} placeholder="请选择文件" onClick={this.handleTriggerInputFile} readOnly />
    );
  }
}
```

以上代码是根据思路拼凑出来的代码，没有验证其是否可以正常运行。

通过一个真实的`<input type="text">`的节点来模拟界面，当然也可以使用`div`，`a`等来展示，此处只是顺手而为的写法；会发现每次点击都会有一个新的虚拟 DOM 来获取新的文件，还可以随时覆盖、删除已有的文件，变得**“完全可控”**，彻底摆脱了`<input type="file">`不可控节点的问题。

### 此外

此处可以发现，不仅可以通过 [`FormData`][form_data_cdn] 存储文件，还可以存储整个`from`表单的数据，此处不再是难点，就不多记录了。
