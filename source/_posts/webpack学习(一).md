---
title: webpack学习(一)
date: 2021-03-15 13:59:41
tags: [webpack，项目工程化]
---

webpack4 的配置和 loader 的原理

<!--more-->

## webpack

Webpack 是⼀个现代 JavaScript 应⽤程序的静态模块打包器（module bundler），当 webpack 处理应⽤程序时，它会递归地构建⼀个依赖关系图(dependency graph)，其中包含应⽤程序需要的每个模块，然后将所有这些模块打包成⼀个或多个 bundle。

## 环境搭建

```javascript
npm init -y # 初始化npm配置⽂件
npm install --save-dev webpack # 安装核⼼库
npm install --save-dev webpack-cli # 安装命令⾏⼯具

# 安装最新的4.x稳定版本
npm i -D webpack@4.44.0
# 安装指定版本
npm i -D webpack@<version>
全局安装（不推荐）
```

## 执行打包

```
//npx 命令会先寻找本地的依赖
npx webpack

//直接添加到package.json
"scripts": {
"test": "webpack"
},
```

## webpack.config.js

```
module.exports = {
    entry: "./src/index.js", //打包⼊⼝⽂件
    //多⼊⼝的处理
    output: {
    filename: "[name][chunkhash:8].js",//利⽤占位符，⽂件名称不要重复
    path: path.resolve(__dirname, "dist")//输出⽂件到磁盘的⽬录，必须是绝对路径
    },
    mode: "production", //打包环境
    module: {
    rules: [
    //loader模块处理
    {
    test: /\.css$/,
    use: "style-loader"
    }]
    },
    plugins: [new HtmlWebpackPlugin()] //插件配置
}
```

## webpack 配置核⼼概念

- chunk：指代码块，⼀个 chunk 可能由多个模块组合⽽成，也⽤于代码合并与分割。
- bundle：资源经过 Webpack 流程解析编译后最终结输出的成果⽂件。
- entry：顾名思义，就是⼊⼝起点，⽤来告诉 webpack ⽤哪个⽂件作为构建依赖图的起点。webpack 会根据 entry 递归的去寻找依赖，每个依赖都将被它处理，最后输出到打包成果中。
- output：output 配置描述了 webpack 打包的输出配置，包含输出⽂件的命名、位置等信息。
- loader：默认情况下，webpack 仅⽀持 .js ⽂件，通过 loader，可以让它解析其他类型的⽂件，充
  当翻译官的⻆⾊。理论上只要有相应的 loader，就可以处理任何类型的⽂件。
- plugin：loader 主要的职责是让 webpack 认识更多的⽂件类型，⽽ plugin 的职责则是让其可以控制流程，从⽽执⾏⼀些特殊的任务。插件的功能⾮常强⼤，可以完成各种各样的任务。
- mode：4.0 开始，webpack ⽀持零配置，旨在为开发⼈员减少上⼿难度，同时加⼊了 mode 的概
  念，⽤于指定打包的⽬标环境，以便在打包的过程中启⽤ webpack 针对不同的环境下内置的优化。。

## loader

但是 webpack 默认只知道如何处理 js 和 JSON 模块，那么其他格式的模块处理，和处理⽅式就需要 loader

```
style-loader
css-loader
less-loader
sass-loader
ts-loader //将Ts转换成js
babel-loader//转换ES6、7等js新特性语法
file-loader//处理图⽚⼦图
eslint-loader
```

## module

当 webpack 处理到不认识的模块时，需要在 webpack 中的 module 处进⾏配置，当检测到是什么格式的
模块，使⽤什么 loader 来处理。

```
module:{
rules:[
    {
    test:/\.xxx$/,//指定匹配规则
    use:{
        loader: 'xxx-load'//指定使⽤的loader //执行顺序是从后向前
    }}
    ]
}
```

## plugins: webpack 扩展

- 作⽤于 webpack 打包整个过程
- webpack 的打包过程是有（⽣命周期概念）钩⼦

eg: html-webpack-plugin, mini-css-extract-plugin

```
module.exports = {
 ...
 plugins: [
    new htmlWebpackPlugin({
    title: "My App",
    filename: "app.html",
    template: "./src/index.html"
 })
 ]
};


module.exports = {
module: {
rules: [
 {
    test: /\.less$/,
    use: [
    // 插件需要参与模块解析，须在此设置此项，不再需要style-loader
            {
            loader: MiniCssExtractPlugin.loader,
            options: {
            hmr: true, // 模块热替换，仅需在开发环境开启
            // reloadAll: true,
             // ... 其他配置
            }
             },
            'css-loader',
            'postcss-loader',
            'less-loader'
        ],
        },
    ],
 },
 plugins: [
    new MiniCssExtractPlugin({
        filename: '[name].css', // 输出⽂件的名字
        // ... 其他配置
    }),
 ]
};

```

## postcss (css 版 bable)

常用工具

`npm install postcss-loader autoprefixer cssnano -D`

```
# 配置postcss.config.js
module.exports = {
    plugins: [require("autoprefixer")],
};

# 配置package.json
"browserslist":["last 2 versions", "> 1%"],

# 或者直接在postcss.config.js⾥配置
module.exports = {
    plugins: [
    require("autoprefixer")({
    overrideBrowserslist: ["last 2 versions", "> 1%"],
     }),
    ],
};
# 或者创建.browserslistrc⽂件
> 1%
last 2 versions
not ie <= 8
```

## loader 原理-css-load 简单实现

本质是一个可以返回字符串的函数

```
//style-loader

module.exports = function (source) {
  return `const tag =  document.createElement('style')
            tag.innerHTML = ${source}
            document.head.appendChild(tag)`;
};

//less-loader

const less = require("less");

module.exports = function (source) {
  less.render(source, (error, output) => {
    let { css } = output;
    this.callback(error, css);
  });
};


//css-loader
module.exports = function (source) {
  this.callback(null, JSON.stringify(source));
};


```
