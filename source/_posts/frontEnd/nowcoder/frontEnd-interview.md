---
title: 面试问题记录
date: 2023-07-01 20:00:00
tags:
 - 面试
categories: 
 - article
 - frontEnd
---

最近的面试题汇总

<!--more-->

### 微购科技 2023-06-16
1. 为什么Hooks只能在函数组件中使用？（开局放大招，直接白给）
    > React Hooks 是 React 16.8 的新增特性，它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。Hooks 是一些可以让你在函数组件里“钩入” React state 以及生命周期等特性的函数。Hooks 可以让函数组件拥有一些类组件特性，比如定义修改 state、组合生命周期，性能优化等。
Hooks 函数只能在函数组件内部使用，不可在函数组件外或者类组件中使用。这是因为 Hooks 是基于函数组件的设计，而类组件已经有了自己的生命周期和 state 管理机制，所以不需要使用 Hooks。

2. 说一下JavaScript的数据类型
    > 基本数据类型：Undefined、Null、Boolean、Number、String、Symbol（ES6新增）;引用数据类型：Object、Array、Function、Date、RegExp、Error

3. typeof和instanceof的区别
    > typeof是一元操作符，放在其单个操作数的前面，操作数可以是任意类型。返回值为表示操作数类型的一个字符串。instanceof是二元操作符，放在两个操作数中间，左边操作数是一个对象，右边操作数是一个函数。如果右边对象是左边对象的实例，则返回true，否则返回false。

4. 追问：instanceof的实现原理
    > instanceof的实现原理是：遍历左边对象的原型链，如果找到右边对象的原型，则返回true，否则返回false。

5. 追问：1 instanceof Number的结果是什么
    > 1 instanceof Number的结果是false，因为1是基本数据类型，不是对象，所以不是Number的实例。

6. 追问：'1' instanceof String的结果是什么
    > '1' instanceof String的结果是false，因为'1'是基本数据类型，不是对象，所以不是String的实例。

7. 引申：new Number(1) instanceof Number的结果是什么
    > new Number(1) instanceof Number的结果是true，因为new Number(1)是Number的实例。

8. 引申：new String('1') instanceof String的结果是什么
    > new String('1') instanceof String的结果是true，因为new String('1')是String的实例。

9. 既然'1'不是String的实例，那为什么'1'可以调用String的方法（自动装箱）
    > 因为JavaScript在调用字符串方法时，会自动将基本数据类型转换为对应的包装对象，然后再调用对应的方法，自动转换的过程称为装箱。但是这个过程在instanceof中不会发生，所以'1' instanceof String的结果是false。

10. 引申：怎么准确判断元素的类型
    > 使用Object.prototype.toString.call()方法，返回一个表示对象的字符串，格式为"[object 类型]"，其中类型就是对象的类型，例如：[object String]、[object Number]、[object Boolean]、[object Undefined]、[object Null]、[object Object]、[object Array]、[object Function]、[object Date]、[object RegExp]、[object Error]、[object Symbol]。

11. JavaScript中有哪些数组方法
    > push、pop、shift、unshift、splice、slice、concat、join、reverse、sort、indexOf、lastIndexOf、forEach、map、filter、some、every、reduce、reduceRight、find、findIndex、includes、fill、copyWithin

12. 追问：那么这些方法分别属于ES多少？
    > ES3：push、pop、shift、unshift、splice、slice、concat、join、reverse、sort、indexOf、lastIndexOf、forEach、map、filter、some、every、reduce、reduceRight、find、findIndex、includes、fill、copyWithin；
    <br /><br />ES5：forEach、map、filter、some、every、reduce、reduceRight、indexOf、lastIndexOf、find、findIndex、includes；
    <br /><br />ES6：copyWithin、fill、find、findIndex、includes

13. 追问：如果我要生成一个长度为10的随机数数组，只用一行代码，应该怎么做？
    ```js
    new Array(10).fill(0).map(item => Math.random())
    ```

14. 追问：为什么这里要用fill(0)？
    > 因为当使用 new Array(10) 创建数组时，实际上是创建一个空的数组对象，属性 length = 10。除此以外，这个对象是一个空对象。对象中并没有数组对应的索引键（index key）
    <br /><br />因此，如果直接使用 map 方法，会出现以下错误：Uncaught TypeError: Cannot read property 'map' of undefined
    <br /><br />为了解决这个问题，可以使用 fill() 方法来填充数组，然后再使用 map() 方法。例如：new Array(10).fill(0).map(() => Math.random())

15. 引申：如果我要生成一个长度为10的随机数数组，只用一行代码，且每个元素的值都不相同，应该怎么做？
    ```js
    let arr = new Array(10).fill(0).map(i=>Math.floor(Math.random() * 10));
    while (new Set(arr).size !== arr.length) {
        arr = new Array(10).fill(0).map(i=>Math.floor(Math.random() * 10));
    }
    ```

16. 谈谈你对前端设计模式的理解
    > 前端设计模式是一套被反复使用的、多数人知晓的、经过分类编目的、代码设计经验的总结。前端常见的设计模式有以下几种
    <br /><br />1. 外观模式（Facade Pattern）
    <br />2. 代理模式（Proxy Pattern）
    <br />3. 工厂模式（Factory Pattern）
    <br />4. 单例模式（Singleton Pattern）
    <br />5. 策略模式（Strategy Pattern）
    <br />6. 观察者模式（Observer Pattern）
    <br />7. 装饰器模式（Decorator Pattern）
    <br />8. 适配器模式（Adapter Pattern）
    <br />9. 模板方法模式（Template Method Pattern）
    <br /><br />这些设计模式都有各自的特点和应用场景，可以根据实际需求选择合适的设计模式来解决问题。

17. 引申：谈谈你对原型链的理解
    > 原型链是由原型对象组成的链式结构，每个对象都有一个原型对象，对象的原型对象也有自己的原型对象，这样就形成了一个原型链。原型链的作用是：当对象访问一个属性时，如果对象本身没有这个属性，则会去原型对象中查找，如果原型对象中也没有这个属性，则会去原型对象的原型对象中查找，以此类推，直到找到这个属性或者找到原型链的尽头。原型链的尽头是Object.prototype，Object.prototype的原型对象为null。

### 瑞云科技 2023-06-21

1. 未完待续
