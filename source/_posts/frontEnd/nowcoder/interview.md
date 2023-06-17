---
title: 面试问题预判
date: 2023-06-30 20:00:00
tags:
 - 面试
categories: 
 - article
 - frontEnd
---

个人面试题库，暂时仅记录一些平时可能答不明白的题目，懂的题目可能不会写。

<!--more-->
## JavaScript
### 基础
1. 说一下JavaScript的数据类型
   > 基本数据类型：Undefined、Null、Boolean、Number、String、Symbol（ES6新增）;引用数据类型：Object、Array、Function、Date、RegExp、Error
2. 基本数据类型和引用数据类型的区别
   > 基本数据类型存储在栈内存中，引用数据类型存储在堆内存中；基本数据类型的值是不可变的，引用数据类型的值是可变的；基本数据类型的比较是值的比较，引用数据类型的比较是引用的比较。

### 原型链相关
1. 谈谈你对原型链的理解
   > 原型链是由原型对象组成的链式结构，每个对象都有一个原型对象，对象的原型对象也有自己的原型对象，这样就形成了一个原型链。原型链的作用是：当对象访问一个属性时，如果对象本身没有这个属性，则会去原型对象中查找，如果原型对象中也没有这个属性，则会去原型对象的原型对象中查找，以此类推，直到找到这个属性或者找到原型链的尽头。原型链的尽头是Object.prototype，Object.prototype的原型对象为null。
2. call、apply、bind的区别
   > call、apply、bind都是用来改变函数的this指向的，call和apply的区别在于传参的方式不同，call是一个一个传参，apply是以数组的形式传参，bind是返回一个新的函数，这个函数的this指向绑定的对象，参数是bind传入的参数和新函数传入的参数的合集。
3. 谈谈你对继承的理解
   > 继承是指子类继承父类的属性和方法，子类可以使用父类的属性和方法，继承的方式有原型链继承、构造函数继承、组合继承、寄生组合继承、ES6的class继承。
4. 谈谈你对constructor的理解
   > constructor是构造函数，每个函数都有一个constructor属性，指向构造函数本身，当函数作为构造函数使用时，constructor指向构造函数本身，当函数作为普通函数使用时，constructor指向Function。

### 数组方法
1. JavaScript中有哪些数组方法
    > push、pop、shift、unshift、splice、slice、concat、join、reverse、sort、indexOf、lastIndexOf、forEach、map、filter、some、every、reduce、reduceRight、find、findIndex、includes、fill、copyWithin
2. 那么这些方法分别属于ES多少
    > ES3：push、pop、shift、unshift、splice、slice、concat、join、reverse、sort、indexOf、lastIndexOf、forEach、map、filter、some、every、reduce、reduceRight、find、findIndex、includes、fill、copyWithin；
   <br /><br />ES5：forEach、map、filter、some、every、reduce、reduceRight、indexOf、lastIndexOf、find、findIndex、includes；
   <br /><br />ES6：copyWithin、fill、find、findIndex、includes
3. 如何删除数组中的元素
   > splice、pop、shift；其中splice可以删除任意位置的元素，pop删除最后一个元素，shift删除第一个元素。他们的用法都是传入一个参数，表示要删除的元素的位置，splice还可以传入第二个参数，表示要删除的元素的个数。

## React相关
### 基础
1. 说一下React的生命周期
   > React的生命周期分为三个阶段：挂载阶段、更新阶段、卸载阶段。挂载阶段包括constructor、getDerivedStateFromProps、render、componentDidMount；更新阶段包括getDerivedStateFromProps、shouldComponentUpdate、render、getSnapshotBeforeUpdate、componentDidUpdate；卸载阶段包括componentWillUnmount。
2. React的setState是同步还是异步
   > React的setState是异步的，当调用setState时，React会将要更新的状态放入一个队列中，等到所有的setState都执行完毕后，再统一更新状态，这样可以提高性能。但是有时候我们需要在setState执行完毕后立即获取更新后的状态，这时候可以在setState的第二个参数中传入一个回调函数，这个回调函数会在setState执行完毕后立即执行。

### 原理
1. Fiber是什么
   > Fiber是React16中引入的新的协调机制，它的目的是为了解决React在渲染过程中出现的卡顿问题。在React15中，渲染过程是同步的，一旦开始渲染，就会一直执行到渲染结束，如果渲染的组件树很大，就会导致渲染时间很长，这样就会造成页面卡顿。在React16中，引入了Fiber，将渲染过程分成了多个小任务，每个小任务执行时间很短，这样就不会造成页面卡顿。
2. 谈谈你对虚拟DOM的理解
   > 虚拟DOM是React中的一个概念，它是用一个普通的JS对象来描述一个DOM节点，这个普通的JS对象就是虚拟DOM。虚拟DOM的好处是可以进行批量更新，当数据发生变化时，会生成一个新的虚拟DOM，然后将新的虚拟DOM和旧的虚拟DOM进行比较，找出差异，然后将差异更新到真实的DOM上，这样就可以减少对真实DOM的操作，提高性能。
3. React的diff和vue的diff有什么区别
    > React和Vue都使用了Diff算法来提高渲染性能，但是它们的实现方式有所不同。
   <br />在React中，Diff算法是在组件树的每个节点上进行的。React会对每个节点进行以下几个方面的比较：
   <br />节点类型：如果节点类型不同，则React会销毁旧节点并创建新节点。
   <br />节点属性：如果节点属性不同，则React会更新节点属性。
   <br />子节点：如果子节点不同，则React会递归地对子节点进行比较。
   <br />在Vue中，Diff算法是在虚拟DOM上进行的。Vue会将模板编译成虚拟DOM，并在每次更新时重新渲染虚拟DOM。Vue会对每个虚拟DOM节点进行以下几个方面的比较：
   <br />节点类型：如果节点类型不同，则Vue会销毁旧节点并创建新节点。
   <br />节点属性：如果节点属性不同，则Vue会更新节点属性。
   <br />子节点：如果子节点不同，则Vue会递归地对子节点进行比较。
   <br />总的来说，React和Vue的Diff算法都是基于虚拟DOM的，但是它们的实现方式有所不同。React是在组件树上进行比较，而Vue是在虚拟DOM上进行比较。此外，Vue还使用了一些优化技巧，如异步更新和缓存策略等，来进一步提高渲染性能。 

### Hooks相关
1. 说一下你对Hooks的理解
   > Hooks是React16.8中引入的新特性，它可以让我们在不编写class的情况下使用state和其他的React特性。Hooks的目的是为了解决React中组件之间状态逻辑复用的问题，它可以让我们在不编写class的情况下使用state和其他的React特性。Hooks的目的是为了解决React中组件之间状态逻辑复用的问题，它可以让我们在不编写class的情况下使用state和其他的React特性。Hooks的目的是为了解决React中组件之间状态逻辑复用的问题，它可以让我们在不编写class的情况下使用state和其他的React特性。
2. React中有哪些常用的Hooks
   > useState、useEffect、useContext、useReducer、useCallback、useMemo、useRef、useImperativeHandle、useLayoutEffect、useDebugValue
