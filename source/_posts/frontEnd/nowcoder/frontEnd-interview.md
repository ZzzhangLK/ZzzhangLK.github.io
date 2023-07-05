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
    > - ES3：push、pop、shift、unshift、splice、slice、concat、join、reverse、sort、indexOf、lastIndexOf、forEach、map、filter、some、every、reduce、reduceRight、find、findIndex、includes、fill、copyWithin；
    > - ES5：forEach、map、filter、some、every、reduce、reduceRight、indexOf、lastIndexOf、find、findIndex、includes；
    > - ES6：copyWithin、fill、find、findIndex、includes

13. 追问：如果我要生成一个长度为10的随机数数组，只用一行代码，应该怎么做？
    ```js
    new Array(10).fill(0).map(item => Math.random())
    ```

14. 追问：为什么这里要用fill(0)？
    > - 因为当使用 new Array(10) 创建数组时，实际上是创建一个空的数组对象，属性 length = 10。除此以外，这个对象是一个空对象。对象中并没有数组对应的索引键（index key）
    > - 因此，如果直接使用 map 方法，会出现以下错误：Uncaught TypeError: Cannot read property 'map' of undefined
    > - 为了解决这个问题，可以使用 fill() 方法来填充数组，然后再使用 map() 方法。例如：new Array(10).fill(0).map(() => Math.random())

15. 引申：如果我要生成一个长度为10的随机数数组，只用一行代码，且每个元素的值都不相同，应该怎么做？
    ```js
    let arr = new Array(10).fill(0).map(i=>Math.floor(Math.random() * 10));
    while (new Set(arr).size !== arr.length) {
        arr = new Array(10).fill(0).map(i=>Math.floor(Math.random() * 10));
    }
    ```

16. 谈谈你对前端设计模式的理解
    > 前端设计模式是一套被反复使用的、多数人知晓的、经过分类编目的、代码设计经验的总结。前端常见的设计模式有以下几种
    > 1. 外观模式（Facade Pattern）
    > 2. 代理模式（Proxy Pattern）
    > 3. 工厂模式（Factory Pattern）
    > 4. 单例模式（Singleton Pattern）
    > 5. 策略模式（Strategy Pattern）
    > 6. 观察者模式（Observer Pattern）
    > 7. 装饰器模式（Decorator Pattern）
    > 8. 适配器模式（Adapter Pattern）
    > 9. 模板方法模式（Template Method Pattern）
    > 
    > 这些设计模式都有各自的特点和应用场景，可以根据实际需求选择合适的设计模式来解决问题。

17. 引申：谈谈你对原型链的理解
    > 原型链是由原型对象组成的链式结构，每个对象都有一个原型对象，对象的原型对象也有自己的原型对象，这样就形成了一个原型链。原型链的作用是：当对象访问一个属性时，如果对象本身没有这个属性，则会去原型对象中查找，如果原型对象中也没有这个属性，则会去原型对象的原型对象中查找，以此类推，直到找到这个属性或者找到原型链的尽头。原型链的尽头是Object.prototype，Object.prototype的原型对象为null。

### 瑞云科技 2023-06-21

#### 笔试
1. Object.prototype._proto_ 返回: undefined
    > Object.prototype._proto_的结果是undefined，因为Object.prototype是所有对象的原型对象，它没有原型对象。
   
2. '[object Object]' == {} 返回: true
    > 在JavaScript中，'[object Object]'是通过Object.prototype.toString.call()方法返回的字符串。而{}是一个对象字面量，它是Object的实例。当你使用==运算符比较两个对象时，它们会被转换为原始值。在这种情况下，它们都被转换为字符串’[object Object]'，因此它们相等，返回true。

3. '[object Object]' === {} 返回: false （引申）
    > 在JavaScript中，'[object Object]'是通过Object.prototype.toString.call()方法返回的字符串。而{}是一个对象字面量，它是Object的实例。当你使用===运算符比较两个对象时，它们不会被转换为原始值，因此它们不相等，返回false。

4. 写一个判断变量是否为数组的表达式
    > Array.isArray()

