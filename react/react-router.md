
<!-- 
React-Router怎么获取历史对象？

React-Router怎么设置重定向？

源码：https://github.com/ReactTraining/react-router
 -->

{% raw %}


## `1、React Router v4 中使用 <Switch>关键字的作用？`


- 官网中对于 `<Switch>` 的解释： 它只会渲染第一个符合条件的 `<Route>` 或 `<Redirect>` ，避免重复匹配，`<Switch>` 需要在 `<Router>` 中使用；

- 对于 `<Route>s` 来说，如果有的链接既可以被路由A匹配，又可以被路由B匹配，那么 `Router` 会同时渲染它们，这便于我们通过多个 `<Route>` 来组合页面结构。

-------------------------------------------------------------------------------------------------------

## `2、React Router 与常规路由有何不同？`

| 主题  |	常规路由  |	React 路由|
|-----  |-------      |----       |
| 参与的页面	|    每个视图对应一个新文件	                        | 只涉及单个HTML页面 |
| URL 更改     |	HTTP 请求被发送到服务器并且接收相应的 HTML 页面	| 仅更改历史记录属性 |
| 体验	       |   用户实际在每个视图的不同页面切换	                | 用户认为自己正在不同的页面间切换 |

---------------------------------------------------------------------------------------------------------

## `3、React-Router的路由有几种模式？`

- `hash` 模式

    - 改变 `hash` 方法

        直接改变 `window.location.hash` 的值、
        
        通过 a 标签 
        ```html 
        <a href="#login">edit</a>
        ```

    - 监听 `hash`

     ```js
     window.onhashchange = function(hash) {
        console.log(hash)
    }
    window.addEventListener('hashchange', function(hash) {
        console.log(hash)
    })

     ```

    - `hash` 的劣势：
        - 最直观的，多个#看着就是难受，那我不管真的感觉丑（把肤浅打在公屏上）
        - 配合后端困难 对于部分需要重定向的操作，后端无法获取hash部分内容，导致后台无法取得url中的数据
        - 服务器端无法准确跟踪前端路由信息

- `history` 模式

    - 这个模式就是基于HTML5的 `History` 接口，主要方法有以下几个：
        ```js
        History.forward() // 前进
        History.back()  // 后退
        History.go()   // 自定义前进或后退步长
        History.pushState(state,title,path) // history.length + 1
        History.replaceState(state,title,path) // history.length 不变
        ```

        - `state`：一个与指定网址相关的状态对象， popstate 事件触发时，该对象会传入回调函数。如果不需要可填 null。

        - `title`：新页面的标题，但是所有浏览器目前都忽略这个值，可填 null。

        - `path`：新的网址，必须与当前页面处在同一个域。浏览器的地址栏将显示这个地址。

    - 监听路由

        ```js
        window.addEventListener('popstate',function(e){
            /* 监听改变 */
            console.log(e.state);
        })
        ```
        需要注意的是：用 history.pushState() 或者 history.replaceState() 不会触发 popstate 事件。 popstate 事件只会在浏览器某些行为下触发, 比如点击后退、前进按钮或者调用 history.back()、history.forward()、history.go()方法。

