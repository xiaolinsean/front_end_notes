## 1、原型链 // TODO


## 2、js 的继承方式 // TODO


## 3、js 数据类型及类型检测 

    基本数据类型六种：number、string、undefined、null、boolean，Symbol；
    复杂数据类型（引用数据类型）：object，（Arry(数组)、Function(函数)、Date等都归为object）；

    类型检测：
    （1）typeof=>string：无法判断具体的引用类型数据，如数组
        console.log(typeof "");   //string
        console.log(typeof 1);  //number
        console.log(typeof true);  //boolean
        console.log(typeof null);  //object
        console.log(typeof undefined);  //undefined
        console.log(typeof []);   //object
        console.log(typeof function(){});   //function
        console.log(typeof {});   //object
    （2）instanceof=>boolean ： 能够判断具体的引用类型、但不能用于判断基本数据类型
        console.log("1" instanceof String);   //flase
        console.log(new String("1") instanceof String);  //true
        console.log(1 instanceof Number);   //false
        console.log(new Number(1) instanceof Number); //true
        console.log(true instanceof Boolean);   //false
        console.log(new Boolean(true) instanceof Boolean); //true
        console.log([] instanceof Array);  //true
        console.log(function(){} instanceof Function);  //true
        console.log({} instanceof Object);  //true
    （3）Object.prototype.toString.call()：最准确最常用的方式
        console.log(Object.prototype.toString.call("aaa"));    //[object String]
        console.log(Object.prototype.toString.call(1));        //[object Number]
        console.log(Object.prototype.toString.call(true));       //[object Boolean]
        console.log(Object.prototype.toString.call(null));       //[object Null]
        console.log(Object.prototype.toString.call(undefined));   //[object Undefined]
        console.log(Object.prototype.toString.call([]));         //[object Array]
        console.log(Object.prototype.toString.call(function() {}));    //[object Function]
        console.log(Object.prototype.toString.call({}));     //[object Object]
参考：[https://www.cnblogs.com/yuanzhiguo/p/7811990.html](https://www.cnblogs.com/yuanzhiguo/p/7811990.html)

## 4、js 小数计算精度问题及解决方法

    64 位双精度浮点型来表示
    
    精度问题:
    (1) 浮点数精度问题，比如 0.1 + 0.2 !== 0.3 
    (2) 大数精度问题，比如 9999 9999 9999 9999 == 1000 0000 0000 0000 1
    (3) toFixed 四舍五入结果不准确，比如 1.335.toFixed(2) == 1.33

    解决方法：
    (1) 考虑到每次浮点数运算的偏差非常小，可以对结果进行指定精度的四舍五入
    (2) 将浮点数转为整数运算，再对结果做除法。注意不要超过最大最小值界限
    (3) 把浮点数转化为字符串，模拟实际运算的过程。一些流行的库，比如 bignumber.js，decimal.js，以及big.js等

参考：[https://www.runoob.com/w3cnote/js-precision-problem-and-solution.html](https://www.runoob.com/w3cnote/js-precision-problem-and-solution.html)


## 5、js 中的闭包  // TODO

## 6、介绍防抖和节流原理、区别以及应用

    函数防抖(debounce)：在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。常用于输入框输入内容后自动搜索联想、resize和scroll过程

    函数节流(throttle)：规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。

参考：[https://juejin.im/post/6844903669389885453](https://juejin.im/post/6844903669389885453)


## 7、this 指向问题

    （1） 全局 this 是 window
    （2） 函数 this 是调用者
    （3） 构造函数的 this 是 new 之后的新对象
    （4） call ，apply ，bind 的 this 是第一个参数
    （5） 事件绑定中的this：
        行内绑定（HTML事件）：this指向全局window
        动态绑定（及其他情况）：this指向当前节点本身
    （6）箭头函数：箭头函数的this是在定义函数时绑定的，不是在执行过程中绑定的
    （7）定时器setInterval和setTimeout: this指向全局window

参考：[https://www.jianshu.com/p/d647aa6d1ae6](https://www.jianshu.com/p/d647aa6d1ae6)
参考：[https://zhuanlan.zhihu.com/p/42145138](https://zhuanlan.zhihu.com/p/42145138)

## 8、如何改变 this 指向

    （1）可以使用局部变量来代替this指针，即常用的 var that = this;
    （2）call 第一个参数为this指向的对象，后面的为函数调用的入参
    （3）apply 第一个参数为this指向的对象，第二个参数是一个数组，为函数调用的入参
    （4）bind 第一个参数为this指向的对象，后面的为函数调用的入参；
    需要注意的是：call()和apply()是执行函数的方式，直接调用就会执行，而bind()是返回一个新的函数，必须调用才会执行，即bind(this,arg1,arg2)()

    手动实现bind
    Function.prototype.customBind = function (ctx, ...args) {
        return (...args2) => this.apply(ctx, args.concat(args2)); // 注意bind函数支持函数柯里化，
    };

## 9、js 中的事件机制

    事件冒泡：具体节点到document，
    事件捕获：document到具体节点，
    “DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。

    （1）HTML事件处理程序 <input type="button" value="Click Me" οnclick="alert(this.value)" /> 
    （2）DOM0级事件处理程序 ： 通过js将一个函数赋值给元素的事件处理程序属性
    （3）DOM2级事件处理程序：addEventListener() 和 removeEventListener()：接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值（true:捕获；false：冒泡）。
    （4）IE事件处理程序：attachEvent() 和 detachEvent()。这两个方法接受相同的两个参数：事件处理程序名称与事件处理程序函数，只支持冒泡。

    阻止事件冒泡：
        IE：window.event.cancelBubble = true;
        DOM标准：event.stopPropagation();
    阻止事件默认行为：
        IE：window.event.returnValue = false;
        DOM标准：event.preventDefault();
参考：[https://blog.csdn.net/sinat_34032652/article/details/79850674](https://blog.csdn.net/sinat_34032652/article/details/79850674)


## 10、js 中的事件委托

    事件委托主要是利用事件的冒泡机制，会把一个或者一组元素的事件委托到它的父层或者更外层元素上，真正绑定事件的是外层元素，当事件冒泡到需要绑定的元素上时，会通过事件冒泡机制从而触发它的外层元素的绑定事件上，然后在外层元素上去执行函数。

    e.target:指向触发事件监听的对象
    e.currentTarget 指向添加监听事件的对象
    当处于事件的目标阶段时，e.currenttarget和e.target是相等，所以事件委托用到的是target属性

参考：[https://www.cnblogs.com/liugang-vip/p/5616484.html](https://www.cnblogs.com/liugang-vip/p/5616484.html)

## 11、有哪些跨域的方法 // TODO



## 12、谈谈对 Function 中的 arguments的理解

    arguments 是一个类数组对象，此对象包含传递给函数的每个参数，第一个参数在索引0处。因此可以用 arguments[0] 来获取第一个参数,它类似于 Array，但除了length属性和索引元素之外没有任何Array属性。

    我们可以将 arguments 转换成真正的数组：
    var args = Array.prototype.slice.call(arguments);
    var args = [].slice.call(arguments);
    // ES2015
    const args = Array.from(arguments);
    const args = [...arguments];

    形参：函数定义时，设置的参数，Function.length返回的是形参的个数
    实参：函数调用时实际传递的参数，arguments.length返回是实参的个数

    arguments.callee引用函数自身

## 13、说下 offsetWith 和 clientWidth、offsetHeight 和 clientHeight 的区别，说说 offsetTop，offsetLeft，scrollWidth、scrollHeight 属性都是干啥的 // TODO





## 手动实现一个函数柯里化 // TODO