---
title: 浅深拷贝的实现
date: 2021-06-14 09:34:59
tags: javascript
---

# deepCopy&shallowCopy

## 浅拷贝 shallowCopy
```javascript
/**
 * 浅拷贝
 * @param {*} obj
 */
const shallowCopy = (obj) =>{
  if(typeof obj !== 'object')
    return obj;
  const newObj = obj instanceof Array ? []:{};
  for(let key in obj){
    if(obj.hasOwnProperty(key))
      newObj[key] = obj[key]
  }
  return newObj;
}

```
<br/>

## 深拷贝 deepCopy
```javascript
/**
 * 深拷贝
 * @param {*} obj
 */
const deepCopy = (obj) =>{
  if(typeof obj !== 'object')
    return obj;
  const newObj = obj instanceof Array ? []:{};
  for(let key in obj){
    if(obj.hasOwnProperty(key))
      newObj[key] =  typeof obj[key] === 'object' ? deepCopy(obj[key]): obj[key];
  }
  return newObj;
}

```

