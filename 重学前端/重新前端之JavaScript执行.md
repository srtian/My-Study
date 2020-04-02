# 重学前端之 JavaScript 执行

一个 JavaScript 引擎会常驻内存中，它等待着我们把 JavaScript 代码或者函数传递给它。<br />
<br />按照 JSC 引擎术语，我们把宿主发起的任务称为宏观任务，将 JavaScript 引擎发起的任务称为微观任务。<br />

<a name="jwV6y"></a>

## 一、宏观任务和微观任务

JavaScript 引擎等待宿主环境分配宏观任务，在操作系统中，通常等待的行为都是一个事件循环，因此在 Node 术语中，也会把这个部分称为事件循环。

每次代码的执行过程，其实就是一个宏观任务，因此我们可以理解为：宏观任务的队列相当于事件循环。而在宏观任务中，JavaScript 的 Promise 还会产生异步代码，而 JS 必须保证这些异步代码在一个宏观任务中完成，因此每个宏观任务都包含一个微观任务队列。

在有了宏观任务和微观任务机制，我们就可以实现 JS 引擎级和宿主级的任务了，例如：Promise 永远在队列尾部添加微观任务。setTimeout 等宿主 API，则会添加宏观任务
<a name="s6sDf"></a>

### 1.1 Promise

Promise 的总体思想是：需要进行 io,等待或者其他异步操作的函数，不反回真实结果，而是返回一个承诺，函数的调用方可以在合适的实际，选择等待这个承诺的兑现。
<a name="SxIJB"></a>

### 1.2 async await

async/await 是 ES2016 新加入的特性，它提供了用 for、if 等代码结构来编写异步的方式。它的运行时基础是 Promise。
<a name="pwWbV"></a>

## 二、闭包和执行上下文

<a name="XZUoO"></a>

### 2.1 闭包

闭包的英文单词是 closure,在不同领域有这不同含义：

1. 在编译原理中，它是处理语法产生式的一个步骤
1. 在计算几合中，他表示的平面点集的凸多边形
1. 在编程语言中，他表示一种函数

闭包可以简单理解为一个绑定了执行环境的函数，和普通函数的区别是，它携带了执行的环境

> 简单类比：人在外太空需要携带吸氧设备

<a name="rPCuQ"></a>

### 2.2 执行上下文

JavaScript 标准把一段代码，执行所需的所有信息定义为“执行上下文”。<br />**执行上下文在 ES3 中包括三个部分：**

- scope：作用域
- variable object: 变量对象，用于存储变量的对象
- this value: this 值

**在 ES5 中，改进了命名方式：**

- lexical environment：词法环境，当获取变量时使用
- variable environment：变量环境，当声明变量时使用
- this value：this 值

**ES2018 中，this 值被归入  lexical environment ，但增加了不少内容：**

- lexical environment：词法环境，当获取变量时使用
- variable environment：变量环境，当声明变量时使用
- code evaluation state：用于恢复代码执行位置
- Function：执行的任务是函数时使用，表示正在被执行的函数
- ScriptOrModule：执行的任务是脚本或者模块时使用，表示正在被执行的代码
- Realm: 使用的基础库和内置对象实例
- Generator：仅生成器上下文有这个实例，表示当前生成器。

<a name="GLbVL"></a>

## 三、函数的分类

```javascript
// 1.普通函数
function foo(){
    // code
}
// 2。箭头函数
const foo = () => {
    // code
}
// 3.方法：在class中定义的函数
class C {
    foo(){
        //code
    }
}
// 4.生成器函数，用function*定义的函数
function* foo(){
    // code
}
// 5. 类：用class定义的类，实际上也是函数
class Foo {
    constructor(){
        //code
    }
}
// 6，7，8 异步函数
async function foo(){
    // code
}
const foo = async () => {
    // code
}
async function foo*(){
    // code
}

```

对于普通变量来说买这些函数没有本质上的区别名都遵循“继承定义时环境”的原则，他们的一个行为差异在 this 关键字。
<a name="MhEHS"></a>

### 3.1 this

this 是 JavaScript 的关键字，是执行上下文中很重要的一部分，同一个函数抵用方式不同，得到的 this 的值也不同，简单来讲就是：调用函数时使用的引用，决定函数执行时的 this 的值。<br />
<br />另外 new 也很有意思：<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/296173/1585727679930-6a977fda-b932-477c-830d-3de9b39202d6.png#align=left&display=inline&height=226&name=image.png&originHeight=452&originWidth=462&size=39370&status=done&style=none&width=231)<br />

<a name="QuZsF"></a>

## 四、javaScript 语句

![image.png](https://cdn.nlark.com/yuque/0/2020/png/296173/1585728933377-9b738526-3b45-4509-a9bf-1545170d305d.png#align=left&display=inline&height=436&name=image.png&originHeight=872&originWidth=555&size=155551&status=done&style=none&width=277.5)
<a name="qoYvv"></a>

### 4.1 普通语句

普通语句执行后，会得到 [[type]] 为 normal 的 Completion Record，JavaScript 引擎遇到这样的 Completion Record，会继续执行下一条语句。这些语句中，只有表达式语句会产生 [[value]]，当然，从引擎控制的角度，这个 value 并没有什么用处
<a name="pTL5X"></a>

### 4.2 语句块

语句块就是拿大括号括起来的一组语句，它是一种语句的复合结构，可以嵌套
<a name="0MlYm"></a>

### 4.3 控制性语句

![image.png](https://cdn.nlark.com/yuque/0/2020/png/296173/1585731140634-686a89bf-e455-46a7-921e-e35bc7f0a221.png#align=left&display=inline&height=232&name=image.png&originHeight=463&originWidth=840&size=67554&status=done&style=none&width=420)
<a name="tsfYZ"></a>

### 4.4 带标签的语句

任何 JavaScript 语句是可以加标签的，在语句前加冒号即可<br />

```javascript
firstStatement: var i = 1;
```
