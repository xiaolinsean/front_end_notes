## 1、原型和原型链

    - 每一个构造函数都拥有一个 prototype 属性，这个属性指向一个对象，也就是原型对象
    - 原型对象默认拥有一个 constructor 属性，指向指向它的那个构造函数
    - 每个对象都拥有一个隐藏的属性 __proto__，指向它的原型对象

    - 实例可以共享原型上面的属性和方法
    - 实例自身的属性会屏蔽原型上面的同名属性，实例上面没有的属性会去原型上面找

    JavaScript 中所有的对象都是由它的原型对象继承而来。而原型对象自身也是一个对象，它也有自己的原型对象，这样层层上溯，就形成了一个类似链表的结构，这就是原型链。

    instanceof：用来判断两个对象是否属于实例关系

    hasOwnProperty：可以确定访问的属性是来自于实例还是原型对象

    当原型上面的属性是一个引用类型的值时，我们通过其中某一个实例对原型属性的更改，结果会反映在所有实例上面，这也是原型 共享 属性造成的最大问题

参考：[https://zhuanlan.zhihu.com/p/62903507](https://zhuanlan.zhihu.com/p/62903507)

## 2、js 的继承方式

    （1）组合继承

        子类的构造函数中调用 Parent.call(this，value) 继承父类的属性，然后改变子类的原型 Child.prototype = new Parent();

        优点，构造函数可以传参，不会与父类共享引用变量，且可以复用父类的函数；缺点：在继承父类的函数时，调用了父类的构造函数，继承了一些不必要的属性；


    （2）寄生组合继承

        Object.create(prototype, descriptors) ：创建一个具有指定原型且可选择性地包含指定属性的对象。这个方法接收两个参数：一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象。

    （3）class 继承

        ES6 中新增了 class ， 可以通过 extends 关键字继承。在子类的 constructor 中调用 super(); class 其实是语法糖，其本质还是函数：

        class A {}
        A instanceof Function // true


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


## 5、js 中的闭包

    简单的理解：闭包就是能够读取其他函数内部变量的函数，由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"，闭包就是将函数内部和函数外部连接起来的一座桥梁。

    比如：函数 A 中定义了函数 B，函数 B 中使用到了函数 A 中的变量，那么函数 B 就是闭包。

    闭包的会产生的问题：多个子函数作用域链中指向的父级作用域中的变量是共享的，因此，如果该共享变量被修改了，那么所有的子函数都会被影响。

    // 实现第一秒打印0，第二秒打印1，第三秒打印2
    for(var i = 0; i < 3; i++){
        setTimeout(function timer(){
            console.log(i)
        }, i * 1000)
    } //  打印都是 3，因为 setTimeout 是异步操作，执行到 setTimeout时，for 循环已经执行完了，i 的值为 3，而且根据上面所说的，i 的值被各个 timer 共享


    // 使用自执行函数，将 i 传入到函数内部，这样 i 的值就固定在参数 j 上，下次访问闭包 timer 函数时，使用的外部变量 j。
    for(var i = 0; i < 3; i++){
        (function(j){
            setTimeout(function timer(){
            console.log(j)
        }, j * 1000)
        })(i)
    }
    

    // 使用 setTimeout 的第三个参数，第三个参数会被当做 timer 的入参。
    for(var i = 0; i < 3; i++){
            setTimeout(function timer(j){
            console.log(j)
        }, i* 1000, i)
    }
    

    // 使用 let， 引入块级作用域   
    for(let i = 0; i < 3; i++){
        setTimeout(function timer() {
            console.log(i)
        }, i * 1000)
    }
    


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

## 13、说下 offsetWith 和 clientWidth、offsetHeight 和 clientHeight 的区别，说说 offsetTop，offsetLeft，scrollWidth、scrollHeight 属性都是干啥的 

    clientHeight和offsetHeight属性和元素的滚动、位置没有关系它代表元素的高度，其中：
    clientHeight：包括padding但不包括border、水平滚动条、margin的元素的高度。对于inline的元素这个属性一直是0，单位px，只读元素。
    offsetHeight：包括padding、border、水平滚动条，但不包括margin的元素的高度。对于inline的元素这个属性一直是0，单位px，只读元素。

    scrollHeight: 因为子元素比父元素高，父元素不想被子元素撑的一样高就显示出了滚动条，在滚动的过程中本元素有部分被隐藏了，scrollHeight代表包括当前不可见部分的元素的高度。而可见部分的高度其实就是clientHeight，也就是scrollHeight>=clientHeight恒成立。在有滚动条时讨论scrollHeight才有意义，在没有滚动条时scrollHeight==clientHeight恒成立。单位px，只读元素。
    scrollTop: 代表在有滚动条时，滚动条向下滚动的距离也就是元素顶部被遮住部分的高度。在没有滚动条时scrollTop==0恒成立。单位px，可读可设置。

    offsetTop: 当前元素顶部距离最近父元素顶部的距离,和有没有滚动条没有关系。单位px，只读元素。

参考：[搞清clientHeight、offsetHeight、scrollHeight、offsetTop、scrollTop](https://blog.csdn.net/qq_35430000/article/details/80277587)

## 14、cookie 和session 的区别 

    1、cookie数据存放在客户的浏览器上，session数据放在服务器上。
    2、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗
        考虑到安全应当使用session。
    3、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
        考虑到减轻服务器性能方面，应当使用COOKIE。
    4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
    5、所以个人建议：
        将登陆信息等重要信息存放为SESSION
        其他信息如果需要保留，可以放在COOKIE中


## 15、javaScript中的作用域和作用域链

    作用域就是在运行时代码中不同部分中函数和变量的可访问性。

    （1）全局作用域：变量可以随处访问
    （2）函数作用域：变量可以在定义它们的函数的边界内访问
    （3）块级作用域：变量可以在定义它们的块中访问，块由 { 和 } 分隔 （ES6新增）

    
    作用域链：当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。

参考：[https://zhuanlan.zhihu.com/p/69910449](https://zhuanlan.zhihu.com/p/69910449)

## 16、变量声明提升和函数声明提升

    var getName = function(){
        console.log(2);
    }
    function getName (){
        console.log(1);
    }
    getName();

    等效于：
    function getName(){    //函数声明提升到顶部
        console.log(1);
    }
    var getName;    //变量声明提升
    getName = function(){    //变量赋值依然保留在原来的位置
        console.log(2);
    }
    getName();    // 最终输出：2

    在非严格模式下：所有末声明直接赋值的变量将自动声明为拥有全局作用域的变量（严格模式下会报错）

参考：[https://blog.csdn.net/qq673318522/article/details/50810650](https://blog.csdn.net/qq673318522/article/details/50810650)


## 17、AMD，CMD，CommonJs，ES6 Module 的区别？

    （1） CommonJS模块是对象，是运行时加载，运行时才把模块挂载在exports之上（加载整个模块的所有），加载模块其实就是查找对象属性。

    （2） ES Module不是对象，是使用export显示指定输出，再通过import输入。此法为编译时加载，编译时遇到import就会生成一个只读引用。等到运行时就会根据此引用去取加载模块的取值。所以不会加载模块所有方法，仅取所需。

    （3） AMD/RequireJS： 依赖前置的， 即不管你用没用到， 只要你设置了依赖， 就会去加载。不是按需加载的。

    （4）CMD/sea.js: 就近加载， 按需加载的, 预先下载，延迟执行

    AMD/CMD是CommonJS在浏览器端的解决方案。CommonJS是同步加载（代码在本地，加载时间基本等于硬盘读取时间）。AMD/CMD是异步加载（浏览器必须这么干，代码在服务端）


参考：[https://www.jianshu.com/p/da2ac9ad2960](https://www.jianshu.com/p/da2ac9ad2960)、[https://www.jianshu.com/p/906aa802bf98](https://www.jianshu.com/p/906aa802bf98)

## 18、JavaScript中的Event Loop（事件循环）机制？

    同步任务（synchronous）: 在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；

    异步任务（asynchronous）: 不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

    运行机制如下：

    （1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

    （2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

    （3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

    （4）主线程不断重复上面的第三步。其中需要注意的是，异步任务之间并不相同，因此他们的执行优先级也有区别。不同的异步任务被分为两类：微任务（micro task）和宏任务（macro task）。
    以下事件属于宏任务：
        setInterval()
        setTimeout()

    以下事件属于微任务
        new Promise()
        new MutaionObserver()

    在一个事件循环中，异步事件返回结果后会被放到一个任务队列中。然而，根据这个异步事件的类型，这个事件实际上会被对应的宏任务队列或者微任务队列中去。并且在当前执行栈为空的时候，主线程会 查看微任务队列是否有事件存在。如果不存在，那么再去宏任务队列中取出一个事件并把对应的回到加入当前执行栈；如果存在，则会依次执行队列中事件对应的回调，直到微任务队列为空，然后去宏任务队列中取出最前面的一个事件，把对应的回调加入当前执行栈...如此反复，进入循环。

    console.log('script start');

    setTimeout(function() {
    console.log('setTimeout');
    }, 0);

    new Promise((resolve) => {
        console.log('Promise')
        resolve()
    }).then(function() {
    console.log('promise1');
    }).then(function() {
    console.log('promise2');
    });

    console.log('script end');

    // script start => Promise => script end => promise1 => promise2 => setTimeout

## 19、caller与callee的作用以及用法

    （1）caller是javascript函数类型的一个属性，它引用调用当前函数的函数；如果函数在全局作用域内被调用时，那么caller将返回null。

    （2）callee 是 arguments 对象的一个属性。它可以用于引用该函数的函数体内当前正在执行的函数。这在函数的名称是未知时很有用，例如在没有名称的函数表达式 (也称为“匿名函数”)内。不过在严格模式下，已经不建议使用了。

## 20、javascript 中的 垃圾回收 机制？

    内存的生命周期：

    （1).内存分配：当我们申请变量，函数，对象的时候，系统会自动为他们分配内存

    (2).内存使用：即读写内存，也就是使用变量、函数

    (3).内存回收：使用完毕，有垃圾回机制回收不再使用的内存

    垃圾回收两大机制：

    (1) 标记清除

    这是javascript中最常用的垃圾回收方式。当变量进入执行环境是，就标记这个变量为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到他们。当变量离开环境时，则将其标记为“离开环境”。
    垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记。然后，它会去掉环境中的变量以及被环境中的变量引用的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后。垃圾收集器完成内存清除工作，销毁那些带标记的值，并回收他们所占用的内存空间。

    (2) 引用计数

    另一种不太常见的垃圾回收策略是引用计数。引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型赋值给该变量时，则这个值的引用次数就是1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数就减1。当这个引用次数变成0时，则说明没有办法再访问这个值了，因而就可以将其所占的内存空间给收回来。这样，垃圾收集器下次再运行时，它就会释放那些引用次数为0的值所占的内存。

## 21、JavaScript 中哪些情况会出现 内存泄漏，应该如何避免？

    （1） 全局变量泛滥，无意设置的全局的变量（比如：没有通过var 定义而导致的变量提升）；显示定义的全局变量，用于临时存储和处理大量数据的全局变量，使用后需要对其清除。

    （2）被遗忘的定时器：及时清除不需要的的定时器，已确保定时器内的引用会被释放；

    （3）DOM引用：DOM节点移除后，对DOM节点的引用以及上面添加的时间要及时清除

    （4）闭包：避免闭包中数据的乱引用

    通过 chrome devtools 可以中的timeline 和 js profiler 可以观察到内存泄露的情况

## 22、js 中如何自定义事件？

    1、创建事件：

    let myEvent = new Event(typeArg, eventInit);
    let myEvent = new CustomEvent(typeArg, eventInit);

    typeArg： DOMString 类型，表示创建事件的名称；
    eventInit：{
                    detail // 表示该事件中需要被传递的数据，在 EventListener 获取。  CustomEvent 特有
                    bubbles // 表示该事件是否冒泡。
                    cancelable // 表示该事件能否被取消。
                    composed //指示事件是否会在影子DOM根节点之外触发侦听器。  Event 特有
                }
    2、事件监听
        
        通过 addEventListener 和 removeEventListener 来添加和移除事件
    
    3、事件触发

        使用 dispatchEvent 去触发（IE8低版本兼容，使用fireEvent）


## 23、js 中什么是深拷贝和浅拷贝，怎么对数组或者对象进行深拷贝？

    基本数据类型：值直接存储在栈内存中，
    复杂数据类型：也叫引用数据类型，其真正的值是存储在堆内存中，栈内存是储存对应的变量名和指向该对象在堆内存中的地址。

    浅拷贝：直接将一个复杂数据类型的变量赋值给另一个变量，另一个变量复制的只是原来变量的地址，他们指向的是同一个堆内存的内容，这种只复制地址的情况就是浅拷贝。

    深拷贝：不复制原来变量的地址，而是在堆内存中另外开辟一块新的地址，将原变量的的各个属性以及属性值逐个复制出去，复制后的变量与原来变量是独立的。

    深拷贝的方法：

    （1）常用的Object.assign()，其实只能对第一层进行深拷贝，不是真实意义上的深拷贝

    （2）JSON.parse() 和 JSON.stringify()
        该方法有以下几个问题：（1）会忽略 `undefined`（2）会忽略 `symbol`（3）不能序列化函数（4）不能解决循环引用的对象（5）不能正确处理`new Date()`（6）不能处理正则
    
    （3）第三库 `jQuery.extend(true, {}, originalObject)` 和 `lodash.cloneDeep() 实现真正意义上的深拷贝

    （4）手写深拷贝函数

        function deepClone(target) {
            if (typeof target === 'object') {
                let cloneTarget = Array.isArray(target) ? [] : {};
                for (const key in target) {
                    cloneTarget[key] = clone(target[key]);
                }
                return cloneTarget;
            } else {
                return target;
            }
        };

更多数据类型的考虑和性能优化可参考:[https://juejin.im/post/6844903929705136141](https://juejin.im/post/6844903929705136141)

## 24、setTimeout 和 requestAnimationFrame // TODO

    



## 25、js 中的执行上下文 // TODO

    js 执行时会有三种情况的执行上下文

    （1）全局执行上下文
    （2）函数执行上下文
    （3）eval() 执行上下文

    每个执行上下文都有三个重要的属性：

    （a）变量对象（OV）
    （b）作用域链
    （c）this

## 26、谈谈对函数柯里化的理解

> 柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术,其基本思想是：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
> 
> ```js
> function curry(fn, args) {
>     var length = fn.length;
>     args = args || [];
> 
>     return function() {
> 
>         var _args = args.slice(0),
> 
>             arg, i;
> 
>         for (i = 0; i < arguments.length; i++) {
> 
>             arg = arguments[i];
> 
>             _args.push(arg);
> 
>         }
>         if (_args.length < length) {
>             return curry.call(this, fn, _args);
>         }
>         else {
>             return fn.apply(this, _args);
>         }
>     }
> }
> 
> 
> var fn = curry(function(a, b, c) {
>     console.log([a, b, c]);
> });
> 
> fn("a", "b", "c",'d') // ["a", "b", "c"]
> fn("a", "b")("c") // ["a", "b", "c"]
> fn("a")("b")("c") // ["a", "b", "c"]
> fn("a")("b", "c") // ["a", "b", "c"]
> ```
> 
> 主要为以下常见的三个用途：延迟计算、参数复用、动态生成函数；
> 
> 
> 无限累加：
> ```js
> function myAdd(){
>     var _args = [...arguments];
>     console.log(arguments);
>     let sum = 0;
>     return function add(){
>         if(!arguments.length){
>             for(let i =0; i < _args.length; i++){
>                 sum = sum + _args[i];
>             }
>             return sum;
>         }else{
>             Array.prototype.push.apply(_args, arguments);
>             return arguments.callee;
>         }
>     }
> }
> 
> console.log(myAdd(1,2,3)(2)());
> ```
> 
> 
> 参考：[JavaScript专题之函数柯里化](https://github.com/mqyqingfeng/Blog/issues/42)、[JavaScript函数柯里化](https://juejin.cn/entry/6844903512942313479)、[javascript 函数 add(1)(2)(3)(4)实现无限极累加](https://www.cnblogs.com/oxspirt/p/5436629.html)
> 

## 组合函数、高阶函数 
[https://segmentfault.com/a/1190000023616150](https://segmentfault.com/a/1190000023616150)