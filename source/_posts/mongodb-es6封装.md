---
title: Mongodb操作 之 ES6封装
date: 2021-02-03 18:59:54
tags: ['mongodb','node','express']
---
node操作mongodb过程中，一套动作下来颇为繁琐。如何化繁为简，提高代码复用率？
==>封装！！！


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
   * mongodb crud_es6
   */
  const MongoClient = require('mongodb').MongoClient;
  const URL = 'mongodb://127.0.0.1:27017/';
  const DBNAME = 'dt';

  class DB {
    // 单例模式
    static getInstance() {
      if (!DB.instance) {
        DB.instance = new DB();
      }
      return DB.instance;
    }

    // 构造函数
    constructor() {
      this.dbClient = null;
      this.connect();
    }

    // 连接 (如果需要动态指定url和dbname，可以在connect中传入参数)
    connect() {
      return new Promise((resovle, reject) => {
        MongoClient.connect(URL, (err, client) => {
          if (err) {
            reject(err)
          } else {
            this.dbClient = client.db(DBNAME);
            resovle(this.dbClient);
          }
        })
      });
    }

    /**
     * 查找
     * @param {String} collectionName 表名
     * @param {*} json 查找条件
     */
    find(collectionName, json) {
      return new Promise((res, rej) => {
        this.connect().then(db => {
          const collection = db.collection(collectionName);
          collection.find(json).toArray((err, data) => {
            if (err) {
              rej(err);
              return;
            }
            res(data);
          })
        })
      })
    }

    /**
     * 插入数据
     * @param {String} collectionName 表名
     * @param {*} json 插入数据
     */
    insert(collectionName, json) {
      return new Promise((resolve, reject) => {
        this.connect().then(db => {
          db.collection(collectionName).insertOne(json, (err, result) => {
            if (err) {
              reject(err);
              return;
            }
            resolve(result);
          });
        });
      });
    }

    /**
     * 更新
     * @param {String} collectionName 表名
     * @param {*} json1 查找条件
     * @param {*} json2 更新的数据
     */
    update(collectionName, json1, json2) {
      return new Promise((res, rej) => {
        this.connect().then(db => {
          db.collection(collectionName).updateOne(json1, { $set: json2 }, (err, result) => {
            if (err) {
              rej(err);
              return;
            }
            res(result);
          })
        })
      });
    }

    /**
     * 删除
     * @param {String} collectionName 表名
     * @param {*} json 查找条件
     */
    remove(collectionName, json) {
      return new Promise((resolve, reject) => {
        this.connect().then(db => {
          db.collection(collectionName).removeOne(json, (err, result) => {
            if (err) {
              reject(err);
              return;
            }
            resolve(result);
          });
        });
      });
    }
  }

  module.exports = DB.getInstance();
```

</br>

# express调用
```javascript
  const express = require('express');
  const DB = require('./db');

  const app = express();

  app.get('/es6', async (req, res) => {
    const userInfo = await DB.find('user', {'name': 'hlf'}); // 等于
    console.log('userInfo', userInfo);
    res.send('user information:<br/>' + JSON.stringify(userInfo))
  });

  app.listen(3000, () => {
    console.log('listening port on 3000');
  });
```
</br>
</br>



