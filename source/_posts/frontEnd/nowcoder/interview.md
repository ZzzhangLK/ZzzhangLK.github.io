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
3. 请解释什么是事件循环？
   > 事件循环是一种处理异步操作的机制，它可以让我们在代码执行过程中处理异步任务，比如网络请求、定时器和用户交互等。事件循环的执行顺序是：首先执行同步代码，然后执行异步代码。当 JavaScript 引擎遇到需要等待的操作时，如 I/O 操作、setTimeout 等异步操作，它会将这些操作加入到任务队列中，等待下一次事件循环时执行。
4. 请解释什么是宏任务和微任务？
   > 宏任务和微任务都是异步任务。宏任务包括：script（整体代码）、setTimeout、setInterval、I/O 操作、UI 渲染等；微任务包括：Promise、process.nextTick、Object.observe、MutationObserver 等。在每次事件循环中，宏任务会优先于微任务执行。
5. 谈谈你对JavaScript的事件轮循的理解？
   > - JavaScript 是一门单线程语言，它的事件轮询机制是通过一个事件循环来实现的。浏览器通过事件队列来管理这些事件，当事件发生时，会将事件加入到事件队列中，然后等待 JavaScript 引擎执行。事件轮询机制的实现方式是通过一个事件循环来实现的。事件循环会不断地从事件队列中取出事件，然后执行相应的回调函数。
   > - JavaScript 引擎在执行 JavaScript 代码时，会将所有同步任务按照顺序放入执行栈中，然后依次执行。当遇到异步任务时，会将异步任务放入任务队列中，等待 JavaScript 引擎空闲时再去执行。当执行栈中的所有同步任务都执行完毕时，JavaScript 引擎会去查看任务队列中是否有任务需要执行。如果有，则将任务队列中的第一个任务取出来放入执行栈中执行。
6. 在JavaScript中，Object类的方法作用有哪些？
   > - Object.assign()：将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。
   > - Object.create()：使用指定的原型对象和属性创建一个新对象。
   > - Object.defineProperty()：定义一个新属性或修改现有属性，并指定该属性的描述符。
   > - Object.defineProperties()：定义一个或多个新属性或修改现有属性，并指定这些属性的描述符。
   > - Object.entries()：返回给定对象自身可枚举属性的键值对数组，其排列与使用 for…in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还枚举原型链中的属性）。
   > - Object.freeze()：冻结一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性。该方法返回被冻结的对象。
   > - Object.fromEntries()：把键值对列表转换为一个对象。
   > - Object.getOwnPropertyDescriptor()：返回指定对象上一个自有属性对应的属性描述符。
   > - Object.getOwnPropertyDescriptors()：返回指定对象上所有自有属性（非继承属性）对应的属性描述符。
   > - Object.getOwnPropertyNames()：返回一个数组，它包含了指定对象所有自身属性（非继承属性）的名称（不包含 Symbol 类型的属性）。 4
6. 谈谈你对跨域资源共享的机制的理解，以及跨域的几种解决方法
   > 跨域资源共享（CORS）是一种机制，它允许 Web 应用程序从不同的域访问其资源。跨域的几种方法包括：
   > - JSONP：通过动态创建 script 标签，将数据作为回调函数的参数传递。
   > - CORS：通过在服务器端设置响应头，允许指定的域名访问资源。
   > - 代理：通过在同一域名下设置代理服务器，将请求转发到目标服务器。

### 原型链相关
1. 谈谈你对原型链的理解
   > 原型链是由原型对象组成的链式结构，每个对象都有一个原型对象，对象的原型对象也有自己的原型对象，这样就形成了一个原型链。原型链的作用是：当对象访问一个属性时，如果对象本身没有这个属性，则会去原型对象中查找，如果原型对象中也没有这个属性，则会去原型对象的原型对象中查找，以此类推，直到找到这个属性或者找到原型链的尽头。原型链的尽头是Object.prototype，Object.prototype的原型对象为null。
