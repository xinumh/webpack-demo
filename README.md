webpack



npm install webpack-cli -g

npm install webpack -g

webpack 配置的可扩展:

将关注点从环境(environment)、构建目标(build target)、运行时(runtime)中分离

 [webpack-merge](https://github.com/survivejs/webpack-merge)

## 多入口：

### 用法

> entry: string | [string]

```js
// 一次注入多个依赖文件，并且将它们的依赖关系绘制在一个 "chunk" 中时，这种方式就很有用
// 常用：一个库，单一入口
module.exports = {
  entry: ['./src/a.js', './src/b.js'],
  output: {
    filename: 'bundle.js',
  },
};
```

>  entry: { <entryChunkName> string | [string] } | {}

```js
// 定义入口的最可扩展的方式
entry: {
    a2: './src/index.js',
    b2: {
      dependOn: 'a2',
      import: './src/app.js'
    }
  },
```

### 场景

1. 分离 app(应用程序) 和 vendor(第三方库) 入口（优化：浏览器缓存）

   **不要** 为 vendor 或其他不是执行起点创建 entry。

2. 多页面应用程序

   复用多个入口起点之间的大量代码/模块

**每个 HTML 文档只使用一个入口起点**

## 唯一输出

告知 webpack 如何向硬盘写入编译文件。

>  即使可以存在多个 `entry` 起点，但只能指定一个 `output` 配置。

### 用法

创建多个chunk，使用占位符确保每个文件具有唯一的名称。

```js
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js',
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist',
  },
}
// 写入到硬盘：./dist/app.js, ./dist/search.js
```

### 场景

资源使用 CDN 和 hash 

## loaders

css-loader

ts-loader

### 特性

`链式调用`

`链中的最后一个 loader，返回 webpack 所期望的 JavaScript`

`运行在 Node.js `

`插件(plugin)可以为 loader 带来更多特性`

`loader 能够产生额外的任意文件`

### 解析

loader 将从 [模块路径](https://webpack.docschina.org/concepts/module-resolution/#module-paths) 加载（通常是从 `npm install`, `node_modules` 进行加载）

[编写一个 loader](https://webpack.docschina.org/contribute/writing-a-loader/)

## plugin

解决 `loader` 无法实现的其他事。

**插件**是一个具有 [`apply`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 方法的 JavaScript 对象



```js
const webpack = require('webpack'); // 访问 webpack 运行时(runtime)
const configuration = require('./webpack.config.js');

let compiler = webpack(configuration);

new webpack.ProgressPlugin().apply(compiler);

compiler.run(function (err, stats) {
  // ...
});
```



[webpack 源码](https://github.com/webpack/webpack)

实现既可表达，又可灵活配置: 配置即代码

## manifest 

[`WebpackManifestPlugin`](https://github.com/shellscape/webpack-manifest-plugin) 缓存、长效缓存

[`mini-css-extract-plugin`](https://webpack.docschina.org/guides/code-splitting/plugins/mini-css-extract-plugin): 用于将 CSS 从主应用程序中分离

[`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin) 插件可以将公共的依赖模块提取到已有的入口 chunk 中

## source map 错误追踪

[模块热替换(hot module replacement)](https://webpack.docschina.org/guides/hot-module-replacement)

## 代码分离

- 入口起点：使用 [`entry`](https://webpack.docschina.org/configuration/entry-context) 配置手动地分离代码
- 防止重复：使用 [Entry dependencies](https://webpack.docschina.org/configuration/entry-context/#dependencies) 或者 [`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin) 去重和分离 chunk。
- 动态导入：通过模块的内联函数调用来分离代码



[官方分析工具](https://github.com/webpack/analyse)

