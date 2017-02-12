---
title: AngularJS学习笔记：Scope的理解
date: 2017-02-12 19:50:26
tags: AngularJS
---
在学习AngularJS的时候，说到Scope(作用域) 是应用在 HTML (视图) 和 JavaScript (控制器)之间的纽带。Scope 是一个对象，有可用的方法和属。但还是不太搞得明白，感觉有点像是JS的原型继承。所以阅读了相关文章，总结如下。

# Scope的本质

Scope实际上就是一个JS对象，controller和view都可以访问它，利用它可以在两者之间传递信息。

# $rootScope

Angular 应用启动并生成视图时,会将根 ng-app 元素与 $rootScope 进行绑定.$rootScope 是所有 $scope 的最上层对象,可以理解为一个 Angular 应用中得全局作用域对象,所以为它附加太多逻辑或者变量并不是一个好主意,和污染 Javascript 全局作用域是一样的.

# 子作用域

每当我们讨明确创建一个$scope对象，就要给DOM元素安上一个controller对象。
代码如下：

    <div ng-controller="MyController">
     {{ person.name }}
    </div>
    
ng-controller指令给所在的DOM元素创建了一个新的$scope 对象，并将这个$scope 对象包含进外层DOM元素的$scope 对象里。在上面的例子里，这个外层DOM元素的$scope 对象，就是$rootScope 对象。这个scope链是这样的：`$scope(mycontroller))->$rootScope`

所有scope都遵循原型继承（prototypal inheritance），这意味着它们都能访问父scope们。对任何属性和方法，如果AngularJS在当前scope上找不到，就会到父scope上去找，如果在父scope上也没找到，就会继续向上回溯，一直到$rootScope 上。是不是很像原型链上的查找。

但也有例外，指令的作用域有些不同，下一节将会记录到。