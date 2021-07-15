---
title: react面试题
date: 2021-06-30 10:34:27
tags: react 面试
---

## React 面试题

### 1.React中setState函数第二个参数的作用
> 答：回调函数，异步调用

### 2.React中setState如何实现同步？
```javascript
// 方法一：传递一个函数
this.setState((prevState, props) => ({
    count: prevState.count + 1
  }));

// 方法二 setTimeout
componentDidMout(){
  setTimeout(()=>{
    this.setState({number:3});
  })
}
```
> 原理是原生事件可以绕过react状态机制，达到事件同步效果。

### 3.React中setState经历了什么？
{% asset_img setState原理.jpeg %}

### 4.说说filber架构
> js和页面渲染线程有互斥性（或者说是阻塞），所以往往会出现js运算时间过长导致页面卡顿。因此React提出了filber（切片）的概念：把js运算左侧切片，js运行一段时间后调度器把线程控制权交给页面，页面渲染完成后再交回给js；有6个优先级标记任务的等级，原则是 willmount willreceiprops  shouldComponentUpdate willupdate 等阶段的任务是可以被打断的；剩下的didmount didupdate 是不能被打断的。
### 5.说说hooks
> a
### 6.react18新特性

### 7.react的合成事件



