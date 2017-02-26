---
title: javascript的this笔记
date: 2017-02-26 21:57:11
tags: javascript this
---
> 这几天在写贪吃蛇小游戏遇到了`this`到底指的是什么的问题，现将笔记总结如下。(参考MDN文档)

`this`是javascript中的一个关键字，this不能再执行期间赋值，它到底是什么取决于每次的函数调用方式。

# 全局上下文

通常时候当我们没有对函数进行封装，或者直接写一个纯粹的函数，这里的`this`指的就是全局对象，window。

# 函数上下文

在函数内部，`this`的值取决于函数是怎么调用。

## 对象方法中的this

    var o = {
      prop: 37,
      f: function() {
        return this.prop;
      }
    };
    console.log(o.f()); // logs 37
    
当o.f()被调用时，this被绑定到o对象。

## 构造函数的this

当一个函数被作为一个构造函数来使用(使用new关键字)，它的this与即将被创建的新对象绑定。

拿刚做的贪吃蛇项目来说。

    var Snake=function(options){
    		this.options=$.extend({},defaults,options);
            this.width=this.options.width;
            this.height=this.options.height;
            this.cellWidth=this.options.cellWidth;
            this.speed=this.options.speed;
            this.initialLength=this.options.initialLength;
            this.snakeLength=this.options.initialLength;
            this.$stage=this.options.stage;
            this.$eatNum=this.options.eatNum;
            this.$score=this.options.score;
            this.$recordTable=this.options.recordTable;
    
            this.record='';
            this.grid=new Array();
            this.snake=new Array();
            this.food=new Array();
            this.die=false;
            this.direction = 1;//向右  2下 3左  4 上
            this.nextDirection = '';
            this.snakeTimer='';
            this.num=0;
            this.score=0;
    
    	}
    	
上面是一个构造函数，当我new出一个实例,并调用函数时(函数定义在原型上)：
`var snake=new Snake({});
snake.init();`

之后的函数被调用时指的就是new出来的实例。

但是绑定事件时，this并不是new出来的实例，这是在写demo上遇到的问题。

## DOM事件处理函数中的this

当函数被用作事件处理函数时，它的this指向触发事件的元素。还不太懂这个知识点的时候，我在事件函数中调用原型的属性总是出错，因此解决问题的要点在怎么绑定上下文。

# 绑定上下文

绑定上下文的方法有：bind(),apply(),call()

`add.call(o, 5, 7); // 1 + 3 + 5 + 7 = 16
add.apply(o, [10, 20]); // 1 + 3 + 10 + 20 = 34`
使用 call 和 apply 函数的时候要注意，如果传递的 this 值不是一个对象，JavaScript 将会尝试使用内部 ToObject 操作将其转换为对象。


    function f(){
      return this.a;
    }
    var g = f.bind({a:"azerty"});
    console.log(g()); // azerty
    var o = {a:37, f:f, g:g};
    console.log(o.f(), o.g()); // 37, azerty
    
调用f.bind(someObject)会创建一个与f具有相同函数体和作用域的函数，但是在这个新函数中，this将永久地被绑定到了bind的第一个参数，无论这个函数是如何被调用的。

**jQuery的方法**
在项目中，我是通过使用jq的`$.proxy(function,this)`来把实例绑定到DOM事件的this的。