参考：[不看看react-router源码？真的懂路由咩](https://juejin.cn/post/6872752069766283271) 

-----------------------------------------------------------------------------------------------------

## `4、React-Router 4 怎样在路由变化时重新渲染同一个组件？`

- 在类似 componentsWillReceiveProps （getStateFromProps、useEffect）生命周期判断 props.location.pathname 相对于上一次是否改变；

- 在组件上加上对应的 key , key 为对应路由变化参数； 

参考：[react-router4怎么在路由变化时重新渲染同一个组件？](https://segmentfault.com/q/1010000011739119)

------------------------------------------------------------------------------------------------------

## `5、React-Router的<Link>、标签和<a>标签有什么区别？`

- `<a>` 标签: 页面的跳转，页面跳转后会导致整个页面重新渲染；

- `<Link>`： `React-Router-dom` 中提供的组件，提供页面内组件间的跳转，可以实现页面局部更新，提高性能；
            Link 组件最终会渲染为 HTML 标签 `<a>`，它的 to、query、hash 属性会被组合在一起并渲染为 href 属性。虽然 Link 被渲染为超链接，但在内部实现上拦截了浏览器的默认行为，然后调用了history.pushState（history.replaceState） 方法;

- `<NavLink>`： `<NavLink>` 是 `<Link>` 的一个特定版本，会在匹配上当前的url的时候给已经渲染的元素添加参数，组件的属性有：activeClassName、activeStyle、exact、strict

------------------------------------------------------------------------------------------------------

## `6、react-router 中 history 路由配置

- 本地开发调试（webpack-dev-server）

    当使用 HTML5 History API 时, 所有的 404 请求都会响应 index.html 的内容。 将 `devServer.historyApiFallback` 设为 true开启：

    ```js
    module.exports = {
    //...
        devServer: {
            historyApiFallback: true
        }
    };

    ```
    也可以通过配置，指定具体的 rewrite

    ```js
    module.exports = {
    //...
        devServer: {
            historyApiFallback: {
            rewrites: [
                { from: /^\/$/, to: '/views/landing.html' },
                { from: /^\/subpage/, to: '/views/subpage.html' },
                { from: /./, to: '/views/404.html' }
            ]
            }
        }
    };

    ```

    参考：[devServer.historyApiFallback](https://webpack.docschina.org/configuration/dev-server/#devserverhistoryapifallback)

- nginx 部署

    - 部署到nginx根目录，只需要对应的NGINX配置即可：

        ```shell
        # nginx配置
        location / {
            root   html;
            index  index.html;
            # url 切换时始终返回index.html
            try_files $uri /index.html;
        }
        ```
    
    - 部署到nginx子目录

        假设部署到/app/build/目录下，访问路径：http://localhost/app/build/

        ```shell
        # nginx配置
        location /app/build/ {
            root   html;
            index  index.html;
            # url 切换时始终返回index.html
            try_files $uri /app/build/index.html;
        }
        ```

        ```js
        // package.json
        "homepage": "http://localhost/app/build/",
        ```

        ```jsx
        // react-router路由配置
        // 注意指定basename，如果本地和服务端路径不一致，可以通过process.env.NODE_ENV === 'production' 来判断区分
        <BrowserRouter basename='/app/build/'>
        </BrowserRouter>
        ```


------------------------------------------------------------------------------------------------------

## `7、react-router 中根据路由按需加载`

在浏览器端，可以使用 `React.lazy` 和 `Suspense`，  `React.lazy` 函数能让你像渲染常规组件一样处理动态引入（的组件）。`Suspense` 组件中渲染 lazy 组件，使得我们可以使用在等待加载 lazy 组件时做优雅降级（如 loading 指示器等）。具体使用代码如下：

```js
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={lazy(() => import(/* webpackChunkName: "Home" */ './routes/Home'))}/>
        <Route path="/about" component={lazy(() => import(/* webpackChunkName: "About" */ './routes/About'))}/>
      </Switch>
    </Suspense>
  </Router>
);
```
- 通过 `webpackChunkName` ,可以设置分割后每一个文件的文件名；

- `Suspense` 可以放在懒加载组件之上的任何位置， 可以通过包含多个懒加载组件，fallback 属性接受任何在组件加载过程中你想展示的 React 元素。

- `React.lazy` 目前只支持默认导出（default exports）。如果你想被引入的模块使用命名导出（named exports），你可以创建一个中间模块，来重新导出为默认模块。

- `React.lazy` 和 `Suspense` 技术还不支持服务端渲染。如果你想要在使用服务端渲染的应用中使用，推荐 `Loadable Components` 这个库.

------------------------------------------------------------------------------------------------------


## `8、React-Router 怎么传递参数？`

- 1.通配符传参

    Route定义方式：
    ```js
    <Route path='/path/:name' component={Path}/>
    ```

    Link组件：
    ```js
    <Link to="/path/通过通配符传参">通配符</Link>
    ```

    参数获取：
    ```js
    this.props.match.params.name
    ```

    特点：刷新页面时不会丢失，但是会拼接在URL中，只能传字符串，如果需要传对象的话，可以使用`JSON.stringify()/JSON.parse()`。

- 通过 `<Link>` 中的 search

    Link组件：
    ```js
        <Link className="link" to={{pathname:'/path',search:'?a=1'}}>path</Link>
    ```

    参数获取：
        ```js
        this.props.location.search
        ```

    特点：只能传字符串，且刷新页面时不会丢失，但是会拼接在URL中，只能传字符串，如果需要传对象的话，可以使用`JSON.stringify()/JSON.parse()`。

- 通过 `<Link>` 中的 state

    Link组件：
    ```js
    <Link className="link" to={{pathname:'/path',state:{text:'text'}}}>path</Link>
    ```

    参数获取：
        ```js
        this.props.location.state
        ```

    特点：可传对象，使用场景仅限于从主页面进入子页面场景（可在子页面刷新），单独进去子页面失效

- 通过 `<Link>` 中的 query
  
    Link组件：
    ```js
    <Link className="link" to={{pathname:'/path',query:"b=2"}}>path</Link>
    ```

    参数获取：
        ```js
        this.props.location.query
        ```

    特点：可传对象，使用场景仅限于从主页面进入子页面场景（可在子页面刷新），单独进去子页面失效



{% endraw %}