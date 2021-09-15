
<!-- 
你有了解Rxjs是什么吗？它是做什么的？
在Redux中怎么发起网络请求？
Redux怎样重置状态？
Redux怎样设置初始状态？
Context api可以取代Redux吗？为什么？
推荐在reducer中触发Action吗？为什么？
Redux怎么添加新的中间件？
redux-saga和redux-thunk有什么本质的区别？
在React中你是怎么对异步方案进行选型的？
你知道redux-saga的原理吗？
你有使用过redux-saga中间件吗？它是干什么的？
Redux中异步action和同步action最大的区别是什么？
Redux和vuex有什么区别？
Redux的中间件是什么？你有用过哪些Redux的中间件？
说说Redux的实现流程
Mobx的设计思想是什么？
Redux由哪些组件构成？
Mobx和Redux有什么区别？
在React项目中你是如何选择Redux和Mobx的？说说你的理解
你有在React中使用过Mobx吗？它的运用场景有哪些？
Redux的thunk作用是什么？
Redux的数据存储和本地储存有什么区别？
在Redux中，什么是reducer？它有什么作用？
举例说明怎么在Redux中定义action？
在Redux中，什么是action？
在Redux中，什么是store？
为什么Redux能做到局部渲染呢？
说说Redux的优缺点分别是什么？
Redux和Flux的区别是什么？
Redux它的三个原则是什么？
什么是单一数据源？
什么是Redux？说说你对Redux的理解？有哪些运用场景？

 -->




## redux中间件
中间件提供第三方插件的模式，自定义拦截 action -> reducer 的过程。变为 action -> middlewares -> reducer 。这种机制可以让我们改变数据流，实现如异步 action ，action 过滤，日志输出，异常报告等功能

redux-logger：提供日志输出
redux-thunk：处理异步操作
redux-promise：处理异步操作，actionCreator的返回值是promise


## react组件的划分业务组件技术组件？
根据组件的职责通常把组件分为UI组件和容器组件。
UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。
两者通过React-Redux 提供connect方法联系起来

## `redux-thunk`

- 基本介绍

    Redux 规定到达 reducer 的 action 必须是一个 plain object 类型， 因此 `store.dispatch(obj)` 的入参只能是一个纯对象结构，`redux-thunk` 主要的功能就是可以让我们dispatch一个函数，而不只是普通的 Object。当入参可以是函数，我们就可以在里面加很多逻辑，包括一些副作用(side effect)的事情（例如异步请求）、或者发起多个action （也就是Redux 001 号的 issue）等，增加很大的灵活性和扩展性。

- 核心代码:
    
    ```js
    function createThunkMiddleware(extraArgument) {
    return ({ dispatch, getState }) => next => action => {
        if (typeof action === 'function') {
        return action(dispatch, getState, extraArgument);
        }

        return next(action);
    };
    }

    const thunk = createThunkMiddleware();
    thunk.withExtraArgument = createThunkMiddleware;

    export default thunk;
    ```
    它的核心代码其实只有两行，就是判断每个经过它的action：如果是function类型，就调用这个function（并传入 dispatch 和 getState 及 extraArgument 为参数），如何是object类型，则直接到 reducer 中。

- 用法

    redux-thunk是作为redux的 middleware 存在的，用法和普通 middleware 的用法是一样的，注册 middleware 的代码如下：

    ```js
    import thunkMiddleware from 'redux-thunk'
    const store = createStore(reducer, applyMiddleware(thunkMiddleware))
    ```

    注册后可以这样使用：

    ```js
    // 用于发起登录请求，并处理请求结果
    // 接受参数用户名，并返回一个函数(参数为dispatch)
    const login = (userName) => (dispatch) => {
    dispatch({ type: 'loginStart' })
    request.post('/api/login', { data: userName }, () => {
        dispatch({ type: 'loginSuccess', payload: userName })
    })
    }
    store.dispatch(login('Lucy'));

    ```

参考：[理解redux-thunk](https://zhuanlan.zhihu.com/p/85403048)、[github/redux-thunk](https://github.com/reduxjs/redux-thunk)
