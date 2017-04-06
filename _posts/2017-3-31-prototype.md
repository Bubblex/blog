---
layout: default
title: 原型
---

# 原型

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [原型](#原型)
    * [[[Prototype]]](#prototype)
        * [Object.prototype](#objectprototype)
        * [属性设置和屏蔽](#属性设置和屏蔽)
    * ["类"](#类)
        * ["类"函数](#类函数)
            * [关于名称](#关于名称)
        * ["构造函数"](#构造函数)
            * [构造函数还是调用](#构造函数还是调用)
    * [技术](#技术)

<!-- tocstop -->

## [[Prototype]]
引用对象的属性时会触发[[Get]]操作，对于默认的[[Get]]操作，第一步是检查对象本身是否有这个属性，如果无法在对象本身找到需要的属性，就会继续访问对象的[[Prototype]]链。查找完整条[[Prototype]]链时，[[Get]]操作的返回值是underfined。
**Object.create( ... )** 会创建一个对象并把这个对象的[[Prototype]]关联到指定的对象。
使用 **for...in** 遍历对象时原理和查找[[Prototype]]链类似，任何可以通过原型链访问到的属性都会被枚举。使用in操作符来检查属性在对象中是否存在时，同样会查找对象的整条原型链(无论属性是否可枚举)。

### Object.prototype
所有普通的[[Prototype]]链最终都会指向内置的Object.prototype。由于所有的“普通”(内置)对象都“源于”(或者说把[[prototype]]链的顶端设置为)这个Object.prototype对象，所有它包含JavaScript中许多通用的功能。

### 属性设置和屏蔽
```js
myObject.foo = "bar"
```
给一个对象设置属性并不仅仅是添加一个新属性或者修改已有的属性值。
- 如果myObject对象中包含名为foo的普通数据访问属性，这条赋值语句会只修改现有的属性值
- 如果foo不直接存在于myObject中，[[Prototype]]链就会被遍历，类似于[[Get]]操作。如果原型链上找不到foo，foo就被直接添加到myObject上。
- 如果属性名foo既出现在myObject中也出现在myObject的[[Prototype]]链上层，就会发生 **屏蔽** 。myObject中的foo属性会屏蔽原型链上层所有的foo属性。 **myObject.foo总会选择原型链中最底层的foo属性** 。
- 如果foo存在于原型链上层，会出现三种情况
  - 如果原型链上的foo普通数据访问属性没有被标记为只读(writable:true),那就会直接在myObject中添加一个名为foo的新属性，它是 **屏蔽属性** 。
  - 如果被标记为只读(writable:false)，那么无法修改已有属性或者在myObject上创建屏蔽属性。严格模式下抛错，否则，忽略赋值语句。不会发生屏蔽。
  - 如果原型链上存在foo并且它是一个setter，那就一定会调用这个setter。foo不会添加到myObject，也不会重新定义foo这个setter。不会发生屏蔽。

向[[Prototype]]链上层已经存在的属性[[Put]]赋值，不一定会触发屏蔽。
如果希望后两种情况发生屏蔽foo，使用 **Object.defineProperty(...)** 来向myObject添加foo。

隐式屏蔽
```js
var anotherObject = {
    a ：2
}

var myObject = Object.create( anotherObject)

anotherObject.a // 2
myObject.a  // 2

anotherObject.hasOwnProperty("a") // true
myObject.hasOwnProperty("a") // false

myObject.a++  //  隐式屏蔽！！

anotherObject.a  // 2
myObject.a  // 3

myObject.hasOwnProperty("a")  // true
```
++相当于 myObject.a = myObject.a + 1。因此++会首先通过原型查找属性anotherObject.a获取当前属性值2，然后给这个值加1，接着用[[Put]]将值赋给myObject中新建的屏蔽属性a。
修改 **委托属性** 时要小心

## "类"
JavaScript和面向类的语言不同，它并没有类来作为对对象的抽象模式。JavaScript中只有对象，它是可以不通过类直接创建对象的语言，对象直接定义自己的行为。 **JavaScript中只有对象。**

### "类"函数
函数的一种特殊属性：有一个公有且不可枚举的prototype属性，他会指向另一个对象。
```js
function Foo() {
    // ...
}

Foo.prototype; // { }
```
这个对象成为Foo的原型
```js
function Foo() {
    // ...
}

var a = new Foo();

Object.getPrototypeOf( a ) === Foo.prototype; // true

```
a内部的[[Prototype]]链接到Foo.prototype所指向的对象。
在面向类的语言中，类可以被复制(实例化)多次，实例化(继承)一个类就意味着 **把类的行为复制到物理对象中** ，每一个新的实例都会重复这个过程。
在JavaScript中，没有类似的复制机制。不能创建一个类的多个实例，只能创建多个对象，它们的[[Prototpye]]。
实际上我们并没有从"类"中复制任何行为到一个对象中，只是让两个对象互相关联。这个关联是一个以外的副作用。new Foo()间接完成了我们的目标： **一个关联到其他对象的新对象** ，Object.create(...)可以直接做到这一点。

#### 关于名称

- 继承：意味着复制操作，JavaScript(默认)并不会复制对象属性。在传统的基于Class的语言如Java、C++中，继承的本质是扩展一个已有的Class，并生成新的Subclass。
- 委托：一个对象通过委托访问另一个对象的属性和函数，更准确描述JavaScript中对象的关联机制。
- 原型继承
- 差异继承

### "构造函数"
面向类的语言中构造实例会用到关键字new
```js
functon Foo() {
    // ...
}

Foo.prototype.constructor === Foo; // true

var a = new Foo();
a.constructor === Foo; // true
```
Foo.prototype 在函数声明时默认有一个公有并且不可枚举的属性.constructor，这个函数引用的是 **对象关联的函数**　本例是Foo。
可以看到a调用new Foo() 创建的对象也有一个.constructor属性，指向“创建这个对象的函数”。实际上a本身并没有.constructor属性。
a.constructor只是通过默认的[[Prototype]]委托指向Foo,这和“构造”毫无关系。
#### 构造函数还是调用
Foo和其他函数没有任何区别。函数本身并不是构造函数，在普通函数调用前面加上new关键字之后，就会把这个函数变成一个“构造函数调用”。实际上，new会劫持所有普通函数并用构造对象的形式调用它。
```js
function NothingSpecial() {
    console.log("Don't mind me!")
}

var a = new NothingSpecial();
// "Don't mind me!"

a; // {}
```
new调用是一个 **构造函数调用**，NothingSpecial本身并不是一个构造函数。
JavaScript中的“构造函数”：所有带有new的函数调用。
函数不是构造函数，当且仅当使用new时，函数调用会变成“构造函数调用”。
## 技术
