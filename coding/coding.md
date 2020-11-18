## 1、手写 bind // TODO



---

## 2、手写 apply // TODO



---

## 3、手写 call // TODO



---

## 4、手写 promise // TODO



---

## `5、买卖股票的最佳时机`
`题干：`
```md 

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。


示例 1:
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。

示例 2:
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

```

`思路：`
```md
动态规划： 前i天的最大收益 = max{前i-1天的最大收益，第i天的价格-前i-1天中的最小价格}
```
`代码实现：`

``` js
var maxProfit = function(prices) {
    let max = 0, minprice = prices[0]
    for(let i = 1; i < prices.length; i++) {
        minprice = Math.min(prices[i], minprice)
        max = Math.max(max, prices[i] - minprice)
    }
    return max
};
```
---

## `6、斐波那契数列`
`题干`
```md
写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

示例 1：
输入：n = 2
输出：1

示例 2：
输入：n = 5
输出：5

```
`思路`
```md
常规思路就是递归，但是计算量太大，实际F(N)，只跟F(N - 1) 和 F(N - 2)，我们只需要逐步记录前面两个数据即可；
```

`代码实现`
```js
var fib = function(n) {
    if(n==0) return 0;
    if(n==1) return 1;
    let fib1 = 0, fib2=1, res = 1;

    for(let i = 2; i <= n; i++){
        res = (fib1 + fib2);
        if(res > 1e9 + 7){
            res = res - (1e9 + 7);
        }
        fib1 = fib2;
        fib2 = res;
    }

    return res
    
};
```
---

## `7、判断一个数字是否是回文`
`题干`
```md
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

示例 1:
输入: 121
输出: true

示例 2:
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

```
`思路`
```md
- 数字 =》 字符串 =》 数组 =》 反转 =》 字符串 对比

- 数字 =》 字符串 =》 数组 收尾追个对比，对比至数组中间索引即可
```

`代码实现`
```js
// 第一种思路
var isPalindrome = function(x) {
    return x.toString() == x.toString().split('').reverse().join('');
};

// 第二种思路
var isPalindrome = function(x) {
    var arr = x.toString().revert();
    let len = arr.length;
    let halfLen = Math.ceil(len/2);
    for(let i =0; i < halfLen; i ++){
        if(i < (len/2 + 1))
        if(arr[i] != arr[len-i-1]) return false;
    }
    return true;
};
```
---