---
title: 浅谈JS的数组拍平
date: 2022-06-10
tags:
 - 转载
categories: 
 - frontEnd
 - JavaScript
---
一、什么是数组扁平化

1. 扁平化，顾名思义就是减少复杂性装饰，使其事物本身更简洁、简单，突出主题。
2. 数组扁平化，对着上面意思套也知道了，就是将一个复杂的嵌套多层的数组，一层一层的转化为层级较少或者只有一层的数组。

<!--more-->

## 一段代码总结 `Array.prototype.flat()` 特性

> 注：数组拍平方法 `Array.prototype.flat()` 也叫数组扁平化、数组拉平、数组降维。本文统一叫：数组拍平

```js
const animals = [" ", [" ", " "], [" ", [" ", [" "]], " "]];

// 不传参数时，默认“拉平”一层
animals.flat();
// [" ", " ", " ", " ", [" ", [" "]], " "]

// 传入一个整数参数，整数即“拉平”的层数
animals.flat(2);
// [" ", " ", " ", " ", " ", [" "], " "]

// Infinity 关键字作为参数时，无论多少层嵌套，都会转为一维数组
animals.flat(Infinity);
// [" ", " ", " ", " ", " ", " ", " "]

// 传入 <=0 的整数将返回原数组，不“拉平”
animals.flat(0);
animals.flat(-10);
// [" ", [" ", " "], [" ", [" ", [" "]], " "]];

// 如果原数组有空位，flat()方法会跳过空位。
[" ", " ", " ", " ",,].flat();
// [" ", " ", " ", " "]
```

### `Array.prototype.flat()` 特性总结

* `Array.prototype.flat()` 用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。
* 不传参数时，默认“拉平”一层，可以传入一个整数，表示想要“拉平”的层数。
* 传入 `<=0` 的整数将返回原数组，不“拉平”
* `Infinity` 关键字作为参数时，无论多少层嵌套，都会转为一维数组
* 如果原数组有空位，`Array.prototype.flat()` 会跳过空位。

## 面试官 N 连问

### 第一问：实现一个简单的数组拍平 `flat` 函数

首先，我们将花一点篇幅来探讨如何实现一个简单的数组拍平 `flat` 函数，详细介绍多种实现的方案，然后再尝试接住面试官的连环追问。

### 实现思路

如何实现呢，思路非常简单：实现一个有数组拍平功能的 `flat` 函数， **我们要做的就是在数组中找到是数组类型的元素，然后将他们展开** 。这就是实现数组拍平 `flat` 方法的关键思路。

有了思路，我们就需要解决实现这个思路需要克服的困难：

* **第一个要解决的就是遍历数组的每一个元素；**
* **第二个要解决的就是判断元素是否是数组；**
* **第三个要解决的就是将数组的元素展开一层；**

### 遍历数组的方案

遍历数组并取得数组元素的方法非常之多， **包括且不限于下面几种** ：

* `for 循环`
* `for...of`
* `for...in`
* `forEach()`
* `entries()`
* `keys()`
* `values()`
* `reduce()`
* `map()`

```js
const arr = [1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]], 5, "string", { name: "弹铁蛋同学" }];
// 遍历数组的方法有太多，本文只枚举常用的几种
// for 循环
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
// for...of
for (let value of arr) {
  console.log(value);
}
// for...in
for (let i in arr) {
  console.log(arr[i]);
}
// forEach 循环
arr.forEach(value => {
  console.log(value);
});
// entries（）
for (let [index, value] of arr.entries()) {
  console.log(value);
}
// keys()
for (let index of arr.keys()) {
  console.log(arr[index]);
}
// values()
for (let value of arr.values()) {
  console.log(value);
}
// reduce()
arr.reduce((pre, cur) => {
  console.log(cur);
}, []);
// map()
arr.map(value => console.log(value));
```

只要是能够遍历数组取到数组中每一个元素的方法，都是一种可行的解决方案。

### 判断元素是数组的方案

* `instanceof`
* `constructor`
* `Object.prototype.toString`
* `isArray`

```js
const arr = [1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]], 5, "string", { name: "弹铁蛋同学" }];
arr instanceof Array
// true
arr.constructor === Array
// true
Object.prototype.toString.call(arr) === '[object Array]'
// true
Array.isArray(arr)
// true
```

 **说明** ：

* `instanceof` 操作符是假定只有一种全局环境，如果网页中包含多个框架，多个全局环境，如果你从一个框架向另一个框架传入一个数组，那么传入的数组与在第二个框架中原生创建的数组分别具有各自不同的构造函数。（所以在这种情况下会不准确）
* `typeof` 操作符对数组取类型将返回 `object`
* 因为 `constructor` 可以被重写，所以不能确保一定是数组。

`javascript const str = 'abc'; str.constructor = Array; str.constructor === Array // true`

