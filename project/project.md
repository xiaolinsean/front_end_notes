- [1、项目中的用户操作记录及错误信息收集](#1项目中的用户操作记录及错误信息收集)
- [2、组件库搭建注意事项  // TODO](#2组件库搭建注意事项---todo)
- [3、node-modules 升级 或 webpack 升级](#3node-modules-升级-或-webpack-升级)
- [4、项目结构组织？ // TODO](#4项目结构组织--todo)
- [5、hybrid 混合开发、纯原生、webApp 之间的区别？](#5hybrid-混合开发纯原生webapp-之间的区别)
- [6、hybrid 混合开发中 H5 和原生客户端如何通信？](#6hybrid-混合开发中-h5-和原生客户端如何通信)
- [7、同项目组员之间如何定制代码规范](#7同项目组员之间如何定制代码规范)
- [8、有哪些前端性能优化的方面？](#8有哪些前端性能优化的方面)
- [9、换肤（切换主题色）的方案？ // TODO](#9换肤切换主题色的方案--todo)
- [10、浏览器输入URL后发生了什么？](#10浏览器输入url后发生了什么)

## 1、项目中的用户操作记录及错误信息收集 

- 用户操作记录可以在对应的事件点增加埋点，直接出发接口请求；

- 错误信息：
  
  - `window.onerror` ： 可以捕获 运行时的异步和非异步错误；
    
    ```js 
    window.onerror = function (msg, url, row, col, error) {
        console.log('我知道异步错误了');
        console.log({
            msg,  url,  row, col, error
        })
        return true;
    };
    ```
  - `addEventListener("error")`: 不仅可以捕获运行时的异步和非异步错误，还可以监控静态资源加载错误，由于网络请求异常不会事件冒泡，因此必须在捕获阶段将其捕捉到才行：
    ```js
    window.addEventListener('error', (msg, url, row, col, error) => {
      console.log('我知道 404 错误了');
      console.log(
          msg, url, row, col, error
      );
    return true;

    ```

    window.addEventListener(‘error’)与window.onerror的区别

      - 前者能够捕获到资源加载错误，后者不能。
      - 都能捕获js运行时错误，捕获到的错误参数不同。前者参数为一个event对象；后者为 msg, url, lineNo, columnNo, error一系列参数。event对象中都含有后者参数的信息。
  - `Promise 错误` : 可以添加一个 Promise 全局异常捕获事件 unhandledrejection, 用来捕获 Promise 实例抛出的异常
    
    ```js
      window.addEventListener("unhandledrejection", function(e){
      e.preventDefault()
      console.log('我知道 promise 的错误了');
      console.log(e.reason);
      return true;
      }
    ```

- 上报方式
  - 通过 Ajax 发送数据
  - 动态创建 img 标签的形式

- [MITO 一款轻量级的收集页面的用户点击行为、路由跳转、接口报错、代码报错、并上报服务端的SDK](https://github.com/clouDr-f2e/mitojs)

参考:[前端异常监控](https://juejin.cn/post/6844903641619365902)、[前端性能和错误监控](https://juejin.im/post/6844903998412029959)


## 2、组件库搭建注意事项  // TODO


## 3、node-modules 升级 或 webpack 升级

我们安装了某个 `modules` 后，在 `package.json` 和 `package-lock.json` 中都能看到安装模块对应的版本描述，默认更新规则是 `^1.3.1` （这对于 npm 版本控制规则意味着 npm 可以更新到补丁版本和次版本：即 1.3.2、1.4.0、依此类推，但不更新主版本）。

`package-lock.json` 中记录的是本地安装的真是版本号，`package.json` 中只是记录更新规则，因此有可能两处的版本号不一样，可能出现 `package.json` 中是 `^1.3.1`，但是 `package-lock.json` 中是 `1.4.0`。

如果有新的次版本或补丁版本，并且输入了 `npm update`，则已安装的版本会被更新，并且 `package-lock.json` 文件会被新版本填充。 `package.json` 则保持不变。

通过运行 `npm outdated` 可以查看 `modules` 的版本情况，包含当前版本、升级后的版本，以及最新的版本

我们可以通过 `npm update` 将当前版本升级到 升级后的版本，但是这种方法不能跨主版本升级。因为主版本的修改一般都会引起较大的更改，默认更新的话可能会引起问题。

若要将所有软件包更新到新的主版本，则全局地安装 npm-check-updates 软件包：

```shell
npm install -g npm-check-updates
```

然后运行：

```shell
ncu -u
```

这会升级 `package.json` 文件的 `dependencies` 和 `devDependencies` 中的所有版本，以便 `npm` 可以安装新的主版本。

现在可以运行更新了：

```shell
npm update
```

参考：[将所有 Node.js 依赖包更新到最新版本](http://nodejs.cn/learn/update-all-the-nodejs-dependencies-to-their-latest-version)

---------------------------------------------------------------------------------------------
## 4、项目结构组织？ // TODO

## 5、hybrid 混合开发、纯原生、webApp 之间的区别？

- Native App
  
    一般是指本地化应用，后续简称 NA；

    优点：体验好，可以做一些比较好的交互效果，可作为独立软件出售；

    缺点：更新较差，需要靠发版本解决；且历史版本无法同步更新，开发成本比较大，需要两波开发人员：Android 和 IOS，分别使用 Object-c 和 Java；

- Web App

    一般是指我们开发的Html5网站，后续简称 H5；

    优点：开发成本较低，前端开发人员开发一套同时适配 IOS 和 Android；更新好，可随时上线，上线后版本能普及到所有使用的用户；

    缺点：体验没有 NA 好，没有独立的软件作为入口（当然后续如果 PWA 能普及并支持下载，这也不再是缺点了）；

- Hybird App

    一般是指混合型 App，一部分是 NA 开发人员开发，一部分是 H5 页面；

    优点：跨平台开发周期短、成本低，又能发挥Native App体验和性能的优势

    缺点：涉及到 H5 与 NA 通信，需要客户端开发和前端开发协同合作；

- 其他 App 

    js + 原生渲染，框架代表：RN、Weex；自绘 UI + 原生，框架代表：Flutter，对此的介绍可参考移动开发技术简介

## 6、hybrid 混合开发中 H5 和原生客户端如何通信？

- 1、NA 和 H5 分别将方法挂载在 window 上，供对方使用

    H5 调用客户端：NA 根据约定，将 H5 可调用的 API 绑定到 webview 的 window 上，H5 按照该约定进行调用；
    客户端调用 H5：H5 根据约定， 将 NA 可调用的 API 和 一些回调函数 绑定到 webview 的 window 上， NA 按照该约定进行调用；
    
- 2、NA 和 H5 通过 JsBridge 桥梁进行通信

    客户端调用 H5：H5 根据约定，将一组 API 绑定在 webview 的 window 上，NA 通过 webview 获取 window 上的 js 方法；
    H5 调用客户端：H5 根据要求主动修改 URL 的值， 客户端监听到 URL 的变化，截取 URL，调用对应的操作。

参考：[Hybrid 应用中 H5 与 NA 通信的那点事儿](https://mp.weixin.qq.com/s/L4VzJGcO4PRs7YXxjKdusA)

## 7、同项目组员之间如何定制代码规范

使用 eslint + prettier 进行配置，处理可以语法检查，还没进行代码自动格式化。

配置参考:[VSCode + ESLint + Prettier 代码语法检查和格式化](https://blog.csdn.net/u014607184/article/details/104061336)

## 8、有哪些前端性能优化的方面？

- 文件（代码）大小优化

    - 图片压缩，可用 css 实现的小图标，尽量用 css 实现
    - webpack SplitChunksPlugin 抽离公共代码，防止代码重复打包
    - 利用 tree shaking， 避免多余代码
    - 对代码进行压缩：JavaScript：UglifyPlugin；CSS ：MiniCssExtractPlugin；HTML：HtmlWebpackPlugin
    - 使用动态 polyfill，避免不必要的 polyfill
    - 使用 fontmin-webpack 插件对字体文件进行压缩
    - 使用 GZIP 压缩文件

- 加载优化

    - 合并请求，图片合并（雪碧图），
    - 静态资源使用 cdn（大图片和大文件）
    - 使用缓存，防止同一资源重复请求
    - 图片懒加载
    - 使用动态 import，切分页面代码，减小首屏 JS 体积；
    - 更快地首屏加载，可考虑 SSR + 同构
    - 使用 service workers

- UI 视觉优化

    - 首屏首先展示骨架屏
    - 图片或内容部分未加载完成时先使用占位符
    - 合理使用 loading


- 代码优化
    
    - 减少重绘重排
    - 使用事件委托，减少内存
    - 防抖（debounce）/节流（throttle），减少过多的请求
    - 使用requestAnimationFrame代替setInterval操作

参考：[前端性能优化 24 条建议（2020）](https://juejin.im/post/6892994632968306702)

## 9、换肤（切换主题色）的方案？ // TODO

参考：[webpack插件实现自动抽取css中的主题色样式，并一键动态切换主题色（element-ui）](https://segmentfault.com/a/1190000016061608)


## 10、浏览器输入URL后发生了什么？

- DNS域名解析: 根据域名找到对应的IP

    浏览器缓存中查找->本地的hosts文件查找->找本地DNS解析器缓存查找->本地DNS服务器查找->根域名服务器->顶级域名服务器->权威域名服务器；找到就返回

- 建立TCP连接
  
  判断是为 http 还是 https(HTTP + SSL / TLS)，然后进行三次握手，建立TCP连接

- 发送HTTP请求，服务器处理请求，返回响应结果
  
  这一步需要注意HTTP缓存的判断

- 关闭TCP连接

    四次挥手

- 浏览器渲染
  
  构建 DOM 树 -> 样式计算、构建 CSSOM -> 合并 DOM 树 和 CSSOM -> 渲染


参考：[细说浏览器输入URL后发生了什么](https://juejin.cn/post/6844904054074654728)