5. 写一个电话号码的正则表达式
    > /^1[3456789]\d{9}$/

6. 简述同源策略与跨源资源共享机制
    > 同源策略是浏览器的一种安全策略，它用于限制一个源的文档或脚本如何能与另一个源的资源进行交互。同源策略的限制包括以下几种：
    > 1. Cookie、LocalStorage 和 IndexDB 无法读取
    > 2. DOM 和 Js对象无法获得
    > 3. AJAX 请求不能发送
    > 
    > 跨源资源共享机制（CORS）是一种机制，它使用额外的HTTP头来告诉浏览器，让运行在一个 origin (domain) 上的Web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域 HTTP 请求。

7. 简述块格式化上下文（BFC）的作用
    > 块格式化上下文（BFC）是Web页面的可视化CSS渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其他元素的交互限定区域。BFC的作用有以下几点：
    > 1. 清除浮动
    > 2. 防止同一BFC容器中的相邻元素间的外边距重叠
    > 3. 防止元素被浮动元素覆盖
    > 4. 自适应两栏布局
    > 5. 防止文字环绕
    > 6. 分属于不同的BFC时，可以阻止margin重叠

8. 简述JavaScript的并发模型与事件循环
    > JavaScript是一门单线程语言，它的并发模型是基于事件循环的。事件循环是一个执行模型，用于等待和发送消息和事件。JavaScript的事件循环包含以下几个步骤：
    > 1. 执行全局Script同步代码，这些同步代码有可能会导致宏任务的执行，例如setTimeout、setInterval、setImmediate、I/O、UI交互事件、postMessage、MessageChannel、setImmediate等。
    > 2. 执行所有的微任务，微任务包括process.nextTick、Promise、Object.observe、MutationObserver等。
    > 3. 执行一个宏任务，执行完毕后再执行微任务，这样一直循环下去，直到所有的任务都执行完毕。

9. v1, v2的类型分别是什么?为什么?
    ```typescript
   function prop<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
    }
    function prop2<T>(obj: T, key: keyof T) {
    return obj[key];
    }
    
    let o = { p1: 0, p2: "" };
    let v1 = prop(o, "p1");
    let v2 = prop2(o, "p1");
   ```
   > - v1的类型是number，v2的类型是T[keyof T]
   > - 两个函数的区别在于第一个函数使用了泛型类型参数K extends keyof T来指定key参数应该是T对象类型的一个键。这意味着函数的返回类型被推断为T中具有该键的属性的类型。在这种情况下，由于键是“p1”，而对象具有一个类型为number的属性“p1”，因此返回类型被推断为number。
   > - 第二个函数使用keyof T作为键参数的类型。这意味着函数的返回类型被推断为T中所有可能属性类型的联合。在这种情况下，由于T具有类型为number和string的属性“p1”和“p2”，因此返回类型被推断为number | string。

10. 实现 range 函数，入参类型为数字，输出值与下标相同，长度为该数值的数组。
    ```typescript
     function range(n) {
          return new Array(n).fill(0).map((_, i) => i)
     }
    console.log(range(5)); // [0, 1, 2, 3, 4]
     ```
11. 现给定一个根据偏移量获取字母的方法getSentenceFragment，实现一个获取所有字母的方法getSentence
    完成以下输入输出。
    ```typescript
    const getSentenceFragment = (offset = 0) => new Promise((resolve, reject) => {
      const pageSize = 3;
      const sentence = [...'hello world'];
      setTimeout(() => resolve({
        data: sentence.slice(offset, offset + pageSize), 
        nextPage: offset + pagesize < sentence.length ? offset + pageSize : undefined
      },500));
    })
    
    const getSentence = () => {
        let result = [];
        let nextPage = 0;
        do {
            const {data, nextPage: next} = await getSentenceFragment(nextPage);
            result.push(...data);
            nextPage = next;
        } while (nextPage !== undefined);
        return result;
    }
    
    getSentence().then(console.log) // ['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd']
    ```
    

### 彩讯科技 2023-07-04

#### 笔试
