<!-- 本文主要记录数组常用的一些操作场景 -->

## `1、数组降维`

- 二维降一维：

    ```js
    // contat + ...
    const oldArr = [1, 2, [3, 4]];

    const newArr = [].concat(...oldArr);
    ```

    ```js
    // reduce 
    const newArr = oldArr.reduce((prev, curr) => (prev.concat(curr)), []);
    ```

    ```js
    // flat 
    const newArr = oldArr.flat();
    ```

- 多维降一维

    ```js
    // 可自定义递归深度
    const eachFlat = (arr = [], depth = 1) => {
        const result = []; // 缓存递归结果
        // 开始递归
        (function flat(arr, depth) {
            // forEach 会自动去除数组空位
            arr.forEach((item) => {
                // 控制递归深度
                if (Array.isArray(item) && depth > 0) {
                    // 递归数组
                    flat(item, depth - 1)
                } else {
                    // 缓存元素
                    result.push(item)
                }
            })
        })(arr, depth)
        // 返回递归结果
        return result;
    }
    ```

    ```js
    // 递归版本
    function flatten(array) {
        var flattend = [];
        (function flat(array) {
            array.forEach(function(el) {
            if (Array.isArray(el)) flat(el);
            else flattend.push(el);
            });
        })(array);
        return flattend;
    }
    ```

    ```js
    // reduce
    const flatten2 = (arr) => arr.reduce((prev, curr, index, list) => {
        if (Array.isArray(curr)) {
            return prev.concat(flatten2(curr));
        }
        return prev.concat(curr);
    }, []);
    ```

    ```js
    // flat
    let oldArr = [1,[2, [3],[4, 5, 6],[7, 8, 9],10,11,],12,13,14,[15, 16, 17],];
    
    oldArr.flat(Infinity);
    ```

------------------------------------------------------------------------------------------------------

参考：[Array.prototype.flat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)

## `2、数组去重`

```js
// 使用 set
function unique(arr = []) {
    return [...new Set(arr)];
}

// 使用 map
function unique2(arr = []) {
    const tmp = new Map();
    return arr.filter(item => {
        return !tmp.has(item) && tmp.set(item, 1);
    })
}


// 使用 includes
function unique3(arr = []) {
    const newArray = [];
    arr.forEach(item => {
        if (!newArray.includes(item)) {
            newArray.push(item);
        }
    });
    return newArray;
}

// 使用 indexOf
function unique4(arr = []) {
    const newArray = [];
    arr.forEach(item => {
        if (newArray.indexOf(item) === -1) {
            newArray.push(item);
        }
    });
    return newArray;
}

let arrTest = [0,"1",2,4,5,3,2,3,2,1,6,7,false];

console.log(unique(arrTest));
console.log(unique2(arrTest));
console.log(unique3(arrTest));
console.log(unique4(arrTest));
// [0, "1", 2, 4, 5, 3, 1, 6, 7, false]
```

参考：[解锁多种JavaScript数组去重姿势](https://juejin.cn/post/6844903608467587085)