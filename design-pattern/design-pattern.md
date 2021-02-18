## `1、单例模式（Singleton Pattern）`

保证一个类仅有一个实例，并提供一个访问它的全局访问点。实现的方法为先判断实例存在与否，如果存在则直接返回，如果不存在就创建了再返回，这就确保了一个类只有一个实例对象。

`class静态方法实现：`
```js
class Singleton {

  constructor(name) {
    this.name = name
    this.instance = null
  }

  getName() {
    alert(this.name)
  }

  static getInstance(name) {
    if (!this.instance) {
      this.instance = new Singleton(name)
    }
    return this.instance
  }
}

const instanceA = Singleton.getInstance('seven1')
const instanceB = Singleton.getInstance('seven2')

console.log(instanceA, instanceB)

```

`闭包实现：`
```js
class Singleton {
    constructor(name) {
        this.name = name;
        this.getName();
    }
    getName() {
         return this.name;
    }
}
// 代理实现单例模式
var ProxyMode = (function() {
    var instance = null;
    return function(name) {
        if(!instance) {
            instance = new Singleton(name);
        }
        return instance;
    }
})();
// 测试单体模式的实例
var a = new ProxyMode("aaa");
var b = new ProxyMode("bbb");
// 因为单体模式是只实例化一次，所以下面的实例是相等的
console.log(a === b);    //true

```

`使用场景：`例如弹框遮罩层、以及 Vuex 和 redux 中的 store

----------------------------------------------------------------

## `2、观察者模式 (Observer Pattern)`

观察者模式也叫订阅/发布模式(也有说订阅/发布模式多了中间者Broker，其本质思想是一样的)，它的作用是当一个对象的状态发生变化时，能够自动通知其他关联对象，自动刷新对象状态，或者说执行对应对象的方法。
这种设计模式可以大大降低程序模块之间的耦合度，便于更加灵活的扩展和维护。

观察者模式包含两种角色：

- 观察者（订阅者）
- 被观察者（发布者）

核心思想：观察者只要订阅了被观察者的事件，那么当被观察者的状态改变时，被观察者会主动去通知观察者，而无需关心观察者得到事件后要去做什么，实际程序中可能是执行订阅者的回调函数。同一个被观察者可以同时被多个观察者观察。

`代码实现`
```js
var Event = (function(){
    var list = {},
          listen,
          trigger,
          remove;
          listen = function(key,fn){
            if(!list[key]) {
                list[key] = [];
            }
            list[key].push(fn);
        };
        trigger = function(){
            var key = Array.prototype.shift.call(arguments),
                 fns = list[key];
            if(!fns || fns.length === 0) {
                return false;
            }
            for(var i = 0, fn; fn = fns[i++];) {
                fn.apply(this,arguments);
            }
        };
        remove = function(key,fn){
            var fns = list[key];
            if(!fns) {
                return false;
            }
            if(!fn) {
                fns && (fns.length = 0);
            }else {
                for(var i = fns.length - 1; i >= 0; i--){
                    var _fn = fns[i];
                    if(_fn === fn) {
                        fns.splice(i,1);
                    }
                }
            }
        };
        return {
            listen: listen,
            trigger: trigger,
            remove: remove
        }
})();
// 测试代码如下：
Event.listen("color",function(size) {
    console.log("尺码为:"+size); // 打印出尺码为42
});
Event.trigger("color",42);
```