2. call、apply、bind的区别
   > call、apply、bind都是用来改变函数的this指向的，call和apply的区别在于传参的方式不同，call是一个一个传参，apply是以数组的形式传参，bind是返回一个新的函数，这个函数的this指向绑定的对象，参数是bind传入的参数和新函数传入的参数的合集。
3. 谈谈你对继承的理解
   > 继承是指子类继承父类的属性和方法，子类可以使用父类的属性和方法，继承的方式有原型链继承、构造函数继承、组合继承、寄生组合继承、ES6的class继承。
4. 谈谈你对constructor的理解
   > constructor是构造函数，每个函数都有一个constructor属性，指向构造函数本身，当函数作为构造函数使用时，constructor指向构造函数本身，当函数作为普通函数使用时，constructor指向Function。
5. prototype 和 proto 区别是什么？
   > prototype是函数的一个属性，指向函数的原型对象；proto是对象的一个属性，指向对象的原型对象。<br />原型对象也是一个对象，它也有自己的 proto 属性，指向它的原型对象。这样一直往上追溯，直到最终的原型对象为 null 为止。null 没有 proto 属性，这样就形成了一个原型链。
6. 显式原型和隐式原型分别指什么？
   > 显式原型指的是函数的prototype属性，隐式原型指的是对象的proto属性。显式原型是函数的 prototype 属性，它指向一个对象，这个对象包含了所有实例共享的属性和方法；而隐式原型是每个实例对象都有的一个属性，它指向该对象的原型对象。实例对象的隐式原型的值，为它的构造函数的显式原型的值。
7. 原型链的终点是什么？
   > 原型链的终点是Object.prototype，Object.prototype的原型对象为null。

### 数组方法
1. JavaScript中有哪些数组方法
    > push、pop、shift、unshift、splice、slice、concat、join、reverse、sort、indexOf、lastIndexOf、forEach、map、filter、some、every、reduce、reduceRight、find、findIndex、includes、fill、copyWithin
2. 那么这些方法分别属于ES多少
   > - ES3：push、pop、shift、unshift、splice、slice、concat、join、reverse、sort、indexOf、lastIndexOf、forEach、map、filter、some、every、reduce、reduceRight、find、findIndex、includes、fill、copyWithin；
   > - ES5：forEach、map、filter、some、every、reduce、reduceRight、indexOf、lastIndexOf、find、findIndex、includes；
   > - ES6：copyWithin、fill、find、findIndex、includes
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
   > - 在React中，Diff算法是在组件树的每个节点上进行的。React会对每个节点进行以下几个方面的比较：
   > - 节点类型：如果节点类型不同，则React会销毁旧节点并创建新节点。
   > - 节点属性：如果节点属性不同，则React会更新节点属性。
   > - 子节点：如果子节点不同，则React会递归地对子节点进行比较。
   > - 在Vue中，Diff算法是在虚拟DOM上进行的。Vue会将模板编译成虚拟DOM，并在每次更新时重新渲染虚拟DOM。Vue会对每个虚拟DOM节点进行以下几个方面的比较：
   > - 节点类型：如果节点类型不同，则Vue会销毁旧节点并创建新节点。
   > - 节点属性：如果节点属性不同，则Vue会更新节点属性。
   > - 子节点：如果子节点不同，则Vue会递归地对子节点进行比较。
   > - 总的来说，React和Vue的Diff算法都是基于虚拟DOM的，但是它们的实现方式有所不同。React是在组件树上进行比较，而Vue是在虚拟DOM上进行比较。此外，Vue还使用了一些优化技巧，如异步更新和缓存策略等，来进一步提高渲染性能。 

### Hooks相关
1. 说一下你对Hooks的理解
   > Hooks是React16.8中引入的新特性，它可以让我们在不编写class的情况下使用state和其他的React特性。Hooks的目的是为了解决React中组件之间状态逻辑复用的问题，它可以让我们在不编写class的情况下使用state和其他的React特性。Hooks的目的是为了解决React中组件之间状态逻辑复用的问题，它可以让我们在不编写class的情况下使用state和其他的React特性。Hooks的目的是为了解决React中组件之间状态逻辑复用的问题，它可以让我们在不编写class的情况下使用state和其他的React特性。
