- [1、constructor `super()`和`super(props)`有什么区别？](#1constructor-super和superprops有什么区别)
- [2、react 中的事件机制](#2react-中的事件机制)
- [3、 react 中组件通信方式有哪些？](#3-react-中组件通信方式有哪些)
- [4、react 中的 setState 的第一个参数和第二个参数分别是什么？](#4react-中的-setstate-的第一个参数和第二个参数分别是什么)
- [5、受控组件和非受控组件的区别？](#5受控组件和非受控组件的区别)
- [6、简述一下 react 的生命周期](#6简述一下-react-的生命周期)
- [7、新旧生命周期对比，新增了哪些生命周期？ 哪些生命周期是不建议使用的？](#7新旧生命周期对比新增了哪些生命周期-哪些生命周期是不建议使用的)
- [8、getDerivedStateFromError() 和 componentDidCatch() 分别是什么作用，有什么区别？](#8getderivedstatefromerror-和-componentdidcatch-分别是什么作用有什么区别)
- [9、React 中的 PureComponent 和 Component 有什么区别？](#9react-中的-purecomponent-和-component-有什么区别)
- [10、react 中的列表 key 属性的作用？](#10react-中的列表-key-属性的作用)
- [11、react 的 Virtual DOM](#11react-的-virtual-dom)
- [12、react 中的 dom-diff 大概原理？](#12react-中的-dom-diff-大概原理)
- [13、 React Fiber](#13-react-fiber)
- [14、react 中的高阶组件 // TODO](#14react-中的高阶组件--todo)
- [15、react 中的 context // TODO](#15react-中的-context--todo)
- [16、react 中的 refs 和 refs 转发](#16react-中的-refs-和-refs-转发)
- [17、react 中使用 refs 和 js 原生获取 DOM 节点（类似 document.getElementById）的对比, 为什么建议使用 refs](#17react-中使用-refs-和-js-原生获取-dom-节点类似-documentgetelementbyid的对比-为什么建议使用-refs)
- [18、用过哪些 hooks ？](#18用过哪些-hooks-)
- [19、hooks 相对于传统 class 有哪些特点？](#19hooks-相对于传统-class-有哪些特点)
- [20、hooks 如何模拟 class 的各个生命周期？](#20hooks-如何模拟-class-的各个生命周期)
- [21、hooks 中的 effect 什么时候执行？ effect 的清除函数什么时候执行？](#21hooks-中的-effect-什么时候执行-effect-的清除函数什么时候执行)
- [22、useMemo 和 useCallback ? 为什么能进行性能优化？](#22usememo-和-usecallback--为什么能进行性能优化)


## 1、constructor `super()`和`super(props)`有什么区别？

    react 中的class 是基于es6的规范实现的, 继承是使用extends关键字实现继承的，子类必须在constructor()中调用super() 方法否则新建实例就会报错，报错的原因是 子类是没有自己的this对象的，它只能继承父类的this对象，然后对其进行加工，而super()就是将父类中的this对象继承给子类的，没有super() 子类就得不到this对象。

    - 如果你使用了constructor就必须写super() 这个是用来初始化this的，可以绑定事件到this上
    - 如果你想要在constructor中使用this.props,就必须给super添加参数 super(props)

    注意，无论有没有 constructor，在render中的this.props都是可以使用的，这是react自动附带的
    如果没有用到constructor 是可以不写的，react会默认添加一个空的constroctor.


## 2、react 中的事件机制

    react的事件是合成事件((Synethic event)，模拟了事件冒泡和捕获的过程，采用了事件代理，批量更新等方法，并且抹平了各个浏览器的兼容性问题。

    （1）合适事件全部委托到document上，而原生事件绑定到DOM元素本身，

    （2）执行顺序：先执行原生事件，事件冒泡至document，再执行合成事件，子元素原生事件 -> 父元素原生事件 -> 子元素合成事件 -> 父元素合成事件 -> 执行真正在document上挂载的事件

    （3）事件注册：在 React 组件挂载阶段，根据组件内的声明的事件类型（onclick、onchange 等），在 document 上注册事件（使用addEventListener），并指定统一的回调函数 dispatchEvent。对于同一种事件类型，不论在 document 上注册了几次，最终也只会保留一个有效实例，这能减少内存开销。

    （4）回调函数处理：React 为了在触发事件时可以查找到对应的回调去执行，会把组件内的所有事件统一地存放到一个对象中（listenerBank）。首先会根据事件类型分类存储，例如 click 事件相关的统一存储在一个对象中，回调函数的存储采用键值对（key/value）的方式存储在对象中，key 是组件的唯一标识 id，value 对应的就是事件的回调函数。listenerBank 结构类似：

        {
            onClick:{
                nodeid1:()=>{...}
                nodeid2:()=>{...}
            },
            onChange:{
                nodeid3:()=>{...}
                nodeid4:()=>{...}
            }
        }
    
    （5）事件派发：
        1. 找到事件触发的 DOM 和 React Component
        2. 从该 React Component，调用 findParent 方法，遍历得到所有父组件，存在数组中。
        3. 从该组件直到最后一个父组件，根据之前事件存储，用 React 事件名 + 组件 key，找到对应绑定回调方法，执行，详细过程为：
        4. 根据 DOM 事件构造 React 合成事件。
        5. 将合成事件放入队列。
        6. 批处理队列中的事件（包含之前未处理完的，先入先处理）

    （6）阻止冒泡：React合成事件的冒泡并不是真的冒泡，而是节点的遍历。因此，阻止合成事件的冒泡不会阻止原生事件的冒泡，但是阻止原生事件的冒泡会阻止合成事件的冒泡。


参考：[https://zhuanlan.zhihu.com/p/49067231](https://zhuanlan.zhihu.com/p/49067231)、[https://www.cnblogs.com/forcheng/p/13187388.html](https://www.cnblogs.com/forcheng/p/13187388.html)、[React 事件系统工作原理](https://juejin.cn/post/6909271104440205326)


## 3、 react 中组件通信方式有哪些？

    （1）父组件向子组件通信: props

    （2）子组件向父组件通信：props 回调

    （3）跨级组件间通信： props/context

    （4）非嵌套组件间通信： 事件订阅、redux

参考：[https://www.jianshu.com/p/fb915d9c99c4](https://www.jianshu.com/p/fb915d9c99c4)



## 4、react 中的 setState 的第一个参数和第二个参数分别是什么？

    setState 第一个参数可以是object，也可以是函数（该函数中接收的 state 和 props 都保证为最新），传函数的目的可以保证 setState 合并执行时，函数所获取的 state 和 props 都为最新的，

    setState 第二个参数是回调函数，回调函数会在state更新后执行，在该回调函数中，我们取到的是更新后的state值。
    关于setState使用时的注意事项可参考：https://blog.csdn.net/u014607184/article/details/104581487

## 5、受控组件和非受控组件的区别？

    受控组件 受控组件的数据是由 React 组件来管理的，所以要为每个状态更新编写数据处理函数，类似对于 input，我们将其变更的值维护在 state 中。受控组件的值变更都会触发 render。

    非受控组件 非受控组件将真实数据储存在 DOM 节点中，直接由 DOM 节点来处理，所以其值改变时，并不会触发 react 的 render。一般我们通过ref来获取其最终的值；

    demo: https://github.com/xiaolinsean/react-learn/blob/master/UncontrolledComponent/src/index.jsx

## 6、简述一下 react 的生命周期

    V16.3之前的版本：

    初始化挂载阶段：constructor()、componentWillMount，render，componentDidMount

    更新阶段：componentWillReceiveProps，shouldComponentUpdate，componentWillUpdate，render，componentDidUpdate

    卸载阶段：componentWillUnmount


    V16.3之后版本：

    初始化挂载阶段：constructor()、static getDerivedStateFromProps()，render，componentDidMount

    更新阶段：static getDerivedStateFromProps()，shouldComponentUpdate，getSnapshotBeforeUpdate() ，render，componentDidUpdate

    卸载阶段：componentWillUnmount


## 7、新旧生命周期对比，新增了哪些生命周期？ 哪些生命周期是不建议使用的？

    新增的：static getDerivedStateFromProps()、getSnapshotBeforeUpdate()

    不建议使用：render之前的几个生命周期函数：componentWillMount、componentWillReceiveProps、componentWillUpdate

    不建议使用的三个生命周期函数在17.0版本之后将不再支持，如果非要使用，只能在前面加'UNSAFE_'


## 8、getDerivedStateFromError() 和 componentDidCatch() 分别是什么作用，有什么区别？

    getDerivedStateFromError() 是静态方法，需要在前面加 static 使用，用于捕获子组件中的报错，从而用于展示备用的错误提示UI。

    componentDidCatch(error, info) 主要是用于搜集页面中的报错信息，可以记录报错信息以及引发错误的相关信息。


## 9、React 中的 PureComponent 和 Component 有什么区别？ 

    PureComponent 其实是在内部帮我们简单实现了一下shouldComponentUpdate的功能，以便提供组件的性能；这里的简单指是：对prop和state做浅比较。

    使用 PureComponent 时的注意事项：

    PureComponent主要针对prop和state为基本数据类型，如bool、string、number，对于复杂的数据类型，因为是做的浅比较，所以始终不会更新，可以自己定义shouldComponentUpdate

    PureComponent 中不建议再另外重写shouldComponentUpdate方法；

    PureComponent的最好作为展示组件，如果prop和state每次都会变，PureComponent做浅比较也会影响性能，可以考虑直接用Component；


## 10、react 中的列表 key 属性的作用？

    简单来说，react利用key来识别组件，它是一种身份标识，用来唯一识别组件，在更新时：

    key相同：若组件属性有所变化，则react只更新组件对应的属性；没有变化则不更新。

    key值不同：则react先销毁该组件(有状态组件的componentWillUnmount会执行)，然后重新创建该组件（有状态组件的constructor和componentWillUnmount都会执行）

    使用 key 的注意事项：

    （1）在数组中生成的每项都要有key属性，并且key的值是一个永久且唯一的值，即稳定唯一。因此不建议使用数据索引作为 key， 另外 数组生成的同级同类型的组件上要保持唯一，而不是所有组件的key都要保持唯一；

    （2）key 属性是添加到自定义的子组件上，而不是子组件内部的顶层的组件上。

## 11、react 的 Virtual DOM 

    Virtual DOM 本质上就是在 JS 和真实 DOM 之间做了一个缓存，js只操作 Virtual DOM

    （1）用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中
    （2）当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异
    （3）把2所记录的差异应用到步骤1所构建的真正的DOM树上，视图就更新了


参考:[https://juejin.im/post/6844903772943024141](https://juejin.im/post/6844903772943024141)、[https://www.jianshu.com/p/bef1c1ee5a0e](https://www.jianshu.com/p/bef1c1ee5a0e)、[https://www.jianshu.com/p/b189b2949b33](https://www.jianshu.com/p/b189b2949b33)

## 12、react 中的 dom-diff 大概原理？ 

    传统 diff 算法的复杂度为 O(n^3)，React 通过制定策略，将 O(n^3) 复杂度的问题转换成 O(n) 复杂度的问题：其 diff 算法策略是基于以下三个前提约定的：

    （1）Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计。

        react diff 只会比较同级的节点，如果不存在则删除该节点，不做跨层级的比较，如果出现节点跨层级的移动，则先删除原节点（及其子节点），再在新的位置创建新节点（及其子节点），因此我们开发过程中，应该尽量减少节点的跨层级移动。

    （2）拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结构。

        如果是同一类型的组件，按照原策略继续比较 virtual DOM tree。

        如果不是，则将该组件判断为 dirty component，从而替换整个组件下的所有子节点。

        对于同一类型的组件，有可能其 Virtual DOM 没有任何变化，如果能够确切的知道这点那可以节省大量的 diff 运算时间，因此 React 允许用户通过 shouldComponentUpdate() 来判断该组件是否需要进行 diff。

    （3）对于同一层级的一组子节点，它们可以通过唯一 id 进行区分。

        当节点处于同一层级时，React diff 提供了三种节点操作，分别为：INSERT_MARKUP（插入）、MOVE_EXISTING（移动）和 REMOVE_NODE（删除）。react 建议开发者在同一级元素中加 key， 以便快速定位旧集合中是否有新集合中的节点，以便于决定进行哪类操作。

    这三个约定，分别对 tree diff、component diff 以及 element diff 进行算法优化。


参考：[https://zhuanlan.zhihu.com/p/20346379](https://zhuanlan.zhihu.com/p/20346379)、[https://segmentfault.com/a/1190000018914249](https://segmentfault.com/a/1190000018914249)

## 13、 React Fiber 

    Fiber 是 React 16 中新的协调引擎。它的主要目的是使 Virtual DOM 可以进行增量式渲染。

    React将虚拟DOM的更新过程划分两个阶段，reconciler阶段与commit阶段。
    reconciler阶段对应早期版本的diff过程，该阶段可以被中断，因此可能执行多次
    commit阶段对应早期版本的patch过程，该阶段不可以中断

参考：[https://zhuanlan.zhihu.com/p/37095662](https://zhuanlan.zhihu.com/p/37095662)

## 14、react 中的高阶组件 // TODO

    高阶组件是参数为组件，返回值为新组件的函数。

    常用场景：
    （1）抽取重复代码，实现组件复用，常见场景：页面复用。
    （2）条件渲染，控制组件的渲染逻辑（渲染劫持），常见场景：权限控制。
    （3）捕获/劫持被处理组件的生命周期，常见场景：组件渲染性能追踪、日志打点。

参考：[https://juejin.im/post/6844904050236850184](https://juejin.im/post/6844904050236850184)


## 15、react 中的 context // TODO

    Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props。同时，这也使得组件的复用性变差，使用时需谨慎。

参考：[官网context](https://zh-hans.reactjs.org/docs/context.html)


## 16、react 中的 refs 和 refs 转发

    react 中的 Refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素。

    （1）当 ref 属性用于 HTML 元素时，构造函数中使用 React.createRef() 创建的 ref 接收底层 DOM 元素作为其 current 属性。

    （2）当 ref 属性用于自定义 class 组件时，ref 对象接收组件的挂载实例作为其 current 属性，即组件实例。

    （3）不能在函数组件上使用 ref 属性，因为他们没有实例。可以使用 forwardRef

    使用方式：

    （1）使用 React.createRef() 创建

        class MyComponent extends React.Component {
            constructor(props) {
                super(props);
                this.myRef = React.createRef();
            }
            render() {
                return <div ref={this.myRef} />; // 通过 this.myRef.current 可访问到对应的节点
            }
        }

    （2）回调 Refs

        class MyComponent extends React.Component {
            constructor(props) {
                super(props);
            }
            render() {
                return <div ref={el => this.myRef = el} />; // 通过 this.myRef 可访问到对应的节点
            }
        }

## 17、react 中使用 refs 和 js 原生获取 DOM 节点（类似 document.getElementById）的对比, 为什么建议使用 refs 

    （1）每个组件类都可以有多个组件实例。这样就不能保证同一个页面中，元素ID的唯一性，此时，使用 document.getElementById 就会出问题；

    （2）使用 refs 可以保证单向数据流：使用refs的另一个好处是，在设计中，你只能在定义它的上下文中访问refs。如果你需要在这个上下文之外访问信息，这会强制你使用props和state


## 18、用过哪些 hooks ？
    基础 Hook
        useState
        useEffect
        useContext

    额外的 Hook
        useReducer
        useCallback
        useMemo
        useRef
        useImperativeHandle
        useLayoutEffect
        useDebugValue


## 19、hooks 相对于传统 class 有哪些特点？

    （1）代码更加简洁；

    （2）传统 class 组件是以生命周期函数来组合不同的逻辑，而 react Hooks 是以逻辑代码来划分的，这样保证独立的逻辑是独立的，在代码层次上也更独立清晰；

    （3）函数组件其根本是一个函数，每次重新执行时，DOM 都已经更新完毕，获取到 state 是最新的，且 effect 每次执行时，都会清除之前绑定的函数，重新绑定，这样保证了state 和 props 是最新的，特别是在有需要清除事件的场景下，相对于传统的 class 需要在不同的生命周期里进行处理，effect 可以自动帮我们处理掉。

    （4）通过不同的 hooks api 组合，我们同样可以实现传统 class 组件中的大部分生命周期场景（getSnapshotBeforeUpdate，componentDidCatch 以及 getDerivedStateFromError：目前还没有这些方法的 Hook 等价写法）


## 20、hooks 如何模拟 class 的各个生命周期？

    constructor：函数组件不需要构造函数。你可以通过调用 useState 来初始化 state。如果计算的代价比较昂贵，你可以传一个函数给 useState。

    getDerivedStateFromProps：改为 在渲染时 安排一次更新。

    shouldComponentUpdate：详见 下方 React.memo.

    render：这是函数组件体本身。

    componentDidMount, componentDidUpdate, componentWillUnmount：useEffect Hook 可以表达所有这些(包括 不那么 常见 的场景)的组合。

    getSnapshotBeforeUpdate，componentDidCatch 以及 getDerivedStateFromError：目前还没有这些方法的 Hook 等价写法，但很快会被添加。

参考：[React Hooks 介绍及与传统 class 组件的生命周期函数对比](https://blog.csdn.net/u014607184/article/details/109744910)

## 21、hooks 中的 effect 什么时候执行？ effect 的清除函数什么时候执行？

    一般情况下 effect 会在初始化和每次更新的时候都会运行；这里还需要注意两点：

    （1）传递给 useEffect 的函数在每次渲染中都会有所不同，这是刻意为之的。事实上这正是我们可以在 effect 中获取最新的 count 的值，而不用担心其过期的原因。每次我们重新渲染，都会生成新的 effect，替换掉之前的。

    （2） effect 的清除阶段在每次重新渲染时都会执行，而不是只在卸载组件的时候执行一次。

    我们可以通过不同的处理方法，来模拟不同的时机去执行 effect：

    （1）effect 第二个参数传[]，只会初始化时执行一次，模拟DidMount;

    （2）利用 useRef() 生产实力变量，通过判断实例变量是否存在，来模拟 DidUpdate;

    （3）effect 中返回的函数，模拟 WillUnMount;

    （4）effect 第二个参数 数组中传对应的参数，可以做性能优化，只在该参数变化时，才会重新执行；

## 22、useMemo 和 useCallback ? 为什么能进行性能优化？

    使用了 Memoization 思想，即对于一些纯函数（无状态，输出只依赖输入，相同的输入会得到相同的输出）的复杂操作或者运算，第一次调用时，会把运算结果缓存起来，当下次相同入参调用时，直接返回上一次运算的结果，以实现性能优化，因为缓存上次的运算结果需要暂用额外的内存，所以这种思想是以空间代价换取时间上的优化。

     useMemo 返回一个 memoized 值。把“创建”函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时才重新计算 memoized 值。这种优化有助于避免在每次渲染时都进行高开销的计算。

     useCallback 返回一个 memoized 回调函数。把内联回调函数及依赖项数组作为参数传入 useCallback，它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新。当你把回调函数传递给经过优化的并使用引用相等性去避免非必要渲染（例如 shouldComponentUpdate）的子组件时，它将非常有用。

参考：[了解JavaScript中的Memoization以提高性能,再看React的应用](https://juejin.im/post/6844903720610693127)
