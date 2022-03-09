- [`1、TypeScript 有什么不同的地方？`](#1typescript-有什么不同的地方)
- [`2、React.ComponentProps 泛型工具`](#2reactcomponentprops-泛型工具)
- [`3、Pick, Omit 等`](#3pick-omit-等)
- [`4、`](#4)

参考：[React & Redux in TypeScript - Complete Guide](https://github.com/piotrwitek/react-redux-typescript-guide#react---type-definitions-cheatsheet)


## `1、TypeScript 有什么不同的地方？`

TypeScript 是 JavaScript 的超集，与现存的 JavaScript 代码有非常高的兼容性，也就是说，TypeScript 包含了 JavaScript 的 all。

- TypeScript 相比于 JavaScript 具有以下优势

    - 更好的可维护性和可读性

    - 引入了静态类型声明，不需要太多的注释和文档，大部分的函数看类型定义就知道如何使用了

    - 在编译阶段就能发现大部分因为变量类型导致的错误

- 使用难点：
    
    - 类型定义：对于每一个变量都需要定义它的类型，特别是对于一个对象而言，可能需要定义多层类型（这也是为什么会出现 AnyScript 的原因。。。）

    - 引用三方类库：第三方库如果不是 TypeScript 写的，没有提供声明文件，就需要去为第三方库编写声明文件

    - 新概念：TypeScript 中引入的类型(Types)、类(Classes)、泛型(Generics)、接口(Interfaces)以及枚举(Enums)，这些概念如果之前没有接触过强类型语言的话，就需要增加一些学习成本

------------------------------------------------------------------------------------------------------

## `2、React.ComponentProps 泛型工具`

`React.ComponentProps` 可以帮助我们获取一个组件的 props 类型声明，其具体定义：

```jsx
type ComponentProps<T extends keyof JSX.IntrinsicElements | JSXElementConstructor<any>> =
    T extends JSXElementConstructor<infer P>
        ? P
        : T extends keyof JSX.IntrinsicElements
            ? JSX.IntrinsicElements[T]
            : {};
```

具体使用方法如下：

```jsx
import * as React from 'react';
import { Button } from 'antd';

type ButtonProps = React.ComponentProps<typeof Button>;
```

这种方法相较于原来的方法：

```jsx
import { ButtonProps } from 'antd/lib/button';
```
不仅格式上更简洁，更重要的一点是获取到的是真实的类型声明，而且是完整的，原来的方法有时候是不完整的。

------------------------------------------------------------------------------------------------------


## `3、Pick, Omit 等`

TS提供了几种内置的预定义的条件类型，需要注意对应的支持版本号

Pick<T> - 用于从类型T中取出指定的成员
Omit<T> - 与 Pick 效果正好相反，用于从类型T中取出指定成员除外的成员
Exclude<T, U> - 用于从类型T中去除在U类型中的成员
Extract<T, U> - 用于从类型T中取出可分配给U类型的成员
NonNullable<T> - 用于从类型T中去除undefined和null类型
ReturnType<T> - 获取函数类型的返回类型
InstanceType<T> - 获取构造函数的实例类型

```jsx
interface Card {
    id: string
    isFeatured?: boolean
    name: string
}

interface DataCardProps {
    cardData: Card[]
    handleSelect: (id: string) => void
    titleText: string
}

/**
 * {
 *   cardData: Card[]
 * }
*/
type CardData = Pick<DataCardProps, 'cardData'>

/*
 * {
 *   cardData: Card[]
 *   titleText: string
 * }
 */
type CardDataWithTitle = Pick<DataCardProps, 'cardData' | 'titleText'>

/*
 * {
 *   handleSelect: (id: string) => void
 *   titleText: string
 * }
 */
type OtherData = Omit<DataCardProps, 'cardData'>


interface Page {
    id: string
    name: string
}

type Keys = Extract<keyof Card, keyof Page> // id, name

type Keys2 = Exclude<keyof Card, keyof Page> // isFeatured
```

参考：
- [TypeScript Utility Types Part 1: Partial, Pick, and Omit](https://www.dslemay.com/blog/2020/04/27/typescript-utility-types-part-1-partial-pick-and-omit)
- [TypeScript Utility Types Part 2: Record, Readonly, & Required](https://www.dslemay.com/blog/2020/05/04/typescript-utility-types-part-2-record-readonly-and-required)
- [TypeScript Utility Types Part 3: Extract, Exclude, and NonNullable](https://www.dslemay.com/blog/2020/05/25/typescript-utility-types-part-3-extract-exclude-and-nonnullable)

------------------------------------------------------------------------------------------------------

## `4、`

------------------------------------------------------------------------------------------------------