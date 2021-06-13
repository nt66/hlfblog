---
title: Mongodb操作 之 ES5封装
date: 2021-02-10 22:39:54
tags: ['mongodb','node','express']
---
继mongodb ES6封装之后，突然想着用ES5的返祖写法去实现一下^_^。其实原理差不多，区别主要是前者用的promise，后者用的callback。仅此而已。

{% asset_img es5.png '返祖' %}

<!--more-->
# 文件目录
```javascript
  db                //db封装类
    |--index.js    
  app.js           //入口文件
  node_modules
  package.json
```
</br>
<!--more-->

# db/index.js封装
```javascript
 /**
 * mongodb crud callback封装
 */
const { MongoClient } = require('mongodb');
const URL = 'mongodb://127.0.0.1:27017/';
const DBNAME = 'dt';

/**
 * 连接数据库
 * @param {String} collectionName 回调函数
 */
const dbConnect = (callback) =>{
  MongoClient.connect(URL,(err,client)=>{
    if(err){
      console.log('mongodb connect err');
      return;
    }
    const db = client.db(DBNAME);
    callback(db);
    client.close();
  })
}

/**
 * 查找
 * @param {string} collectionName 表名
 * @param {*} json 查找条件
 */
exports.find = (collectionName, json, callback)=>{
  dbConnect((db)=>{
    const collection = db.collection(collectionName);
    collection.find(json).toArray((err,data)=>{
      callback(err,data);
    })
  })
}

/**
 * 修改
 * @param {string} collectionName 表名
 * @param {*} json1 查找条件
 * @param {*} json1 更新数据
 */
exports.update = (collectionName, json1, json2, callback)=>{
  dbConnect((db)=>{
    const collection = db.collection(collectionName);
    collection.updateOne(json1, { $set: json2 }, (err,result)=>{
      callback(err,result);
    })
  })
}
```
</br>
</br>



