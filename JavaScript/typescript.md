

## 1、TypeScript 有什么不同的地方？

TypeScript 是 JavaScript 的超集，与现存的 JavaScript 代码有非常高的兼容性，也就是说，TypeScript 包含了 JavaScript 的 all。

- TypeScript 相比于 JavaScript 具有以下优势

    - 更好的可维护性和可读性

    - 引入了静态类型声明，不需要太多的注释和文档，大部分的函数看类型定义就知道如何使用了

    - 在编译阶段就能发现大部分因为变量类型导致的错误

- 使用难点：
    
    - 类型定义：对于每一个变量都需要定义它的类型，特别是对于一个对象而言，可能需要定义多层类型（这也是为什么会出现 AnyScript 的原因。。。）

    - 引用三方类库：第三方库如果不是 TypeScript 写的，没有提供声明文件，就需要去为第三方库编写声明文件

    - 新概念：TypeScript 中引入的类型(Types)、类(Classes)、泛型(Generics)、接口(Interfaces)以及枚举(Enums)，这些概念如果之前没有接触过强类型语言的话，就需要增加一些学习成本


![logo](http://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Markdown-mark.svg/208px-Markdown-mark.svg.png)
