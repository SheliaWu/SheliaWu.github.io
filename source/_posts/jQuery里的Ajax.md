﻿---
title: jQuery里的Ajax
date: 2017-02-27 22:28:16
tags: Ajax
---
> 在项目上自己很少有机会用到ajax，但是这是前端与后端发送接收数据必要用到的。在这里写下笔记，毕竟好记性不如烂笔头。

#`$.ajax()`#
    jQuery里面提供了一个`$.ajax()`的方法封装,通常的例子如下：
    
    $.ajax({
    type: "post",
    url: "http://...",
    data: {"username":"shelia","sex":"female"},
    success:function(result){
    ...
    }
    });
    
接下来是函数的几个主要参数：

 - url
要求为String类型的参数，（默认为当前页地址）发送请求的地址。
 - type 
要求为String类型的参数，请求方式（post或get）默认为get。注意其他http请求方法，例如put和delete也可以使用，但仅部分浏览器支持。
 - timeout 
要求为Number类型的参数，设置请求超时时间（毫秒）。此设置将覆盖$.ajaxSetup()方法的全局设置。
 - async 
要求为Boolean类型的参数，默认设置为true，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为false。注意，同步请求将锁住浏览器，用户其他操作必须等待请求完成才可以执行。
 - cache
要求为Boolean类型的参数，默认为true（当dataType为script时，默认为false），设置为false将不会从浏览器缓存中加载请求信息。
 - data 
要求为Object或String类型的参数，发送到服务器的数据。如果已经不是字符串，将自动转换为字符串格式。get请求中将附加在url后。防止这种自动转换，可以查看　　processData选项。对象必须为key/value格式，例如{foo1:"bar1",foo2:"bar2"}转换为&foo1=bar1&foo2=bar2。如果是数组，JQuery将自动为不同值对应同一个名称。例如{foo:["bar1","bar2"]}转换为&foo=bar1&foo=bar2。
 - dataType 
要求为String类型的参数，预期服务器返回的数据类型。如果不指定，JQuery将自动根据http包mime信息返回responseXML或responseText，并作为回调函数参数传递。可用的类型如下：
xml：返回XML文档，可用JQuery处理。
html：返回纯文本HTML信息；包含的script标签会在插入DOM时执行。
script：返回纯文本JavaScript代码。不会自动缓存结果。除非设置了cache参数。注意在远程请求时（不在同一个域下），所有post请求都将转为get请求。
json：返回JSON数据。
jsonp：JSONP格式。使用SONP形式调用函数时，例如myurl?callback=?，JQuery将自动替换后一个“?”为正确的函数名，以执行回调函数。
text：返回纯文本字符串。
 - complete
要求为Function类型的参数，请求完成后调用的回调函数（请求成功或失败时均调用）。参数：XMLHttpRequest对象和一个描述成功请求类型的字符串。
          function(XMLHttpRequest, textStatus){
             this;    //调用本次ajax请求时传递的options参数
          }
 - success：要求为Function类型的参数，请求成功后调用的回调函数，有两个参数。
         (1)由服务器返回，并根据dataType参数进行处理后的数据。
         (2)描述状态的字符串。
         function(data, textStatus){
            //data可能是xmlDoc、jsonObj、html、text等等
            this;  //调用本次ajax请求时传递的options参数
         }
 - error
要求为Function类型的参数，请求失败时被调用的函数。该函数有3个参数，即XMLHttpRequest对象、错误信息、捕获的错误对象(可选)。ajax事件函数如下：
       function(XMLHttpRequest, textStatus, errorThrown){
          //通常情况下textStatus和errorThrown只有其中一个包含信息
          this;   //调用本次ajax请求时传递的options参数
       }
 - contentType
要求为String类型的参数，当发送信息至服务器时，内容编码类型默认为"application/x-www-form-urlencoded"。该默认值适合大多数应用场合。

同样的方法有`$.post`和`$.get()`。这两个方法是针对post和get方式的，两者是将ajax方法简化了，两者的使用方式如下：

- `$.get(url,[data],[callback])`
- `$.post(url,[data],[callback],[type])`