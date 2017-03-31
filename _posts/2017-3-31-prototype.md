---
layout: default
title: 原型
---

# 原型
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
