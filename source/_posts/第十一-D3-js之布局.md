---
title: 第十一 D3.js之布局之饼图
date: 2021-06-27 22:55:28
tags: D3.js
---

## 布局又名数据转化

**先来一个帅气的饼图**

{% asset_img pie.png %}

```typescript

let width = 600;
let height = 600;

let svg = d3.select('body').append('svg').attr('width',width).attr('height',height);

// init data
let dataSet:any = [
    ['小米', 60.8], ['三星', 58.4], ['联想', 47.3], ['苹果', 46.6],
    ['华为', 41.3], ['酷派', 40.1], ['其他', 111.5]
];

// set pie dataObject
let pieMain = d3.pie().value(d=>d[1]);
let pieData = pieMain(dataSet);
console.log('pieData',pieData);

// 画弧
let outerRadius = width/2.6;
let innerRadius = 0;

let arcMain = d3.arc()
                .innerRadius(innerRadius)
                .outerRadius(outerRadius);

// 定义七个颜色
let colors = ['red','blue','yellow','green','black','steelblue','gray'];

// 画个g容器
let arcs = svg.selectAll('g')
              .data(pieData)
              .enter()
              .append('g')
              .attr('transform',`translate(${width/2},${height/2})`)

arcs.append('path')
    .attr('fill',(d:any,i:number)=>colors[i])
    .attr('d',(d:any)=>arcMain(d));

// 添加文字
arcs.append('text')
    .attr('transform',(d:any)=>{
      let x:number = arcMain.centroid(d)[0] * 1.5; // arcMain.centroid(d) 是弧的中心点坐标
      let y:number = arcMain.centroid(d)[1] * 1.5;
      return `translate(${x},${y})`;
    })
    .attr('fill','white')
    .attr('text-anchor', 'middle')
    .attr('font-size',18)
    .text( (d: any)=> {
        let percent: number = d.value / d3.sum(dataSet, function (d: any) {
            return d[1];
        }) * 100;
        return percent.toFixed(2) + '%';
    });

// 添加链接弧外文字的直线元素
arcs.append('line')
    .attr('x1', function (d: any) {
        return arcMain.centroid(d)[0] * 2;
    })
    .attr('y1', function (d: any) {
        return arcMain.centroid(d)[1] * 2;
    })
    .attr('x2', function (d: any) {
        return arcMain.centroid(d)[0] * 2.2;
    })
    .attr('y2', function (d: any) {
        return arcMain.centroid(d)[1] * 2.2;
    })
    .attr('stroke', 'black');

// 添加文字
arcs.append('text')
    .attr('dy', '.35em')
    .attr('text-anchor', 'middle')
    .attr('transform', function (d: any) {
        let x: number = arcMain.centroid(d)[0] * 2.4;
        let y: number = arcMain.centroid(d)[1] * 2.4;
        return `translate(${x}, ${y})`
    })
    .text(function (d: any) {
        return d.data[0];
    })

```
