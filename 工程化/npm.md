## `1、npm 发布自己的包`

假设你的项目已经搭建完成，`package.json` 也已经存在（如果不存在，可通过 `npm init` 生成）

- 添加账号信息

首先，需要在[https://www.npmjs.com/](www.npmjs.com)注册一个账号，私有仓库也需要先注册一个账号；

```js
// 采取问答式 添加账号信息
$ npm adduser
Username: your name
Password: your password
Email: yourmail@gmail.com

// 如果你是私有库，则需要加上对应的私有库地址
$ npm adduser -- --registry http://your_npm_url

```

- 发布 

```js

npm publish

// 如果你是私有库，则需要加上对应的私有库地址
npm publish --registry http://your_npm_url
```

下次修改后，需要改一下版本号（version），然后再 `publish` ，主要规则版本格式：主版本号.次版本号.修订号，版本号递增规则如下：

```markdown
- 主版本号：当你做了不兼容的 API 修改，
- 次版本号：当你做了向下兼容的功能性新增，
- 修订号：当你做了向下兼容的问题修正。
```

---------------------------------------------------------------------------------------------

## `2、npm 删除自己的包`

- 强制删除

```js
npm unpublish --force //强制删除

// 如果你是私有库，则需要加上对应的私有库地址
npm unpublish --force --registry http://your_npm_url
```

- 删除指定版本号

```js
npm unpublish npm_name@1.0.1 //指定版本号

// 如果你是私有库，则需要加上对应的私有库地址
npm unpublish npm_name@1.0.1 --registry http://your_npm_url
```

---------------------------------------------------------------------------------------------


## `3、搭建自己的 npm 库`


[https://zhuanlan.zhihu.com/p/134603457](https://zhuanlan.zhihu.com/p/134603457)


---------------------------------------------------------------------------------------------

## `4、npm link 的作用`

在本地开发npm模块的时候，我们可以使用npm link命令，将npm 模块链接到对应的运行项目中去，方便地对模块进行调试和测试。

例如我们现在有两个项目 `link-module` 和 `test-project`;

```js

cd link-module
npm link

```
link-module会根据 `package.json上` 的配置，被链接到全局，路径是`{prefix}/lib/node_modules/<package>`。

```js
cd test-project
npm link link-module

```
`link-module` 会被链接到 `test-project/node_modules`下面，需要注意的是 `package-name` 是依据 `package.json` 的 `name` 而非文件名称。

这样，所有对 `link-module` 的修改会被直接映射到 `test-project/node_modules/link-module` 下面，这样对于本地调试很方便，而不需要每次有修改都需要重新发版再安装。

如果想解除项目与模块的依赖则可以在项目目录下执行 `npm unlink link-module` 即可。

如果想要从全局环境移除 `link-module` 模块链接，则可以在该模块目录下执行 `npm unlink link-module` 即可。


---------------------------------------------------------------------------------------------