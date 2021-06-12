---
title: 'Object.create和对象字面量的区别'
date: 2021-06-12 22:52:00
tags: ['javascript']
---

<br/>

平时我们写代码时经常使用对象，用的最多的就是字面量方法创建如： const obj = {}，最近在整理JS继承方式时发现 Object.create() 也可以创建对象，那这两种方式有差别吗？

<br/>

## 对象字面量方式创建对象
```javascript
var objA = {};
objA.name = 'a';
objA.sayName = function() {
    console.log(`My name is ${this.name} !`);
}

objA.sayName();
console.log(objA.__proto__ === Object.prototype); // true
console.log(objA instanceof Object); // true
```

<br/>

## 回顾一下new一个对象其实做了以下三步：
```javascript
var obj = new Object(); // 创建一个空对象
obj.__proto__ = F.prototype; // obj的__proto__指向构造函数的prototype
var result = F.call(obj); // 把构造函数的this指向obj，并执行构造函数
```

### <font color='red'> 这里重点：obj的__proto__指向构造函数的prototype  </font>
<br/>
<br/>

# Object.create()
> Object.create()方法创建一个新对象，使用<font color='red' size='4'>已有的对象</font>来提供新创建的对象的__proto__
```javascript
const person = {
  isHuman: false,
  printIntroduction: function () {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};
const me = Object.create(person); // me.__proto__ === person
me.name = "Matthew";
me.isHuman = true; // 继承的属性可以被重写
me.printIntroduction(); // My name is Matthew. Am I human? true
```
### <font color='red'> 这里的重点在于me.__proto__ === person </font>也就是说me对象的内置原型直接指向了person

<br/>

## 再来看看第二个参数
>  Object.create(proto,[propertiesObject])

* proto必填参数，是新对象的原型对象，如上面代码里新对象me的__proto__指向person。注意，如果这个参数是null，那新对象就彻彻底底是个空对象，没有继承Object.prototype上的任何属性和方法，如hasOwnProperty()、toString()等。

```javascript
var a = Object.create(null);
console.dir(a); // {}
console.log(a.__proto__); // undefined
console.log(a.__proto__ === Object.prototype); // false
console.log(a instanceof Object); // false 没有继承`Object.prototype`上的任何属性和方法，所以原型链上不会出现Object
```

* propertiesObject是可选参数，指定要添加到新对象上的可枚举的属性

<br/>

```javascript
var cc = Object.create({b: 1}, {
    a: {
        value: 3,
        writable: true,
        configurable: true
    }
});
console.log(cc); // {a: 3}
console.log(cc.__proto__); // {b: 1} 新对象cc的__proto__指向{b: 1}
console.log(cc.__proto__ === Object.protorype); // false
```
可以看出第二个参数是cc的对象，而b是cc继承的对象

{% asset_img assets1.png %}
<br/>

# 总结一下
* 对象字面量创建的对象是Object的实例，原型指向Object.prototype。
* Object.create(arg, pro)创建的对象的原型取决于arg，arg为null，新对象是空对象，没有原型，不继承任何对象；arg为指定对象，新对象的原型指向指定对象。


