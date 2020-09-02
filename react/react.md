## 1、constructor `super()`和`super(props)`有什么区别？

    react 中的class 是基于es6的规范实现的, 继承是使用extends关键字实现继承的，子类必须在constructor()中调用super() 方法否则新建实例就会报错，报错的原因是 子类是没有自己的this对象的，它只能继承父类的this对象，然后对其进行加工，而super()就是将父类中的this对象继承给子类的，没有super() 子类就得不到this对象。

    - 如果你使用了constructor就必须写super() 这个是用来初始化this的，可以绑定事件到this上
    - 如果你想要在constructor中使用this.props,就必须给super添加参数 super(props)

    注意，无论有没有 constructor，在render中的this.props都是可以使用的，这是react自动附带的
    如果没有用到constructor 是可以不写的，react会默认添加一个空的constroctor.


## 2、react 中的事件机制

    参考：https://juejin.im/post/6844903939092348936


## 3、 react 中组件通信方式有哪些？

    props， props 回调, context, 事件订阅, redux
    参考：https://www.jianshu.com/p/fb915d9c99c4



## 4、react 中的 setState 的第一个参数和第二个参数分别是什么？

    setState 第一个参数可以是object，也可以是函数（该函数中接收的 state 和 props 都保证为最新），传函数的目的可以保证 setState 合并执行时，函数所获取的 state 和 props 都为最新的，

    setState 第二个参数是回调函数，回调函数会在state更新后执行，在该回调函数中，我们取到的是更新后的state值。
    关于setState使用时的注意事项可参考：https://blog.csdn.net/u014607184/article/details/104581487

## 5、受控组件和非受控组件的区别？

    受控组件 受控组件的数据是由 React 组件来管理的，所以要为每个状态更新编写数据处理函数，类似对于 input，我们将其变更的值维护在 state 中。受控组件的值变更都会触发 render。

    非受控组件 非受控组件将真实数据储存在 DOM 节点中，直接由 DOM 节点来处理，所以其值改变时，并不会触发 react 的 render。一般我们通过ref来获取其最终的值；

    demo: https://github.com/xiaolinsean/react-learn/blob/master/UncontrolledComponent/src/index.jsx

