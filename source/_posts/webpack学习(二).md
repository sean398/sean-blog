---
title: webpack学习(二)
date: 2021-03-22 22:07:50
tags: [webpack，项目工程化]
categories: [前端学习, webpack]
---

webpack4 的配置和 loader 的原理

<!--more-->

## 图片/字体文件处理

- file-loader
- url-loader

区别： url-loader 可以设置文件大小限制，当低于限制时可以将文件转化成为 dataurl，减少网络请求。

```
# 使⽤
module.exports = {
    modules: {
    rules: [
    {
        test: /\.(png|jpg|gif|jpeg|webp|svg|eot|ttf|woff|woff2)$/,
        use: [
        {
            loader: 'url-loader', // 仅配置url-loader即可，内部会⾃动调
            ⽤file-loader
            options: {
            limit: 10240, //⼩于此值的⽂件会被转换成DataURL
            name: '[name]_[hash:6].[ext]', // 设置输出⽂件的名字
            outputPath: 'assets', // 设置资源输出的⽬录
        }
    },
    {
        test: /\.(eot|ttf|woff|woff2|svg)$/,
        use: "file-loader"
    }
 ],
 exclude: /node_modules/
```

## HtmlWebpackPlugin

`npm install --save-dev html-webpack-plugin`

### 配置

```
title: ⽤来⽣成⻚⾯的 title 元素
filename: 输出的 HTML ⽂件名，默认是 index.html, 也可以直接配置带有⼦⽬录。
template: 模板⽂件路径，⽀持加载器，⽐如 html!./index.html
inject: true | 'head' | 'body' | false ,注⼊所有的资源到特定的 template 或者
templateContent 中，如果设置为 true 或者 body，所有的 javascript 资源将被放置到 body
元素的底部，'head' 将放置到 head 元素中。
favicon: 添加特定的 favicon 路径到输出的 HTML ⽂件中。
minify: {} | false , 传递 html-minifier 选项给 minify 输出
hash: true | false, 如果为 true, 将添加⼀个唯⼀的 webpack 编译 hash 到所有包含的脚本和
CSS ⽂件，对于解除 cache 很有⽤。
cache: true | false，如果为 true, 这是默认值，仅仅在⽂件修改之后才会发布⽂件。
showErrors: true | false, 如果为 true, 这是默认值，错误信息会写⼊到 HTML ⻚⾯中
chunks: 允许只添加某些块 (⽐如，仅仅 unit test 块)
chunksSortMode: 允许控制块在添加到⻚⾯之前的排序⽅式，⽀持的值：'none' | 'default' |
{function}-default:'auto'
excludeChunks: 允许跳过某些块，(⽐如，跳过单元测试的块)
```

## clean-webpack-plugin

`npm install --save-dev clean-webpack-plugin`

```
plugins: [
 new CleanWebpackPlugin()
]
```

## sourceMap

package.json 配置

`devtool: "none"`

[devtool 文档](https://webpack.js.org/configuration/devtool#devtool)

```
devtool:"cheap-module-eval-source-map",// 开发环境配置
//线上不推荐开启
devtool:"cheap-module-source-map", // 线上⽣成配置
```

## webpackDevServer

```
//package.json
"scripts": {
 "server": "webpack-dev-server"
 },

 //webpack.config.js

 devServer: {
 contentBase: "./dist",
 open: true,
 port: 8081
 },
```

## 使用 express mock

```
// npm i express -D
// 创建⼀个server.js 修改scripts "server":"node server.js"
//server.js
const express = require('express')
const app = express()
app.get('/api/info', (req,res)=>{
res.json({
name:'开课吧',
age:5,
msg:'欢迎来到开课吧学习前端⾼级课程'
 })
})
app.listen('9092')
//node server.js
http://localhost:9092/api/info


//使用服务器代理解决跨域问题
proxy: {
"/api": {
    target: "http://localhost:9092"
    }
 }
```

## 多页面配置

```

//使用glob获取文件路径，代码自动生成多页面配置

const glob = require("glob");

const setMpa = () => {
  const entry = {};
  const htmlwebpackplugins = [];

  const entryFiles = glob.sync(path.join(__dirname, "./src/*/index.js"));

  entryFiles.map((item, index) => {
    const entryFile = entryFiles[index];
    const match = entryFile.match(/src\/(.*)\/index\.js$/);
    const pageName = match[1];
    entry[pageName] = entryFile;
    htmlwebpackplugins.push(
      new htmlWebpackPlugin({
        template: `./src/${pageName}/index.html`,
        filename: `${pageName}.html`,
        chunks: [pageName],
      })
    );
  });

  return {
    entry,
    htmlwebpackplugins,
  };
};
const { entry, htmlwebpackplugins } = setMpa();
```
