### 1、constructor `super()`和`super(props)`有什么区别？

```
react 中的class 是基于es6的规范实现的, 继承是使用extends关键字实现继承的，子类必须在constructor()中调用super() 方法否则新建实例就会报错，报错的原因是 子类是没有自己的this对象的，它只能继承父类的this对象，然后对其进行加工，而super()就是将父类中的this对象继承给子类的，没有super() 子类就得不到this对象。

- 如果你使用了constructor就必须写super() 这个是用来初始化this的，可以绑定事件到this上
- 如果你想要在constructor中使用this.props,就必须给super添加参数 super(props)

注意，无论有没有 constructor，在render中的this.props都是可以使用的，这是react自动附带的
如果没有用到constructor 是可以不写的，react会默认添加一个空的constroctor.
```

### 2、react 中的事件机制
```
参考：https://juejin.im/post/6844903939092348936
```

### 3、 react 中组件通信方式有哪些？
```
props， props 回调, context, 事件订阅, redux
参考：https://www.jianshu.com/p/fb915d9c99c4
```