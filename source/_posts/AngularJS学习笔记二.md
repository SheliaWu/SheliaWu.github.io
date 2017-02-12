---
title: AngularJS：指令作用域详解
date: 2017-02-12 23:28:26
tags: AngularJS
---
>本文摘录自[玩转AngularJS的作用域][1]

每当一个指令被创建时，都会有一个选择，是继承自己的父作用域还是创建一个新的自己的作用域。AngularJS指令的作用域有三种选择，分别是false，true，{}，默认情况下是false。

# scope=false

这种情况表示，在指令模板中直接使用副作用域中的变量。

示例代码：

    angular.module("MyApp", [])
        .controller("MyController", function ($scope) {
            //J1 这里我们在作用域里初始化两个变量
        $scope.name = "dreamapple";
        $scope.age = 20;
            //J2 创建一个方法，修改我们创建的对象的年龄
        $scope.changeAge = function () {
            $scope.age = 22;
        }
    })
            //J3 创建我们的指令，指令名字为"myDirective"
    
        .directive("myDirective", function () {
        var obj = {
            //J4   指令的声明模式为 "AE" 属性和元素
            restrict: "AE",
            //J5   指令继承父作用域的属性和方法
            scope: false,
            replace: true,
            template: "<div class='my-directive'>" +
                "<h3>下面部分是我们创建的指令生成的</h3>" +
                "我的名字是：<span ng-bind='name'></span><br/>" +
                "我的年龄是：<span ng-bind='age'></span>" +
                "<input type='text' ng-model='name'>"+
                " </div>"
        }
        return obj;
    });

当我们在指令更改父作用域的方法和属性，父作用域绑定的数据也发生了响应的变化。

# scope=true

当把scope属性设置为true时，这表明我们创建的指令要创建一个新的作用域，这个作用域继承自我们的父作用域。但是改变指令内的数据，只有指令的作用域发生了变化，父作用域数据并没有发生变化

**注意：**

- 当我们将scope设置为true的时候，我们就新创建了一个作用域，只不过这个作用域是继承了我们的父作用域；我觉得可以这样理解，我们新创建的作用域是一个新的作用域，只不过在初始化的时候，用了父作用域的属性和方法去填充我们这个新的作用域。它和父作用域不是同一个作用域。
- 当我们将scope设置为false的时候,我们创建的指令和父作用域（其实是同一个作用域）共享同一个model模型，所以在指令中修改模型数据，它会反映到父作用域的模型中。

这就是两者继承父作用域的区别。

# scope={}

我们将scope设置为{}时，意味着我们创建的一个新的与父作用域隔离的新的作用域，这使我们在不知道外部环境的情况下，就可以正常工作，不依赖外部环境。

实例代码如下：

        angular.module("MyApp", [])
        .controller("MyController", function ($scope) {
        $scope.name = "dreamapple";
        $scope.age = 20;
        $scope.changeAge = function(){
            $scope.age = 0;
        }
    })
        .directive("myDirective", function () {
        var obj = {
            restrict: "AE",
            scope: {
                name: '@myName',
                age: '=',
                changeAge: '&changeMyAge'
            },
            replace: true,
            template: "<div class='my-directive'>" +
                "<h3>下面部分是我们创建的指令生成的</h3>" +
                "我的名字是：<span ng-bind='name'></span><br/>" +
                "我的年龄是：<span ng-bind='age'></span><br/>" +
                "在这里修改名字：<input type='text' ng-model='name'><br/>" +
                "<button ng-click='changeAge()'>修改年龄</button>" +
                " </div>"
        }
        return obj;
    });
    
    
我们使用了隔离的作用域，不代表我们不可以使用父作用域的属性和方法。我们可以通过向scope的{}中传入特殊的前缀标识符（即prefix），来进行数据的绑定。在创建了隔离的作用域，我们可以通过@,&,=引用应用指令的元素的属性。但是注意要使用驼峰命名法。

## @
这是一个单项绑定的前缀标识符
使用方法：在元素中使用属性，好比这样`<div my-directive my-name="{{name}}"></div>`，注意，属性的名字要用-将两个单词连接，因为是数据的单项绑定所以要通过使用{{}}来绑定数据。
## &
这是一个双向数据绑定前缀标识符
使用方法：在元素中使用属性，好比这样`<div my-directive age="age"></div>`,注意，数据的双向绑定要通过=前缀标识符实现，所以不可以使用{{}}。
## =
这是一个绑定函数方法的前缀标识符
使用方法：在元素中使用属性，好比这样`<div my-directive change-my-age="changeAge()"></div>`，注意，属性的名字要用-将多个个单词连接。


  [1]: https://segmentfault.com/a/1190000002773689