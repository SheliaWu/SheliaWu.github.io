---
title: 响应式web设计
date: 2017-02-09 10:39:24
tags: HTML5 CSS3
---

> 本文摘自[响应式网页设计][1]

# 什么是响应式设计

响应式设计网页是由Ethan Marcotte提出的一个概念，指的是网页应该能根据不同分辨率的设备自适应和自动缩放等，当然也应该不仅限于此。

响应式设计应该做到两点，响应式布局和响应式内容（图片、多媒体）。

# 响应式布局

我们需要兼容不同屏幕分辨率、清晰度以及屏幕定向方式竖屏（portrait）、横屏（landscape），怎样才能让一种设计方案满足所有。

这时候布局应该是一种弹性的栅格布局，不同尺寸下弹性适应。这里指的应该是Bootstrap的流式布局。

## Meta标签定义

使用viewport meta 标签在手机浏览器上控制布局

    <meta name=”viewport” content=”width=device-width, initial-scale=1, maximum-scale=1″>
    
**注意：**很多人常常使用initial-scale=1到非响应式网站上，这会让网站以100%宽度渲染而不会自动缩放，用户需要手动移动页面或者缩放。最差的是和initial-scale=1同时使用user-scalable=no或maximum-scale=1，这将使你的网站不能被缩放——用户不能放大/缩小网页来看到全部的内容。

## 使用媒体查询

- 设备类型
all所有设备
screen 电脑显示器
print打印用纸或打印预览视图
handheld便携设备
tv电视机类型的设备
speech语意和音频盒成器
braille盲人用点字法触觉回馈设备
embossed盲文打印机
projection各种投影设备
tty使用固定密度字母栅格的媒介，比如电传打字机和终端

- 设备特性
width浏览器宽度
height浏览器高度
device-width设备屏幕分辨率的宽度值
device-height设备屏幕分辨率的高度值
orientation浏览器窗口的方向纵向还是横向，当窗口的高度值大于等于宽度时该特性值为portrait，否则为landscape
aspect-ratio比例值，浏览器的纵横比
device-aspect-ratio比例值，屏幕的纵横比

媒体查询的例子如下：

    /*for 240px width screen*/
    @media only screen and (max-device-width:240px){
    selector{...}
    }
    /*for 320px width screen*/
    @media only screen and (min-device-width:241px) and (max-device-width:320px){
    selector{...}
    }
    
## 对于表格的处理

- 隐藏不重要的数据列

 实现方法：

        media only screen and (max-width: 800px) {
    	table td:nth-child(2), 
    	table th:nth-child(2) {display: none;}
    }
    
- 多列横向变2列纵向

    实现方法：
    <thead>定位隐藏，<td>变块元素，并绑定对应<th>列名，然    后用伪元素的content:attr(data-th)实现<th>

- 固定首列，剩余列横向滚动

    实现代码：
    
    > thead {float:left;}
    tbody {display:block;width:auto;overflow-x:auto;}
    
# 响应式图片

## 处理原理

浏览器获取用户终端的屏幕尺寸、分辨率逻辑处理后输出适应的图片，如屏幕分辨率320*480，那么我们匹配给它的是宽度应小于320px的图片。如果终端屏幕的DPI(devicepixels)DPI详解值很高，也就是高清屏，那么我们就得输出2倍分辨率的图形(宽:640px)；以保证在高清屏下图形的清晰度。

## 解决方案

其实W3C已经有一个用于响应式图形的草案：新定义标签`<picture>`，因为它还只是草案，目前还没有支持的浏览器，期待在不久的未来我们能用上。虽然目前不支持，但我们还是来了解下，为之后的内容做个铺垫。

`<picture>`是一个图形element，内容由多个源图组成，并由CSS Media Queries来适配出合理图形，代码规范如下：


    <picture width="500" height="500">
       <source media="(min-width: 640px)" srcset="large-1.jpg 1x, large-2.jpg 2x">
       <source media="(min-width: 320px)" srcset="med-1.jpg 1x, med-2.jpg 2x">
       <source srcset="small-1.jpg 1x, small-2.jpg 2x">
       <img src="small-1.jpg" alt="">
       <p>Accessible text</p>
       <!-- Fallback content-->
       <noscript>
          <img src="external/imgs/small.jpg" alt="Team photo">
       </noscript>
    </picture>

source: 一个图片源；media: 媒体查询，用于适配屏幕尺寸；srcset: 图片链接，1x适应普通屏，2x适应高清屏；<noscript/>: 当浏览器不支持脚本时的一个替代方案；<img/>: 初始图片；另外还有一个无障碍文本，类似<img/>的alt属性。

`<picture>`目前还不支持，但它的原理我们是可借鉴的，所以就诞生了一个用于图片响应式处理的类库Picturefill

    <span data-picture data-alt="图片描述文本">
        <span data-src="small.jpg"></span>
        <span data-src="medium.jpg"     data-media="(min-width: 400px)"></span>
        <span data-src="large.jpg"      data-media="(min-width: 800px)"></span>
        <span data-src="extralarge.jpg" data-media="(min-width: 1000px)"></span>
        <!-- 浏览器不支持JS时的备用方案. -->
        <noscript>
            <img src="external/imgs/small.jpg" alt="图片描述文本">
        </noscript>
    </span>


其原理就是JS获取Source的源以及CSS Media Queries规则，输出适应图片， 逻辑细节这里不再解析，感兴趣的可查看其JS代码，逻辑不是很复杂，也可以自己封装一个类库，以适用于自身产品，例如图片加载失败的替代方案。

## 高分辨率(DPI)下的响应式处理

  > SVG：优点可承载色彩丰富、设计复杂图形，且渲染不会出现边缘不顺滑；缺点是IE的支持不完美，在我大中华这是硬伤。
  
   > Icon fonts：支持多浏览器，图形颜色大小的修改成本低，易于维护；图形表现单一，不支持色彩丰富且复杂的图形，IE6渲染有毛边。
   
   > -webkit-image-set:只支持单个图形的适配，不利于图形合并，兼容不完美（Safari 6+, Chrome 21+）

JS检测：`var retina = window.devicePixelRatio > 1;`

CSS Media Query:

    @media (-webkit-min-device-pixel-ratio: 2), /* Webkit-based browsers */
           (min--moz-device-pixel-ratio: 2),    /* Older Firefox browsers (prior to Firefox 16) */
           (min-resolution: 2dppx),             /* The standard way */
           (min-resolution: 192dpi)             /* dppx fallback */

由于高清屏的特性，1px是由2×2个像素点来渲染

  [1]: https://isux.tencent.com/responsive-web-design.html
