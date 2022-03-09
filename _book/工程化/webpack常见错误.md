### 1、CopyWebpackPlugin 版本
```shell
ValidationError: Invalid options object. Copy Plugin has been initialized using an options object that does not match the API schema.
 - options[0] misses the property 'patterns'. Should be:
   [non-empty string | object { from, to?, context?, globOptions?, filter?, transformAll?, toType?, force?, priority?, info?, transform?, transformPath?, noErhould not have fewer than 1 item)
```
之前的格式：
```js
new CopyWebpackPlugin([
    {
        from: path.join(__dirname, '../static'),
        to: path.join(__dirname, '../build/static')
    },
    {
        from: path.join(__dirname, '../common'),
        to: path.join(__dirname, '../build/common')
    }
])
```

修改成如下格式：
```js
new CopyWebpackPlugin({
    patterns: [
        {
            from: path.join(__dirname, '../static'),
            to: path.join(__dirname, '../build/static')
        },
        {
            from: path.join(__dirname, '../common'),
            to: path.join(__dirname, '../build/common')
        }
    ]
})
```

### 2、commonChunksPlugin 升级为 splitChunksPlugin

```shell
TypeError: webpack.optimize.CommonsChunkPlugin is not a constructor
```

V4之后不再支持commonChunksPlugin，改用splitChunksPlugin

```js
optimization: {
    // 提取公共代码
    splitChunks: {
        // V4之后不再支持commonChunksPlugin，改用splitChunksPlugin
        cacheGroups: {
            // 注意: priority属性
            // 其次: 打包业务中公共代码
            common: {
                name: 'common',
                // test: /common\/|components\//,
                chunks: 'all',
                minSize: 3, // 文件大小限制
                minChunks: 2, // 引用次数
                priority: 0,
            },
            // 首先: 打包node_modules中的文件
            vendor: {
                name: 'vendor',
                test: /[\\/]node_modules[\\/]/,
                chunks: 'all',
                priority: 10, // 设置优先级，先抽离node_modules
            },
        },
    },
},
```

### 3、webpack-merge

```shell
TypeError: merge is not a function
```
`webpack-merge` v4 和 v5 的写法有所不同

```js
// webpack-merge v4 (and earlier)
const merge = require('webpack-merge')  

// webpack-merge v5 (and later)
const { merge } = require('webpack-merge')
```

### 4、webpack不能混合使用 import 和 module.exports

webpack不能混合使用 import 和 module.exports ， 如果混用，就会报这个错误：

```shell
Cannot assign to read only property 'exports' of object '#<Object>'
```

使用 export default 替代 module.exports，在项目中应该形成一个规范；

另外，配置 `.babelrc` 测试下来也可以
```js
"presets": [
    [
        "@babel/preset-env",
        {
            "modules": "commonjs", // 这一项配置起作用
            "useBuiltIns": "usage",
            "corejs": "2"
        }
    ],
    "@babel/preset-react"
]
```


### 5、webpack 打包 img 后 src 为“[object Module]”

升级 `url-loader` 或 `file-loader` 后，会出现 img 的 src 为 [object Module]。

这是因为 `url-loader` 或 `file-loader` 在新版本中 esModule 默认为 true，因此手动设置为 false：
```js
{
    test: /\.(png|jpg|gif)$/i,
    loader: 'url-loader',
    options: {
        esModule: false,
        limit: 1024, // 1K
        name: 'images/[name].[hash:8].[ext]',
    },
},
```


### 6、mini-css-extract-plugin 插件引用顺序

```shell
Conflicting order. Following module has been added:…
```

此警告意思为在不同的js中引用相同的css时，先后顺序不一致。也就是说，在1.js中先后引入a.css和b.css，而在2.js中引入的却是b.css和a.css，此时会有这个warning。

全局去修改引入顺序比较繁琐，增加ignoreOrder: true配置，如：

```js
new MiniCssExtractPlugin({
    // ......
    ignoreOrder: true
}),
```


