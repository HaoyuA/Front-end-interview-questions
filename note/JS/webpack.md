干吗的 

转译代码 (ES6转为ES5 SCSS转为CSS)

构建build

代码压缩

代码分析



[webpack官方文档](https://webpack.docschina.org/concepts/)

官方文档说 本质上 webpack 是用于现代JavaScript应用程序的 **静态模块打包工具**

它在处理应用程序是 会在内部构建一个**依赖图**  这个依赖图会映射到项目的每个模块 并打包生成一个或多个bundle



本地安装 

```bash
yarn add webpack@4 webpack-cli@3 --dev
```

 运行

```bash
npx webpack //自动找本地的webpack
./node_modules/.bin/webpack 
```



其中的 webpack.config.js可以设置

```javascript
mode.exports = {
  mode: 'production' || 'development'
  // production 产品模式就是将代码压缩至最小 就一行 无注释 便于加载
  // 开发模式 就是给开发者看的代码 方便debug 虽然我看不懂
  entry: './src/index.js', //转译的文件
  output: {
    path: path.resolve(__dirname, 'dist'), // 产出路径 默认dist 可删这行
    filename: 'main.js',  // 产出的文件名
    filename: "[name].[contenthash].js"，
    
  },
}
```



#### http 缓存的作用

响应头中的cache-control

假如访问baidu.com

它会下载 index.html  xxx.css xxx.js

如果在十秒后又访问了 可以在除了index.html 的文件上加一个缓存时间 这样在第二次访问就不用再下载资源了 但是衍生出两个问题

那么如果资源更新了怎么办

因为在webpack中 每次打包都会对 文件内容进行哈希 `filename: "[name].[contenthash].js"，`所以只要内容更新 文件名就会变 缓存是根据名字来的 所以新的资源会被重新加载

为什么index不能缓存

如果首页都缓存了 那么怎么知道去更新某个资源呢 所以还是不能缓存

实际体现为 响应头上的

Response Headers 上的 Cache-Control: public属性 告诉浏览器 可以被所有人缓存 这样第二次加载就能变快



#### webpack 打包 html css less...

用webpack生成html

```javascript
// 先装 html-plugin 模块
yarn add --dev html-webpack-plugin@4

// 然后改写 webpack.config.js里的代码
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");
module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    title: "我是天才", // title标签内容
    template: "src/assets/index.html",  // 模板
    filename: "[name].[contenthash].js",
  },
  plugins: [new HtmlWebpackPlugin()],
};
// 再build时 发现dist 里自动生成了 index.html!!! 而且有对js的引用！自动更新引用的名字
```



webpack 引入css

先安装 css-loader和 style-loder

`yarn add style-loader --dev` `yarn add css-loader --dev`

css-loder是将css文件内容读到js里面

style-loader将css-loader读到的内容放到head里面的style标签

```javascript
// webpack.config.js + module部分
module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "[name].[contenthash].js",
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: "我是天才",
      template: "src/assets/index.html",
    }),
  ],
  // 
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

或者 把css抽成文件

装个`yarn add mini-css-extract-plugin --dev`

```javascript
// webpack.config.js
module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "[name].[contenthash].js",
  },
  devtool: "inline-source-map",
  devServer: {
    contentBase: "./dist",
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: "我是天才",
      template: "src/assets/index.html",
    }),
    new MiniCssExtractPlugin({
      filename: '[name].[contenthash].css',
      chunkFilename: '[id].[contenthash].css',
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
    ],
  },
```







使用webpack-dev-sever

`yarn add --dev webpack-dev-server`

实时更新不用重新手动打包

并不需要在dist中更新 而是直接在内存中更新 更快



如何用两个config分别用于开发和生产：

```javascript
// 在package.json 里用 --config 后加想用的文件的路径

"scripts": {
    "start": "webpack-dev-server --open --config webpack.config.js",
    "build": "rm -rf dist && webpack --config webpack.config.prod.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```



file-loader

加载文件 需要怎么做呢

如果我想在页面引入在assets目录下的一张图片 1.png

一般的话我们会

```javascript
const div = document.getElementById("app");

div.innerHTML = `<img src="./assets/1.png"> `;  // 报错
// 但事实上相对路径在打包后就不对了 需要使用file loader

import png from "./assets/1.png";
console.log(png); // 6daaad54ca61db33da5462b4edacb3d6.png 自动哈希过的文件路径 注意不是文件内容 所以可以直接放到src里
div.innerHTML = `
  <img src="${png}"> 
`;

// webpack.config.js
module: {
    rules: [
      // 加一个rule
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: ["file-loader"],
      },
    ],
  },
```

file-loader作用就是把文件变成文件路径



####  Import() 实现懒加载

首先监听某个事件

然后在回调函数中 需要import()加载一个资源

然后这个请求会返回一个promise 成功就加载资源 失败就报错

```javascript
const button = document.createElement("button");
button.innerText = "懒加载";
button.onclick = () => {
  const promise = import("./lazy");
  console.log(promise);
  promise.then(
    (module) => {
      const fn = module.default;
      fn();
    },
    () => {}
  );
};
```





#### Loader vs Plugin

先看看官方的[loader](https://webpack.docschina.org/concepts/#loaders)和[plugin](https://webpack.docschina.org/concepts/#plugins)

webpack 只能理解 JavaScript 和 JSON 文件，这是 webpack 开箱可用的自带能力。**loader** 让 webpack 能够去处理其他类型的文件，并将它们转换为有效 [模块](https://webpack.docschina.org/concepts/modules)，以供应用程序使用，以及被添加到依赖图中。loader 有test 和use 属性去告诉那些文件需要被转换以及被转换时需要用哪个Loader

loader 用于转换某些类型的模块，而**Plugin**则可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。

**webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

就像上面做到的那样 

**转译js**

默认可以转译js 但是实际上是一个babel-loader load到webpack 在输出dist里的js

**转译css**

2种方法

style/css loader 将其导入到js 在以style标签插到html里 1 => 1

或者

mini-css-extrac-plugin 去抽成文件 **注意** 这里是**n**个文件变成**1**个文件 n = >1

**转译html**

html-webpack-plugin 给或者不给个模板 template  1/0 => 1  会自动引用css 和js



**那么区别是什么**

1. 翻译一下 loader就是加载器 plugin 插件
2. loader 去load文件的 举个🌰 比如webpack 内置的babel-loader 加载  将ES6语法编译成IE支持的js 或者style/css loader将css文件加载到页面style标签 
3. plugin 插件是扩展wenpack功能的 🌰 mini-css-extract-plugin把 css 抽成文件