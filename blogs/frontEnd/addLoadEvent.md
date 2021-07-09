---
title: 理解addLoadEvent函数
date: 2021-01-28
tags:
 - JavaScript
 - DOM编程艺术
categories: 
 - frontEnd
---
```onload```事件是```HTML DOM Event```对象的一个**属性**，又叫**事件句柄**（Event Handlers），它会在页面或图像<font color='red'>加载完成后</font>（注意是加载完成后）<font color='red'>立即发生</font>。

```window.onload = func```的作用就是在页面加载完成后将```func```函数绑定到```onload```事件上并执行。如果页面加载完成之后，只需要执行一个函数```func```，那么只用```window.onload = func```也就可以了，但是如果需要执行**两个甚至多个**函数呢？

直接调用两次onload不就行了:

```js
window.onload = firstfunction;
window.onload = secondfunction;
```
这么做的话，**只有**```secondfunction```会被绑定，因为前面的值被后面的值**覆盖**了。那么该怎么办？

将两个函数合并到一个函数当中不就行了，匿名函数发挥作用的时候到了：
```js
window.onload = function() {
    fristfunction;
    secondfunction;
}
```
 不过，它也只能绑定两个函数。还好，大神们早已解决了这个问题。**西蒙·威利森** (Simon Willison)——**jQuery框架的开发者**之一编写了下面的```addLoadEvent```函数：
```js
function addLoadEvent(func) {
    var oldonload = window.onload;//将现有的事件处理函数的值存入变量中
    if (typeof window.onload != 'function') {
        window.onload = func;//如果这个事件处理函数没有绑定任何函数，就把新函数添加给它
    } else {
        window.onload = function() {
            oldonload();
            func();//如果已经绑定了函数，就把新函数追加到现有指令的末尾
      }
    }
}
```
 然后，不管页面加载完成后要执行多少个函数，只要调用这个函数就可以了：
```js
addLoadEvent(firstfunction);
addLoadEvent(secondfunction);
addLoadEvent(thirdfunction);
...
```

**附：相关概念**
- 1. 支持onload事件的 HTML 标签有```<body>, <frame>, <frameset>, <iframe>, <img>, <link>, <script>```支持该事件的 JavaScript 对象有```image（图像）, layer, window（整个页面）```
- 2. 事件句柄（Event Handlers），可以在某个事件发生时通过一个事件句柄对某个元素进行操作。
    事件是可以被控件识别的操作，如按下确定按钮，选择某个单选按钮或者复选框。每一种控件有自己可以识别的事件，如窗体的加载、单击、双击等事件，编辑框（文本框）的文本改变事件，等等。
- 3. ```HTML DOM Event``` 对象代表事件的状态，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标按钮的状态等。事件通常与函数结合使用，<font color='red'>函数不会在事件发生前被执行</font>（这句很重要）。


> 参考资料：
> JavaScript DOM编程艺术 by Jeremy Keith

<p align="right">2021年1月28日 海口</p>