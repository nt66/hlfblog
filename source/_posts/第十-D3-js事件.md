---
title: 第十--D3.js事件
date: 2021-06-27 20:29:55
tags: D3.js
---

## 事件

**d3的事件可分为：鼠标事件|键盘事件|触屏事件|拖拽|缩放**

### 鼠标事件比较简单，用on方法监听即可
```typescript
let rect = svg.append('rect')
              .attr('fill','steelblue')
              .attr('x',200)
              .attr('y',200)
              .attr('width',100)
              .attr('height',30)
              .on('mouseover',(d,i)=>{
                  d3.select(this) // 注意这里，只有多个元素才能用。 单个直接用rect对象
                    .transition()
                    .duration(500)
                    .attr('fill','red');
              });
```

API | 说明
:-  | :-
click | 点击
mouseover | 略了吧
mouseout | 略了吧
mousemove | 略了吧
mousedown | 略了吧
mouseup | 略了吧
dbclick | 略了吧

<br />

### 键盘事件
**keydown|keypress|keyup**

```typescript
d3.select('body')
  .on('keydown',()=>{
    // d3.event.keyCode  键ID
    rect.attr('fill','yellow')
  })
```

API | 说明
:-  | :-
keydown | 按下
keyup | 弹起
keypress | 特殊键盘按下

<br />

其他事件如 **drag** 等先略过