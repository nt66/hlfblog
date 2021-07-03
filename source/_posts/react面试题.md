---
title: react面试题
date: 2021-06-30 10:34:27
tags: react 面试
---

## React 面试题

### 1.React中setState函数第二个参数的作用
> 答：回调函数，异步调用

### 2.React中setState如何实现同步
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
> 原理是原生事件()可以绕过react状态机制，达到事件同步效果。

### 3.React中setState经历了什么
> 


