---
title: svg之path详解
date: 2021-06-23 15:34:37
tags: svg
---

## <div id="class02-01"> path </div>

path元素是SVG基本形状中最强大的一个，它不仅能创建其他基本形状，还能创建更多其他形状。你可以用path元素绘制矩形（直角矩形或者圆角矩形）、圆形、椭圆、折线形、多边形，以及一些其他的形状，例如贝塞尔曲线、2次曲线等曲线。

使用方法
```javascript
<path d="M10 10 H 90 V 90 H 10 L 10 10" stroke="red" fill="transparent" />
```
最难理解的就是属性d里面的命令。
<font color="red">一般来说大写代表绝对定位，小写表相对定位 </font>

属性 | 说明
:- | :-
M  | move to 
L  | line to
H  | 横向画线
V  | vertical 纵向画线
C  | curveto 曲线
S  | smooth curveto 三次贝塞尔曲线简写,可省略第一个点
Q  | quadratic curveto 两次贝塞尔
T  | 两次贝塞尔曲线简写，可省略第一个点
A  | 弧线 rx（x轴半径） ry（y轴半径） x-axis-rotation（旋转角度） large-arc-flag（角度大小） sweep-flag（弧线方向） x y ,
Z  | 终点


```javascript
<path d="M10 10 L 100 100 L 200 200 Z" stroke="red" fill="transparent" /> // 画直线

<path d="M10 10 C 100 10, 100 200,300 200" stroke="red" fill="transparent" /> // 三次贝塞尔 第一、二两个是控制点，最后一个是终点

<path d="M10 10 C 100 10, 100 200,300 200 S 350 200, 400 180" stroke="red" fill="transparent" /> // S是三次贝塞尔简写 S前面如果是C或者S 那S的第一个控制点就是前面的第二个控制点的对称点

<path d="M10 10 Q 100 10, 100 200" stroke="red" fill="transparent" /> // Q是二次贝塞尔

<path d="M10 10 Q 100 10, 100 200 T 300 200" stroke="red" fill="transparent" /> // T是二次贝塞尔的简写，道理和S类似，省略了第一个控制点

```

