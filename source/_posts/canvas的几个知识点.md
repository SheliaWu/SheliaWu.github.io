---
title: canvas的几个知识点
date: 2017-02-20 22:08:14
tags: HTML5
---
今天重新复习了HTML5，对canvas标签理解加深了一点，有几个知识点想记下来以免忘记。

# 绘制矩形

绘制矩形 context.fillRect(x,y,width,height) strokeRect(x,y,width,height)

x:矩形起点横坐标（坐标原点为canvas的左上角，当然确切的来说是原始原点，后面写到变形的时候你就懂了，现在暂时不用关系）

y:矩形起点纵坐标

width:矩形长度

height:矩形高度

# 绘制圆弧

圆弧context.arc(x, y, radius, starAngle,endAngle, anticlockwise)

x:圆心的x坐标

y:圆心的y坐标

straAngle:开始角度

endAngle:结束角度

anticlockwise:是否逆时针（true）为逆时针，(false)为顺时针

ps：经过试验证明书本上ture是顺时针，false是逆时针是错误的，而且无论是逆时针还是顺时针，角度都沿着顺时针扩大。

# 路径

这是今天理解的重点。

- 系统默认在绘制第一个路径的开始点为beginPath
- 如果画完前面的路径没有重新指定beginPath，那么画第其他路径的时候会将前面最近指定的beginPath后的全部路径重新绘制
- 每次调用context.fill（）的时候会自动把当次绘制的路径的开始点和结束点相连，接着填充封闭的部分。

# 渐变

1.线性渐变 var lg= context.createLinearGradient(xStart,yStart,xEnd,yEnd)
2.线性渐变颜色lg.addColorStop(offset,color)

xstart:渐变开始点x坐标

ystart:渐变开始点y坐标

xEnd:渐变结束点x坐标

yEnd:渐变结束点y坐标

offset:设定的颜色离渐变结束点的偏移量(0~1)

color:绘制时要使用的颜色