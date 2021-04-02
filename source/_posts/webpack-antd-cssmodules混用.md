---
title: webpack-antd-cssmodules混用
date: 2021-04-02 15:35:09
tags: ['webpack']
---
年前立了个flag，说是一周一篇博客。。。真是啪啪打脸。知耻而后勇，记录一下今天遇到的小问题吧

# 前言

webpack做开发和生产构建之时，常会用到cssModules。使用cssModules最大的好处是：

**样式的作用域隔离，避免了css命名冲突的问题**

这确实是多人协同开发的福音。但是，当我们配置了cssModules之后，一些第三方类库的样式文件就会解析失败。。。。如antd


# 事情经过
今天新开了一个ts项目框架。引入antd组件的时候发现样式不生效

{% asset_img 1.jpg %}

## planA 
入口less文件中引入antd样式文件
```javascript
@import '~antd/dist/antd.css';
```
然鹅 not work again...
<br/>

## planB 
配置webpack
```javascript
{
        test: [/\.ts$/, /\.tsx$/],
        exclude: /node_modules/,
        use: [
          {
            loader: 'babel-loader',
            options: {
              presets: [
                '@babel/preset-env',
                '@babel/preset-react',
                '@babel/preset-typescript'
              ],
              plugins: [
                [
                  'import', { 
                    libraryName: 'antd', 
                    libraryDirectory: 'es', 
                    style: 'css' 
                  }
                ],
                ["@babel/plugin-proposal-class-properties"]
              ]
            }
          }]
      },
```
not work again and again...... -_-!!!

## 打开页面控制台  
antd的样式命名和别的不一样啊  
{% asset_img 2.jpg %}  
推断：<font face="楷体" size = 4 color = Red >可能是cssModule和antd样式冲突了</font>  

## 解决方案： <font face="楷体" size = 4 color = Red >**webpack cssModules配置加过滤**</font>  
```javascript
{
        test: [/\.css$/, /\.less$/],
        exclude: [/[\\/]node_modules[\\/].*antd/], // 这里做个判断
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              plugins: (loader) => [
                require('autoprefixer')()
              ],
            }
          },
          {
            loader: 'less-loader',
          },
        ]
      }
```  
<br/>


{% asset_img 3.jpg '打完收工~~' %}  







