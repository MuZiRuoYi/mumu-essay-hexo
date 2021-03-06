---
title: 你不知道的 JavaScript（上卷）
date: 2019-08-24 14:30:05
tags: [Frontend, JavaScript]
category: 'code'
---

最近一直在看《你不知道的 JavaScript（上卷）》，看完后做了一下总结，对 js 有了不一样的一些理解。

### 作用域

作用域又可以叫做*词法作用域*，这里就可编译有关系咯，“词法”就是指编译中的“词法分析”阶段。

对于所有的语言都需要先编译，后才可以执行，比如 C#、Java，就会有明显的编译过程（至于他们编译的不同与 Java 的跨平台可以自行了解），js 是“边解释边执行”，其实“解释”就是编译过程，一般这个是在执行前几十毫秒前甚至更短时间内进行。

“词法作用域”就是由于作用域是在这个编译阶段确定的，也就是说是在执行前就已经确定的，是“静态的”，只与变量所处的物理位置相关。

所以也会很明显的明白为什么**声明提升**，因为任何作用域内变量都是编译时候声明的，和执行根本不在同一个阶段进行。

### this

`this`又一种说法是指向*执行上下文*，它是执行实行确定的，其实是与调用栈相关的，可以通过浏览器开发者工具打断点时候查看调用栈。

而书中进行了几种总结，并且说明了指向的优先级：`new`绑定（`new`一个函数）> 显示绑定 > 隐式绑定 > 默认绑定。

#### `new`绑定

所有函数都可以被`new`调用，也就是所有函数都可以当做所谓的“构造函数”，所以使用内置的数据类型`new Array()`，`new Object()`的时候，发现`Array`、`Object`也只是一些内置函数。`new`调用时候会进行以下四步：

1. 创建一个全新对象；
2. 新对象被执行`[[Proptotype]]`链接；
3. 新对象绑定到函数调用的`this`；
4. 如果函数没有返回其他对象，`new`表达式中函数调用会自动返回这个新对象。

第三步会发生`this`绑定。

#### 显示绑定

这个就是我们常用的`bind`、`apply`、`call`显示绑定了。需要注意的是，`apply`、`call`是调用时候绑定；`bind`是硬绑定，不调用，同时还可以算一种柯里化（绑定时候传递一些参数，调用时候再传递剩下的）。

#### 隐式绑定

指向调用者。需要注意有时候可能会丢失，比如：

```js
var obj = {
  name: 'obj',
  print: function() {
    console.log(this.name);
  }
};

// 第一种
var print = obj.print;
print();

// 第二种
function callFunction(fun) {
  // ...
  fun();
}
callFunction(obj.print);
```

两种都需要注意的是`this`是执行阶段的事情，执行时候其实已经丢失了调用者，尤其第二种一定需要注意传参有一个赋值过程，把实参赋值给形参，和第一种发生了一样的事情。

#### 默认绑定

当都不符合以上情况时候，就是默认绑定，指向全局（浏览器执行`window`，nodejs 环境指向`global`），严格模式指向`undefined`。

### 类与对象

人们总总喜欢在 js 里模拟面向对象开发，其实再模拟也仅仅是表面看起来像而已，实际本质去完全不一回事。

先说一下类吧，类，实际上可以理解为是声明了一组数据结构及其处理这个数据结构的数据的方法，继承就是基于以上的进一步扩展；实例化就是按照这组规则的一组数据，并且可以调用类的方法来处理这组数据。

对于 js 而言，只有指针或者说引用。`new`一个实例，上面已经说明了过程；至于继承，就是原型链的指向改变。是否发现，每次实例化和继承，都是创建一个对象存储数据（实际是函数做的事情），然后继承根本不是扩展了规则，而是添加了指向所谓“父对象”的指针。

仔细想想，其实两种内在完全不相同，所以我们最好还是不要通过一些面向对象语言的东西理解 js。

### 总结：

以上只是对一些自认为关键点进行详细的说明，如果想仔细了解还是去找一下书看一看比较好，以下是我看完全书后整理出来的一些关键知识点。

![你不知道的JavaScript（上卷）总结](/img/you_don't_know_js_1.png '你不知道的JavaScript（上卷）总结')
