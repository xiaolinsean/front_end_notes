
- [ECMAScript 6 中数组的扩展](#ecmascript-6-中数组的扩展)
  - [1、扩展运算符](#1扩展运算符)
  - [2、Array.from()](#2arrayfrom)
  - [3、Array.of()](#3arrayof)
  - [4、copyWithin()](#4copywithin)
  - [5、find() 和 findIndex()](#5find-和-findindex)
  - [6、fill()](#6fill)
  - [7、entries()，keys() 和 values()](#7entrieskeys-和-values)
  - [8、includes()](#8includes)
  - [9、flat()，flatMap()](#9flatflatmap)
  - [10、数组的空位](#10数组的空位)
  - [11、Array.prototype.sort() 的排序稳定性](#11arrayprototypesort-的排序稳定性)

# ECMAScript 6 中数组的扩展

之前有记录过 ES5 中数组的常用方法([JavaScript 数组方法大全](https://blog.csdn.net/u014607184/article/details/51820564))，现在对着阮一峰老师的文档([数组的扩展](https://es6.ruanyifeng.com/#docs/array))重新记录一下 ECMAScript 6 中数组的扩展。

## 1、扩展运算符

扩展运算符（spread）是三个点（...）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。
```js

let arr1 = [1,2,3,4];
console.log(...arr1); // 1 2 3 4
let [a, ...rest] = arr1;
console.log(rest); // [2,3,4]

function push(arr, ...items){ // 剩余参数
    console.log(items); // [2,3,4]
    arr.push(...items); // 剩余参数全部展开
    console.log(arr); // [1,2,3,4]
}
push([1],2,3,4);

```

任何定义了遍历器（Iterator）接口的对象（参阅 Iterator 一章），都可以用扩展运算符转为真正的数组。

```js
let nodeList = document.querySelectorAll('div');
let array = [...nodeList];

let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]

[...'hello']
// [ "h", "e", "l", "l", "o" ]

```

## 2、Array.from()

Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）（所谓类似数组的对象，本质特征只有一点，即必须有length属性）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。

实际应用中，常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的arguments对象，以及 set, map。

```js
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).filter(p => {
  return p.textContent.length > 100;
});

// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}

Array.from('hello')
// ['h', 'e', 'l', 'l', 'o']

let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']

// arguments对象
function foo() {
  const args = [...arguments];
}

// NodeList对象
[...document.querySelectorAll('div')]

```

`Array.from() 相比于扩展运算符，可以多处理 类似数组的对象。`

## 3、Array.of()

Array.of方法用于将一组值，转换为数组。这个方法的主要目的，是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异。

```js
console.log(new Array()) // []
console.log(new Array(3)) // [, , ,]
console.log(new Array(3, 11, 8)) // [3, 11, 8]

console.log(Array.of()) // []
console.log(Array.of(3)) // [3]
console.log(Array.of(3, 11, 8)) // [3, 11, 8]
```

## 4、copyWithin()

数组实例的copyWithin()方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。

```js
Array.prototype.copyWithin(target, start = 0, end = this.length)
```
它接受三个参数。

- target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
- start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
- end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。

```js
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]

// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1)
// [4, 2, 3, 4, 5]
```

## 5、find() 和 findIndex()

数组实例的find方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。find方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。

数组实例的findIndex方法的用法与find方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1

```js
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10

[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2

// 这两个方法都可以接受第二个参数，用来绑定回调函数的this对象。

function f(v){
  return v > this.age;
}
let person = {name: 'John', age: 20};
[10, 12, 26, 15].find(f, person);    // 26

```

## 6、fill()

fill方法使用给定值，填充一个数组。常用于数组初始化，或者数组成员批量替换。

```js
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]

```

fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。

```js
['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
```

`注意，如果填充的类型为对象，那么被赋值的是同一个内存地址的对象，而不是深拷贝对象。`
```js
let arr = new Array(3).fill({name: "Mike"});
arr[0].name = "Ben";
arr
// [{name: "Ben"}, {name: "Ben"}, {name: "Ben"}]

let arr = new Array(3).fill([]);
arr[0].push(5);
arr
// [[5], [5], [5]]
```

## 7、entries()，keys() 和 values()

ES6 提供三个新的方法——entries()，keys()和values()——用于遍历数组。它们都返回一个遍历器对象（详见《Iterator》一章），可以用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。

```js
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"

```


## 8、includes()

includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。ES2016 引入了该方法。

第二个参数表示搜索的起始位置，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。

```js
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true

[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true

```
indexOf方法有两个缺点

- 一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。
- 二是，它内部使用严格相等运算符（===）进行判断，这会导致对NaN的误判。 `[NaN].indexOf(NaN) // -1`。

## 9、flat()，flatMap()

flat()用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。

```js
[1, 2, [3, 4]].flat()
// [1, 2, 3, 4]
```

flat()默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将flat()方法的参数写成一个整数，表示想要拉平的层数，默认为1。 如果不管有多少层嵌套，都要转成一维数组，可以用Infinity关键字作为参数。

```js
[1, 2, [3, [4, 5]]].flat()
// [1, 2, 3, [4, 5]]

[1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]

[1, [2, [3]]].flat(Infinity)
// [1, 2, 3]
```

flatMap()方法对原数组的每个成员执行一个函数（相当于执行Array.prototype.map()），然后对返回值组成的数组执行flat()方法。该方法返回一个新数组，不改变原数组。

```js
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]
```

## 10、数组的空位

ES5 对空位的处理，已经很不一致了，大多数情况下会忽略空位。

- forEach(), filter(), reduce(), every() 和some()都会跳过空位。
- map()会跳过空位，但会保留这个值
- join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串。

ES6 则是明确将空位转为undefined。

## 11、Array.prototype.sort() 的排序稳定性

排序稳定性（stable sorting）是排序算法的重要属性，指的是排序关键字相同的项目，排序前后的顺序不变。

常见的排序算法之中，插入排序、合并排序、冒泡排序等都是稳定的，堆排序、快速排序等是不稳定的。

早先的 ECMAScript 没有规定，Array.prototype.sort()的默认排序算法是否稳定，留给浏览器自己决定，这导致某些实现是不稳定的。ES2019 明确规定，Array.prototype.sort()的默认排序算法必须稳定。

针对早期的 sort() 的默认排序算法不稳定的情况，我们可以增加逻辑来使其排序是稳定的：当两个元素相等时，我们再比较一下其原来的索引即可。