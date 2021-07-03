---
title: 第十二 D3.js布局之力导向图
date: 2021-06-28 09:48:59
tags: D3.js
---

## 力导向图

力导向图主要应用于复杂网络可视化展示。他的好处是减少了布局中边的交叉，并且尽量保持一样长。运用阻尼衰减，能够在拖拉过程中快速稳定回来。

**基础力导向图**

{% asset_img aaa.png %}

```javascript
const width = 600;
const height = 600;
const svg = d3.select('body').append('svg').attr('width',width).attr('height',height).attr('viewBox', `0 0 ${width} ${height}`);
const g = svg.append("g").attr("transform","translate(50,50)");

// 点数据
const nodes = [
  {name:"0"},
  {name:"1"},
  {name:"2"},
  {name:"3"},
  {name:"4"},
  {name:"5"},
  {name:"6"},
  {name:"7"},
  {name:"8"}
];

// 连线数据
const edges = [
  {source:0,target:4,value:1.3},
  {source:4,target:5,value:1},
  {source:4,target:6,value:1},
  {source:4,target:7,value:1},
  {source:1,target:6,value:2},
  {source:2,target:5,value:0.9},
  {source:3,target:7,value:1},
  {source:5,target:6,value:1.6},
  {source:6,target:7,value:0.7},
  {source:6,target:8,value:2}
];


// 力导向图对象
var forceSimulation = d3.forceSimulation()
    		.force("link",d3.forceLink())
    		.force("charge",d3.forceManyBody())
    		.force("center",d3.forceCenter());

// 生成节点数据
forceSimulation.nodes(nodes)
    		.on("tick",ticked);


// 生成边数据
forceSimulation.force("link")
    		.links(edges)
    		.distance(function(d){  // 每一边的长度
    			return d.value*100;
    		});

// 设置中心点位置
forceSimulation.force("center")
    		.x(width/2)
    		.y(height/2);


// 绘制边
const links = g.append("g")
    		.selectAll("line")
    		.data(edges)
    		.enter()
    		.append("line")
            .classed("link", true);

// 绘制node的g对象
const gs = g.selectAll(".circleText")
    		.data(nodes)
    		.enter()
    		.append("g")
    		.attr("transform",function(d,i){
    			var cirX = d.x;
    			var cirY = d.y;
    			return "translate("+cirX+","+cirY+")";
    		})
    		.call(d3.drag()
    			.on("start",started)
    			.on("drag",dragged)
    			.on("end",ended)
    		);

// 绘制节点
gs.append("circle")
  .attr("r",10)
  .classed("node", true)
  .classed("fixed", d => d.fx !== undefined);

// 绘制文字
gs.append("text")
  .attr("x",0)
  .attr("y",0)
  .attr("dy",6)
  .attr("dx",-5)
  .text((d)=>d.name);

function ticked(){
  links
    .attr("x1",function(d){return d.source.x;})
    .attr("y1",function(d){return d.source.y;})
    .attr("x2",function(d){return d.target.x;})
    .attr("y2",function(d){return d.target.y;});
  
  gs.attr("transform",function(d) { return "translate(" + d.x + "," + d.y + ")"; });
}

function started(d){
  if(!d3.event.active){
    forceSimulation.alphaTarget(0.8).restart(); // 设置衰减系数，对节点位置移动过程的模拟，数值越高移动越快，数值范围[0，1]
  }
  d.fx = d.x;
  d.fy = d.y;
}

function dragged(d){
  d.fx = d3.event.x;
  d.fy = d3.event.y;
}

function ended(d){
  if(!d3.event.active){
    forceSimulation.alphaTarget(0);
  }
  d.fx = null;
  d.fy = null;
}
```




