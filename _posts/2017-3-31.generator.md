---
layout: default
title: Generator 函数
---

# Generator函数
### 基本概念
Generator函数是ES6提供的一种异步编程解决方案，语法行为与传统函数完全不同。
从语法上，可以把Generator函数理解成一个**状态机**，封装了多个内部状态。
执行Generator函数会返回一个遍历对象，也就是说Generator还是一个**遍历对象生成函数**。返回的遍历对象，可以依次遍历Generator函数内部的每一个状态。
在形式上，Generator函数是一个普通函数，但是有两个特征。一是，**function** 关键字与函数名之间有一个星号；二是，函数体内部使用**yield**语句，定义不同的内部状态( **yield** 在英语里的意思就是“产出”)。

```js
function* helloWorldGenerator() {
    yield 'hello';
    yield 'world';
    return 'ending';
}

var hw = helloWorldGenerator();
```
该函数有三个状态：hello、world和return语句(结束执行)
调用Generator函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是 **遍历器对象(Iterator Object)**。
下一步，必须调用遍历器对象的next方法，使得指针移向下一个状态。也就是说，每次调用 **next** 方法，内部的指针就从函数头部或者上一次停下来的地方开始执行，直到遇到下一个 **yield** 语句(或**return**语句)为止。换言之，Generator函数是分段执行的，**yield** 语句是暂停执行的标记，而 **next** 方法可以恢复执行。

```js
hw.next()
// { value: 'hello', done: false}

hw.next()
// { value: 'world', done: false}

hw.next()
// { value: 'ending', done: ture}

hw.next()
// { value: undefined, done: ture}
```

总结一下，调用Generator函数，返回一个遍历器对象,代表Generator函数的内部指针。以后，每次调用遍历器对象的next方法，就会返回一个有着value和done两个属性的对象。**value** 属性表示当前的内部状态的值，是yield语句后面那个表达式的值；**done** 属性值是一个布尔值，表示遍历是否结束。
```js
*app() {

}
// 解构

app: function* () {

}
```
