﻿---
title: setTimeout在循环中
date: 2017-02-19 20:09:30
tags: javascript
---
> 考察了异步，作用域，闭包知识点。

这个问题源于在知乎上碰到的一篇文章，文章传送门在这[如果你想靠前端技术还房贷，你不能连这个都不会][1]。之前看了《javascript高级程序设计》这本书，想着自己应该能理解，一复习发现还是半懂不懂那就是不懂，还是需要多敲代码多练习。

题目是这样子的：

        // 请问输出什么
    for (var i = 0; i < 10; ++i) {
        setTimeout(function () {console.log(i)}, 0);
    }

**第一问，这段代码输出什么？第二问，如果想让这段代码输出0123456789，应该怎么修改？**

这题考察了闭包和setTimeout()的知识点。

# 闭包的理解
先说我对闭包的理解吧，闭包就是在我们定义一个外部函数时，又定义一个子函数，如果这个内部函数引用父函數作用域的變量，他就是一个闭包。因为javascript的“链式作用域”结构，子对象会向上寻找所有父对象的变量，所以子函数能访问父级函数的变量。

# setTimeout（）的理解
setTimeout是异步的。
正确的理解setTimeout的方式(注册事件)：
有两个参数，第一个参数是函数，第二参数是时间值。
调用setTimeout时，把函数参数，放到事件队列中。等主程序运行完，再调用。
就像我们给按钮绑定时间一样，事件绑定了不会执行，只有去触发它了之后才会。而setTimeout()的执行条件就是等主程序执行完毕。那么时间参数怎么理解。我们可以理解为时间参数之后，再把setTimeout的function放入事件队列中，如果此时队列为空，那么就直接调用fn。如果前面还有其他的事件，那就等待。

在上面的两个理解之下，我们再去看题目。

# 答案解析
> 不知道是否百分百准确。

在执行这个函数时，在每一次循环时把setTimeout函数放进了事件队列中，但是主程序并没有执行完毕，在第十次放入事件队列后，主程序执行完毕开始依次执行setTimeout函数。因为setTimeout的函数参数引用了变量i的，所以i不会在主程序执行完毕后被垃圾回收，内存不会销毁，这时去寻找i的值`i=10`，因为var没有块级作用域，输出10，因此会输出十次10。

怎样会输出0-9呢？我尝试了，有两种解决方法：
(1)使用具有块级作用域的let定义变量

    for(let i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);  
    }, 1000);
}


(2)自执行匿名函数

    for(var i = 0; i < 10; i++) {
    (function(x){setTimeout(function() {
        console.log(x);  
    }, 1000);})(i)
}
外部的匿名函数会立即执行，并把 i 作为它的参数，此时函数内 x 变量就拥有了 i 的一个拷贝。
  [1]: https://www.zhihu.com/collection/142522312