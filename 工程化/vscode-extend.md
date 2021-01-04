- [vscode 中使用的插件及常用技巧](#vscode-中使用的插件及常用技巧)
  - [常用插件](#常用插件)
    - [1、TODO Tree](#1todo-tree)
    - [2、Bookmarks](#2bookmarks)
    - [3、GitLens](#3gitlens)
    - [4、Git Graph](#4git-graph)
    - [5、Bracket Pair Colorizer](#5bracket-pair-colorizer)
    - [6、Highlight Matching Tag](#6highlight-matching-tag)
    - [7、Prettier](#7prettier)
    - [8、ESLint](#8eslint)
    - [9、Auto Close Tag](#9auto-close-tag)
    - [10、Auto Rename Tag](#10auto-rename-tag)
    - [11、Chinese (Simplified) Language Pack for Visual Studio Code](#11chinese-simplified-language-pack-for-visual-studio-code)
    - [12、HTML CSS Support](#12html-css-support)
    - [13、JavaScript (ES6) code snippets](#13javascript-es6-code-snippets)
    - [14、React/Redux/react-router Snippets](#14reactreduxreact-router-snippets)
    - [15、Path Intellisense](#15path-intellisense)
    - [16、Markdown All in One](#16markdown-all-in-one)
    - [17、Live Server](#17live-server)
    - [18、REST Client](#18rest-client)
  - [常用快捷键](#常用快捷键)
    - [1、Ctrl + Shift + P](#1ctrl--shift--p)
    - [2、Ctrl + P](#2ctrl--p)
    - [3、Ctrl + F](#3ctrl--f)
    - [4、Ctrl + Shift + O](#4ctrl--shift--o)

# vscode 中使用的插件及常用技巧

## 常用插件

### 1、TODO Tree

在开发或者是review代码时，经常会碰到一些待处理或者待优化的场景，这个插件能帮你组织和管理 TODO 注释, 你在代码中注释的带 TODO 的标签会统一在侧边栏显示出来，当然不限于 TODO 注释，可以自定义管理标签比如 FIXME 等，可以基于标签过滤和筛选。

插件地址：[https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree)

### 2、Bookmarks

「Bookmarks」这个插件的功能就和它名字一样直接，没错它就是你的一个源码书签，当我们看大工程项目或源码的时候，往往需要在成千上万个源文件之间跳转，Bookmarks 能帮你方便地创建和管理书签，看到哪个位置想加个书签就按快捷键 `Ctrl + Alt + K` ，再按一次就是删除。标注的书签可以在编辑器左侧统一管理，如下图：


插件地址：[https://marketplace.visualstudio.com/items?itemName=alefragnani.Bookmarks](https://marketplace.visualstudio.com/items?itemName=alefragnani.Bookmarks)


### 3、GitLens



git 的相关工具有很多了，我推荐 GitLens 的理由主要有两个，（1）在每一行代码后面都会显示该行代码的最近一次的更新记录，（2）左侧管理栏可以很方便的看到当前打开文件的修改记录和当前鼠标定位行的修改记录。很直观！

插件地址：[https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)


### 4、Git Graph


Git Graph 是一款可视化 Git 仓库的插件，类似 `SourceTree` ，Git Graph 将提交记录变成一条条时间线，分支也能清晰地用不同颜色时间线区分出来，并且点开提交线上的提交点可以查看当时的提交动作，可以在提交动作上查看做了哪些改动，也可以方便地跳转到改动文件，并且基于图中提交点提供了丰富的 Git 操作,基本覆盖了常用的操作。安装好后，点击左下角状态栏的 Git Graph 即可查看。

插件地址：[https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)


### 5、Bracket Pair Colorizer

Bracket Pair Colorizer 主要是用于高亮显示成对得大括号、花括号、小括号，点击其中一个括号，就能标识出成对的另外一个括号，并且用不同的颜色进行区分，再也不用眯着眼睛数括号是多了还是少了。

插件地址：[https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)


### 6、Highlight Matching Tag



除了匹配括号外，对于前端工程师来说，标签的匹配也是一件让人眼花的事，这里推荐 `Highlight Matching Tag` 插件，不过该插件目前支持度比较好的只有 HTML 和 JSX，默认的设置看起来不是很明显，我们可以通过自己设置来改变显示样式，可以如下配置：
```javascript
"highlight-matching-tag.highlightSelfClosing": true,
"highlight-matching-tag.styles": {
    "opening": {
        "left": {
            "custom": {
                "borderWidth": "0 0 0 2px",
                "borderStyle": "solid",
                "borderColor": "red",
                //   "borderRadius": "5px",
                "overviewRulerColor": "red"
            }
        },
        "right": {
            "custom": {
                "borderWidth": "0 0 0 2px",
                "borderStyle": "solid",
                "borderColor": "red",
                //   "borderRadius": "5px",
                "overviewRulerColor": "red"
            }
        }
    }
}
```

插件地址：[https://marketplace.visualstudio.com/items?itemName=vincaslt.highlight-matching-tag](https://marketplace.visualstudio.com/items?itemName=vincaslt.highlight-matching-tag)


### 7、Prettier

代码的格式化的插件，相对于 beautify 我更推荐 Prettier 主要用于代码的格式化，支持 JavaScript 、 TypeScript 、 Flow 、 JSX 、 JSON、CSS 、 SCSS 、 Less 、HTML 、 Vue 、 Angular 等语言，通过自己配置，可以达到想要的格式化效果，并在保存文件时，自动格式化。在团队合作项目中，各开发人员统一代码风格尤为重要。具体的配置可以参考我的另一篇文章[VSCode + ESLint + Prettier 代码语法检查和格式化](https://blog.csdn.net/u014607184/article/details/104061336)

插件地址：[https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

### 8、ESLint

说到 Prettier 代码规范化，不得不提到 ESLint，结合使用 ESLint 和 Prettier，可以达到项目统一代码规范的目的， Prettier 负责代码格式化， ESLint 负责代码语法检查， ESLint 也是可以配置化的，具体项目中的配置可以参考我的另一篇文章[VSCode + ESLint + Prettier 代码语法检查和格式化](https://blog.csdn.net/u014607184/article/details/104061336)

插件地址：[https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

### 9、Auto Close Tag

自动闭合HTML/XML标签

插件地址：[https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)

### 10、Auto Rename Tag

修改标签的一侧时，自动完成另一侧标签的同步修改

`Auto Close Tag` 和 `Auto Rename Tag` 在写HTML标签时有一定的帮助。

插件地址：[https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)

### 11、Chinese (Simplified) Language Pack for Visual Studio Code
 
此中文（简体）语言包为 VS Code 提供本地化界面。安装后，在 locale.json 中添加 "locale": "zh-cn"，即可载入中文（简体）语言包。要修改 locale.json，你可以同时按下 Ctrl+Shift+P 打开命令面板，之后输入 "config" 筛选可用命令列表，最后选择配置语言命令。


插件地址：[https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans)

### 12、HTML CSS Support

HTML标签上写class智能提示当前项目所支持的样式

插件地址：[https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css)


### 13、JavaScript (ES6) code snippets

ES6语法智能提示，以及快速输入，不仅仅支持.js，还支持.ts，.jsx，.tsx，.html，.vue，省去了配置其支持各种包含js代码文件的时间

插件地址：[https://marketplace.visualstudio.com/items?itemName=xabikos.JavaScriptSnippets](https://marketplace.visualstudio.com/items?itemName=xabikos.JavaScriptSnippets)


### 14、React/Redux/react-router Snippets

如果是使用 react 生态来开发的话，这个插件可以自动提示和快速引入。

插件地址：[https://marketplace.visualstudio.com/items?itemName=discountry.react-redux-react-router-snippets](https://marketplace.visualstudio.com/items?itemName=discountry.react-redux-react-router-snippets)


### 15、Path Intellisense

Path Intellisense 可以自动提示和补全路径。

插件地址：[https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)

### 16、Markdown All in One

- 提供了常用操作便利的快捷键
- 支持目录（Ctrl+Shift+P --> create Table of contents ）
- 一边书写一边预览(Ctrl + Shift + V or Ctrl + K V)
- 可轻松转换为HTML文件和PDF文件
- 优化了List editing的编辑
- 可格式化table (Alt + Shift + F) 以及Task list (use Alt + C to - check/uncheck a list item)
- 支持特殊数学符号渲染

插件地址：[Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)

### 17、Live Server

Live Server：一个具有实时加载功能的小型服务器，我们可以通过它来启动一个本地服务，查看基本的页面效果，`Live Server` 自带自动刷新功能，相比大项目的搭建，`Live Server` 可以用来实时查看一个简单页面的效果。

使用方法：

```markdown
1、选择需要浏览的文件，右键 --> open with live server

2、在窗口的最底部有Go Live可以点击，一旦点击，就会自动在浏览器中打开HTML文件

3、快捷键 

(alt+L, O) 打开服务   

(alt+L, C) 关闭服务

4、按F1,然后在输入栏中输入

Live Server: Open Live Server to start a server  打开服务，

或者 Live Server: Close Live Server to stop a server 关闭服务

插件地址：[Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
```

### 18、REST Client

REST Client 扩展插件，允许你发送 HTTP 请求并直接在 VS Code 上查看响应结果，这样我们对于一些基本的接口调试，我们就不必再下载安装 postman 等接口调试工具了。

基本使用：支持定义变量（方便环境切换）、创建多个请求（### 隔开）

```http
# test.http

# 开发环境
@hostname = dev.com;

#测试环境
# @hostname = test.com

### test 1
https://{{hostname}}/api/test1?rnd=1609219943516&location=1


### test2
POST  https://{{hostname}}/api/test2?rnd=1609219943298
Content-Type: application/json

{
    "data1": "111",
    "data2": "22222",
}

```
插件地址：[REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)


## 常用快捷键

### 1、Ctrl + Shift + P

`Ctrl + Shift + P` 可以打开 VSCode 命令窗口，在这个窗口下输入上述的插件名称就能知道这个插件支持哪些特性了，顺带还会说明特性快捷键。

### 2、Ctrl + P

`Ctrl + P` 文件查找。快速打开文件列表，输入关键字匹配文件，优先显示最新打开过的文件，方便地在指定文件之间跳转。

### 3、Ctrl + F

`Ctrl + F` 在当前文件里查到对应内容,编辑器左侧的搜索可以查找当前项目里的对应内容。

### 4、Ctrl + Shift + O

`Ctrl + Shift + O` 查看当前文件的符号，可以用关键字过滤符号，当然你也可以在左侧的大纲视图中查找符号，不过大纲视图不能查找匹配符号，所以我更习惯用快捷键方式查找符号。