2. React中有哪些常用的Hooks
   > useState、useEffect、useContext、useReducer、useCallback、useMemo、useRef、useImperativeHandle、useLayoutEffect、useDebugValue

## Vue相关
### 基础
1. v-if和v-show有什么区别?
   > - v-if和v-show都是Vue框架中的指令，它们的作用都是控制元素的显示和隐藏，区别在于：v-if是创建和删除元素，而v-show只是改变元素中的display样式属性。
   > - 一般来说，v-if有更高的切换开销，而v-show有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用v-show较好；如果在运行时条件很少改变，则使用v-if较好。
   > - v-if和v-show都是用来控制元素的渲染。v-if判断是否加载，可以减轻服务器的压力，在需要时加载,但有更高的切换开销;v-show调整DOM元素的CSS的dispaly属性，可以使客户端操作更加流畅，但有更高的初始渲染开销。如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
### 原理
1. Proxy和Object.defineProperty有什么区别?
   > Proxy 和 Object.defineProperty 的区别主要有以下几点
   > - Proxy 可以拦截整个对象，而 Object.defineProperty 只能拦截对象的属性。
   > - Proxy 可以监听一些 Object.defineProperty 监听不到的操作，比如监听数组，监听对象属性的新增、删除等。
   > - Proxy 返回的是一个新对象，我们可以只操作新的对象达到目的，而 Object.defineProperty 只能遍历对象属性直接修改。
   > - Proxy 不兼容 IE，而 Object.defineProperty 不兼容 IE8 及以下版本。

### Webpack相关
1. 说一下你对webpack的理解
   > Webpack是一个模块打包工具，它可以将各种资源，如JS、CSS、图片等都作为模块来处理和使用。
2. 谈谈你对webpack新特性的理解
   > - 模块联邦（Module Federation）：可以让多个应用程序共享代码，从而提高应用程序的性能和可维护性。
   > - 零配置（Zero Configuration）：可以让开发人员更快地开始使用 Webpack，而无需进行复杂的配置。
   > - 集成优化（Integrated Optimization）：可以让开发人员更轻松地优化应用程序的性能。
   > - 改进的 Tree Shaking：可以更好地优化应用程序的大小。
   > - 改进的缓存：可以更好地缓存应用程序的代码。
3. webpack的五大模块有哪些
   > - Entry：入口模块，Webpack会从入口模块开始，递归地找出所有的依赖模块。
   > - Output：输出模块，Webpack会将所有的依赖模块打包成一个或多个bundle文件。
   > - Loader：模块转换器，Webpack只能理解JavaScript和JSON文件，而Loader可以让Webpack处理其他类型的文件，并将它们转换为有效的模块，以供应用程序使用，以及被添加到依赖图中。
   > - Plugins：扩展插件，在Webpack构建流程中的特定时机会广播出对应的事件，插件可以监听这些事件的发生，在特定时机做对应的事情。
   > - Mode：模式，Webpack提供了三种模式，分别是development、production和none。Mode的作用是设置Webpack的内置优化，不同的模式会有不同的优化策略。
4. 谈谈你对webpack生命周期的理解
   > - 初始化（Initialization）：在 Webpack 开始编译前执行，用于初始化插件等。
   > - 编译（Compilation）：在 Webpack 开始编译时执行，用于收集模块依赖等。
   > - 构建模块（Module Build）：在 Webpack 编译模块时执行，用于处理模块代码等。
   > - 完成模块构建（Module Completion）：在 Webpack 完成模块构建时执行，用于处理模块构建结果等。
   > - 优化（Optimization）：在 Webpack 开始优化编译时执行，用于优化编译结果等。
   > - 生成资源（Asset Generation）：在 Webpack 开始生成资源时执行，用于生成最终的资源文件等。
   > - 输出（Output）：在 Webpack 输出最终的资源文件前执行，用于处理输出结果等。
