- [1、package.json 版本选择以及 package-lock.json 的作用？](#1packagejson-版本选择以及-package-lockjson-的作用)
- [2、说一下 webpack 中 css-loader 和 style-loader 的区别](#2说一下-webpack-中-css-loader-和-style-loader-的区别)
- [3、说一下 webpack 中 file-loader 和 url-loader 的区别](#3说一下-webpack-中-file-loader-和-url-loader-的区别)
- [4、Webpack 打包时 Hash 码 有哪几种？](#4webpack-打包时-hash-码-有哪几种)
- [5、import { Button } from 'antd'，打包的时候只打包 button，分模块加载，是怎么做到的](#5import--button--from-antd打包的时候只打包-button分模块加载是怎么做到的)
- [6、Webpack 打包出来的体积太大，如何优化体积？ // TODO](#6webpack-打包出来的体积太大如何优化体积--todo)
- [7、@babel/polyfill、@babel/runtime、@babel/plugin-transform-runtime 的区别和联系？](#7babelpolyfillbabelruntimebabelplugin-transform-runtime-的区别和联系)



## 1、package.json 版本选择以及 package-lock.json 的作用？

- ‘^16.8.0’ 表示安装16.x.x的最新版本，安装时不改变大版本号。
- ‘~16.8.0’ 表示安装16.8.x的最新版本，安装时不改变大版本号和次要版本号。
- ‘16.8.0’ 表示安装指定的版本号，也就是安装16.8.0版本。

- package-lock.json里会维护一个依赖管理树，里面记录着每个依赖的确定版本, 获取地址和哈希值等信息，这样就保证了每次安装下载的依赖版本都是一样的。

---------------------------------------------------------------------------------------------------------------------- 
## 2、说一下 webpack 中 css-loader 和 style-loader 的区别

- （1） style-loader: 使用<style>将 css-loader 内部样式注入到我们的HTML页面
- （2） css-loader： 解析了css文件里面的css代码，以及 css 中各依赖关系，比如：import / require（） @import / url 引入的内容

webpack解析是自下而上的，所以会先通过 css-loader 解析 css 代码， 然后通过 style-loader 注入到 HTML 页面中。如果需要将 css 单独抽离出一个文件，可以使用 extract-text-webpack-plugin（webpack v4 之前）和 mini-css-extract-plugin （webapck v4）

----------------------------------------------------------------------------------------------------------------------
## 3、说一下 webpack 中 file-loader 和 url-loader 的区别

- （1）`file-loader`: 对应规则的文件，复制一份到打包之后的文件夹，生成新的文件名，并传给项目的输入文件使用；
- （2）`url-loader`: url-loader 封装了 file-loader， 安装了 url-loader 就不需要再安装 file-loader 了。url-loader工作分两种情况：1.文件大小小于limit参数，url-loader将会把文件转为DataURL（base64）；2.文件大小大于limit，url-loader会调用file-loader进行处理，参数也会直接传给file-loader。

因此，再处理项目中图片等文件时，我们一般使用 url-loader， 设置合适的 limit 的值: 设置过大会导致js文件过大；设置过小会导致 http 请求次数过多。

----------------------------------------------------------------------------------------------------------------------


## 4、Webpack 打包时 Hash 码 有哪几种？

- （1）hash: 每一次构建都会生成唯一的 hash 值，且当前构建的所有文件添加的 hash 值都一样；

- （2）chunkhash: chunkhash 可以理解成入口 hash，即改入口中依赖的某个文件改变了，那么该入口所有生成文件的 hash 值都会改变，常见的就是 css 文件变动了，对应的 js 文件 hash 值也会改变。

- （3）contenthash: 内容 hash， 即该 hash 值只跟当前文件的 content 有关， content 变化了 hash 值才会改变。

因此，我们给图片和 css 文件添加 hash 值用于缓存时，应该使用 contenthash。

----------------------------------------------------------------------------------------------------------------------

## 5、import { Button } from 'antd'，打包的时候只打包 button，分模块加载，是怎么做到的 

使用 babel-plugin-import， 
```js
{ "libraryName": "antd", style: true }：
```
将 
```js
import { Button } from 'antd';
```
编译成：
```js
var _button = require('antd/lib/button');
require('antd/lib/button/style');
```

----------------------------------------------------------------------------------------------------------------------
## 6、Webpack 打包出来的体积太大，如何优化体积？ // TODO

- css 篇

    - 单独提取css（公共）文件 MiniCssExtractPlugin

    - 压缩css optimize-css-assets-webpack-plugin (配合 cssnano 配置压缩参数)

- js 篇

    - 压缩 js 代码：uglifyjs-webpack-plugin

    - 提取公共代码：splitChunks

- html 压缩： html-webpack-plugin 中配置

----------------------------------------------------------------------------------------------------------------------
## 7、@babel/polyfill、@babel/runtime、@babel/plugin-transform-runtime 的区别和联系？

- @babel/polyfill：通过改写全局prototype的方式实现，会导致污染了全局环境，我们需要手动引入，在编译时自动引入对应的core-js；

- @babel/runtime：提供编译模块的工具函数（core-js 、regenerator等），需要在使用到的地方手动引入，不会造成全局环境的污染；

- @babel/plugin-transform-runtime： 依赖 @babel/runtime ，利用 plugin 自动识别并替换代码中的新特性，你不需要再引入，而是按需替换，检测到你需要哪个，就引入哪个 polyfill；

----------------------------------------------------------------------------------------------------------------------

