[阮一峰 ES6 入门教程](https://es6.ruanyifeng.com/)

## `1、JavaScript、ES5、ES6和ECMAScript 2015有什么区别?`

- ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现；

- ES5： 我们常说的ES5泛指上一代 ECMAScript 语言标准；

- ES6: ES6是ECMA的为JavaScript制定的第6个版本的标准,2015年6月份发布的ES6的第一个版本,后面每年还有第二、第三。。。版本，所以 ES6 泛指下一代标准；

- ECMAScript 2015： 是指 2015 发布的 ECMAScript 标准，也就是 ES6 的第一个版本

--------------------------------------------------------------------------------------------------------------

## `2、Symbol 数据类型`

ES6 引入了第七种原始数据类型Symbol，表示独一无二的值，前六种分别为: undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

Symbol 值通过Symbol函数生成，生成后，即为独一无二的值。

```js
let s1 = Symbol();
let s2 = Symbol();

console.log(s1 == s2); // false
console.log(typeof s1);  // symbol
console.log(Object.prototype.toString.call(s1)); // [object Symbol]
console.log(s1 instanceof Symbol);  // false

```

Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。ES2019 提供了一个实例属性description，直接返回 Symbol 的描述。 Symbol 可以`显示`的转换成 string 和 bool 类型。

```js
let s1 = Symbol("s1");
let s2 = Symbol("s1");

console.log(s1); // Symbol(s1)
console.log(s1.toString()); // 'Symbol(s1)'
console.log(s1.description);  // s1
console.log(s1 == s2); // false

```

Symbol 的唯一性 可以用于对象的属性名，就能保证不会出现同名的属性，能防止某一个键被不小心改写或覆盖。可以通过以下方法设置对应的属性值，但是不能用点运算符。

```js
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果 
console.log(a[mySymbol]) // hello
console.log(a.mySymbol) // undefined
```
需要注意的是：Symbol 作为属性名，遍历对象的时候，该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。
但是，它也不是私有属性，有一个Object.getOwnPropertySymbols()方法，可以获取指定对象的所有 Symbol 属性名，该方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值，这个特性可以用作私有属性设置。
另一个新的 API，`Reflect.ownKeys()` 方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

有时，我们希望重新使用同一个 Symbol 值，就需要用到 `Symbol.for()` 了，它接受一个字符串作为参数，然后全局搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。这里需要注意的是： `Symbol.for()` 登记是在全局环境下的，忽视具体申明的作用域。

```js
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true
Symbol.keyFor(s1) //  foo

```

--------------------------------------------------------------------------------------------------------------


## `3、Set 和 WeakSet 数据结构`

Set 是 ES6 提供了新的数据结构。它类似于数组，但是成员的值都是唯一的，没有重复的值。set 判断值是否相同使用的算法叫做“Same-value-zero equality”，它类似于精确相等运算符（===），主要的区别是向 Set 加入值时认为NaN等于自身，而精确相等运算符认为NaN不等于自身。 

```js
const s = new Set([1,2,3,4,5,4,3,2]);

console.log(typeof s); // object
console.log(Object.prototype.toString.call(s)); // [object Set]
console.log(s instanceof Set);  // true
console.log(s); // Set(5) {1, 2, 3, 4, 5}
console.log([...s]); // [1, 2, 3, 4, 5]   数组去重

const s1 = new Set("abcdcsb");
console.log(s1); // Set(5) {"a", "b", "c", "d", "s"}
console.log([...s1].join('')); // 'abcds'  字符串去重

```

Set 实例的属性和方法

```js
const s = new Set([1,2,3,3,2,'2','name',undefined]);

console.log(s); // Set(6) {1, 2, 3, "2", "name", undefined}
console.log(s.size); // 6
console.log(s.has('name'));  // true
s.add(null); 
console.log(s.has(null)); // true
s.delete(null);
console.log(s.has(null)); // false
s.clear();
// clear方法清除所有成员，没有返回值。
```

遍历操作

```js
let set = new Set(['red', 'green', 'blue']);

console.log(set.keys()); // SetIterator {"red", "green", "blue"}
console.log(set.values()); // SetIterator {"red", "green", "blue"}
console.log(set.entries()); // SetIterator {"red" => "red", "green" => "green", "blue" => "blue"}

for(let item of set.keys()){
    console.log(item);
}
// red
// green
// blue

// Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的values方法
for(let item of set){
    console.log(item);
}
// red
// green
// blue

for(let item of set.entries()){
    console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]

set.forEach((value, key) => console.log(key + ' : ' + value))
// red : red
// green : green
// blue : blue

```

实现并集（Union）、交集（Intersect）和差集（Difference）

```js
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// （a 相对于 b 的）差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}

```

`WeakSet` 结构与 `Set` 类似，也是不重复的值的集合。但是，它与 `Set` 有两个区别。

- 首先，`WeakSet` 的成员只能是对象，而不能是其他类型的值。

- 其次，`WeakSet` 中的对象都是弱引用，即垃圾回收机制不考虑 `WeakSet` 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 `WeakSet` 之中。

--------------------------------------------------------------------------------------------------------------


## `4、Map 和 WeakMap 数据结构`

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是只能用字符串当作键。 ES6 提供了 `Map` 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

```js
const m = new Map([{'a':'aa'},['b','bb']]);

console.log(typeof m); // map
console.log(Object.prototype.toString.call(m)); // [object Map]
console.log(m instanceof Map);  // true
console.log(m) // Map(2) {undefined => undefined, "b" => "bb"}

```
Map构造函数接受数组作为参数，不仅仅是数组，任何具有 Iterator 接口、且每个成员都是一个`双元素的数组`的数据结构（详见《Iterator》一章）都可以当作Map构造函数的参数。对同一个键多次赋值（对同一个对象的引用），后面的值将覆盖前面的值。

Map 常用增删改查 操作

```js

const m = new Map([['a','aa'],['b','bb']]);

console.log(m.set(undefined, 'undefined')); // Map(3) {"a" => "aa", "b" => "bb", undefined => "undefined"}

m.set('c', 'cc').set('d','dd'); // .set()返回整个 Map 结构，所以可以链式调用
console.log(m.size); // 5
console.log(m.get('c')); // cc
console.log(m.has(undefined)); // true
console.log(m.delete(undefined)); // true 删除成功返回 true , 否则返回 false

m.clear(); // 清除所有成员，不返回值

```

遍历

```js
const m = new Map([['a','aa'],['b','bb'],['c','cc']]);

console.log(m.keys()); // MapIterator {"a", "b", "c"}
console.log(m.values()); // MapIterator {"aa", "bb", "cc"}
console.log(m.entries()); // MapIterator {"a" => "aa", "b" => "bb", "c" => "cc"}

// for ... of 遍历 或者转变成数组 类似：[...m.keys()]

m.forEach((value, key, map) => {
  console.log("Key: %s, Value: %s", key, value);
});
// Key: a, Value: aa
// Key: b, Value: bb
// Key: c, Value: cc

```

`WeakMap` 结构与 `Map` 结构类似，也是用于生成键值对的集合。`WeakMap` 与 `Map` 的区别有两点: 

- 首先，`WeakMap` 只接受对象作为键名（null除外），不接受其他类型的值作为键名。

- 其次，`WeakMap` 的键名所指向的对象，不计入垃圾回收机制。

--------------------------------------------------------------------------------------------------------------

## `5、for...in 和 for...of 的区别`

```js
let arr = ['a','b','c'];

let obj = {a:'aa',b:'bb','c':'cc'};

for(let i in arr){
    console.log(i);
}
// 0
// 1
// 2
// d

for(let i of arr){
    console.log(i);
}
// a
// b
// c

for(let i in obj){
    console.log(i);
}
// a
// b
// c

for(let i of obj){
    console.log(i);
}
// Uncaught TypeError: obj is not iterable

```
- `for...in` 主要用于遍历对象，得到对应的键名 key, 由于数组也是一种特殊的对象，其 key 为数组索引，所以遍历数组时，返回索引；

- `for...of` 是 ES6 中新增的用于遍历具备 `Iterator` 接口的数据结构，原生具备`Iterator` 接口的数据结构包括：Array、 Map、 Set、 String、 函数的 arguments 对象、 NodeList 对象；
  `for...of` 遍历返回对应的键值 value，因为上述的 `obj` 不具备 `Iterator` 接口，所以会报错。另外需要注意的是，`for...of` 遍历数组时，只返回具有数字索引的属性, `for...in` 会全部遍历出来。

--------------------------------------------------------------------------------------------------------------

## `6、`