### 将数组的元素展开一层的方案

* 扩展运算符 + `concat`

`concat()` 方法用于合并两个或多个数组，在拼接的过程中加上扩展运算符会展开一层数组。详细见下面的代码。

* `conca`t +`apply`

主要是利用 `apply` 在绑定作用域时，传入的第二个参数是一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 `func` 函数。也就是在调用 `apply` 函数的过程中，会将传入的数组一个一个的传入到要执行的函数中，也就是相当对数组进行了一层的展开。

* `toString` + `split`

不推荐使用 `toString` + `split` 方法，因为操作字符串是和危险的事情，在上一面文章中我做了一个操作字符串的案例还被许多小伙伴们批评了。如果数组中的元素所有都是数字的话，`toString` +`split` 是可行的，并且是一步搞定。

```js
const arr = [1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]], 5, "string", { name: "弹铁蛋同学" }];
// 扩展运算符 + concat
[].concat(...arr)
// [1, 2, 3, 4, 1, 2, 3, [1, 2, 3, [1, 2, 3]], 5, "string", { name: "弹铁蛋同学" }];

// concat + apply
[].concat.apply([], arr);
// [1, 2, 3, 4, 1, 2, 3, [1, 2, 3, [1, 2, 3]], 5, "string", { name: "弹铁蛋同学" }];

// toString  + split
const arr2 =[1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]]]
arr2.toString().split(',').map(v=>parseInt(v))
// [1, 2, 3, 4, 1, 2, 3, 1, 2, 3, 1, 2, 3]
```

总结完要解决的三大困难，那我们就可以非常轻松的实现一版数组拍平 `flat` 函数了。

```js
const arr = [1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]], 5, "string", { name: "弹铁蛋同学" }];
// concat + 递归
function flat(arr) {
  let arrResult = [];
  arr.forEach(item => {
    if (Array.isArray(item)) {
      arrResult = arrResult.concat(arguments.callee(item)));   // 递归
      // 或者用扩展运算符
      // arrResult.push(...arguments.callee(item));
    } else {
      arrResult.push(item);
    }
  });
  return arrResult;
}
flat(arr)
// [1, 2, 3, 4, 1, 2, 3, 1, 2, 3, 1, 2, 3, 5, "string", { name: "弹铁蛋同学" }];
```

到这里，恭喜你成功得到了面试官对你手撕代码能力的基本认可 。但是面试官往往会不止于此，将继续考察面试者的各种能力。

### 第二问：用 `reduce` 实现 `flat` 函数

我见过很多的面试官都很喜欢点名道姓的要面试者直接用 `reduce` 去实现 `flat` 函数。想知道为什么？文章后半篇我们考虑数组空位的情况的时候就知道为啥了。其实思路也是一样的。

```js
const arr = [1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]], 5, "string", { name: "弹铁蛋同学" }]

// 首先使用 reduce 展开一层
arr.reduce((pre, cur) => pre.concat(cur), []);
// [1, 2, 3, 4, 1, 2, 3, [1, 2, 3, [1, 2, 3]], 5, "string", { name: "弹铁蛋同学" }];

// 用 reduce 展开一层 + 递归
const flat = arr => {
  return arr.reduce((pre, cur) => {
    return pre.concat(Array.isArray(cur) ? flat(cur) : cur);
  }, []);
};
// [1, 2, 3, 4, 1, 2, 3, 1, 2, 3, 1, 2, 3, 5, "string", { name: "弹铁蛋同学" }];
```

### 第三问：使用栈的思想实现 `flat` 函数

```js
// 栈思想
function flat(arr) {
  const result = []; 
  const stack = [].concat(arr);  // 将数组元素拷贝至栈，直接赋值会改变原数组
  //如果栈不为空，则循环遍历
  while (stack.length !== 0) {
    const val = stack.pop(); 
    if (Array.isArray(val)) {
      stack.push(...val); //如果是数组再次入栈，并且展开了一层
    } else {
      result.unshift(val); //如果不是数组就将其取出来放入结果数组中
    }
  }
  return result;
}
const arr = [1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]], 5, "string", { name: "弹铁蛋同学" }]
flat(arr)
// [1, 2, 3, 4, 1, 2, 3, 1, 2, 3, 1, 2, 3, 5, "string", { name: "弹铁蛋同学" }];
```

### 第四问：通过传入整数参数控制“拉平”层数

```js
// reduce + 递归
function flat(arr, num = 1) {
  return num > 0
    ? arr.reduce(
        (pre, cur) =>
          pre.concat(Array.isArray(cur) ? flat(cur, num - 1) : cur),
        []
      )
    : arr.slice();
}
const arr = [1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]], 5, "string", { name: "弹铁蛋同学" }]
flat(arr, Infinity);
// [1, 2, 3, 4, 1, 2, 3, 1, 2, 3, 1, 2, 3, 5, "string", { name: "弹铁蛋同学" }];
```

