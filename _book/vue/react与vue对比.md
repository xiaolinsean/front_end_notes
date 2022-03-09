## react 与 vue 对比

### 1、定位：

- vue： UI层框架，渐进式

- react： UI层框架

### 2、语法

- vue： 模板申明式

- react：JSX


### 3、hooks

- vue： vue 3.0 参考 hooks 引入了 component api
- react: 16.8 开始推出 hooks, 推荐函数式写法

### 4、DOM 更新 （DOM DIFF）

- vue: vue 2.0 开始引入 DOM diff;数据双向绑定，直接修改数据即可更新view
- react: 单向数据流，要像更新 view 层，需要手动调用 setstate() 函数


### 5、文化理念


### 6、生命周期

### 7、keep-alive

- vue：支持 keep-alive， 组件切换过程，可以保存之前的组件状态
- react： 每次切换组件都是重新渲染，需要手动实现


### 8、css scope

- vue: vue 内置支持 css scope
- react: 需要借助外部工，类似style component 或者 css module