### 第五问：使用 `Generator` 实现 `flat` 函数

```js
function* flat(arr, num) {
  if (num === undefined) num = 1;
  for (const item of arr) {
    if (Array.isArray(item) && num > 0) {   // num > 0
      yield* flat(item, num - 1);
    } else {
      yield item;
    }
  }
}
const arr = [1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]], 5, "string", { name: "弹铁蛋同学" }]
// 调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象。
// 也就是遍历器对象（Iterator Object）。所以我们要用一次扩展运算符得到结果
[...flat(arr, Infinity)]  
// [1, 2, 3, 4, 1, 2, 3, 1, 2, 3, 1, 2, 3, 5, "string", { name: "弹铁蛋同学" }];
```

### 第六问：实现在原型链上重写 `flat` 函数

```js
Array.prototype.fakeFlat = function(num = 1) {
  if (!Number(num) || Number(num) < 0) {
    return this;
  }
  let arr = this.concat();    // 获得调用 fakeFlat 函数的数组
  while (num > 0) {         
    if (arr.some(x => Array.isArray(x))) {
      arr = [].concat.apply([], arr);   // 数组中还有数组元素的话并且 num > 0，继续展开一层数组 
    } else {
      break; // 数组中没有数组元素并且不管 num 是否依旧大于 0，停止循环。
    }
    num--;
  }
  return arr;
};
const arr = [1, 2, 3, 4, [1, 2, 3, [1, 2, 3, [1, 2, 3]]], 5, "string", { name: "弹铁蛋同学" }]
arr.fakeFlat(Infinity)
// [1, 2, 3, 4, 1, 2, 3, 1, 2, 3, 1, 2, 3, 5, "string", { name: "弹铁蛋同学" }];
```

### 第七问：考虑数组空位的情况

由最开始我们总结的 `flat` 特性知道，`flat` 函数执行是会跳过空位的。ES5 大多数数组方法对空位的处理都会选择跳过空位包括：`forEach()`, `filter()`, `reduce()`, `every()` 和 `some()`都会跳过空位。

所以我们可以利用上面几种方法来实现 flat 跳过空位的特性

```js
// reduce + 递归
Array.prototype.fakeFlat = function(num = 1) {
  if (!Number(num) || Number(num) < 0) {
    return this;
  }
  let arr = [].concat(this);
  return num > 0
    ? arr.reduce(
        (pre, cur) =>
          pre.concat(Array.isArray(cur) ? cur.fakeFlat(--num) : cur),
        []
      )
    : arr.slice();
};
const arr = [1, [3, 4], , ,];
arr.fakeFlat()
// [1, 3, 4]

// foEach + 递归
Array.prototype.fakeFlat = function(num = 1) {
  if (!Number(num) || Number(num) < 0) {
    return this;
  }
  let arr = [];
  this.forEach(item => {
    if (Array.isArray(item)) {
      arr = arr.concat(item.fakeFlat(--num));
    } else {
      arr.push(item);
    }
  });
  return arr;
};
const arr = [1, [3, 4], , ,];
arr.fakeFlat()
// [1, 3, 4]
```

### 扩展阅读：**由于空位的处理规则非常不统一，所以建议避免出现空位。**

**ES5 对空位的处理，就非常不一致，大多数情况下会忽略空位。**

* `forEach()`, `filter()`, `reduce()`, `every()` 和 `some()`都会跳过空位。
* `map()`会跳过空位，但会保留这个值。
* `join()`和 `toString()`会将空位视为 `undefined`，而 `undefined`和 `null`会被处理成空字符串。

**ES6 明确将空位转为 `undefined`。**

* `entries()`、`keys()`、`values()`、`find()`和 `findIndex()`会将空位处理成 `undefined`。
* `for...of`循环会遍历空位。
* `fill()`会将空位视为正常的数组位置。
* `copyWithin()`会连空位一起拷贝。
* 扩展运算符（`...`）也会将空位转为 `undefined`。
* `Array.from`方法会将数组的空位，转为 `undefined`。

## 总结

面试官现场考察一道写代码的题目，其实不仅仅是写代码，在写代码的过程中会遇到各种各样的知识点和代码的边界情况。虽然大多数情况下，面试官不会那么变态，就 `flat` 实现去连续追问面试者，并且手撕好几个版本，但面试官会要求在你写的那版代码的基础上再写出一个更完美的版本是常有的事情。只有我们沉下心来把基础打扎实，不管面试官如何追问，我们都能自如的应对。`flat` 的实现绝对不会只有文中列出的这几个版本，敲出自己的代码是最好的进步，在评论区或者在 issue 中写出你自己的版本吧！

[查看原文](https://zhuanlan.zhihu.com/p/99842362 "查看原文")
