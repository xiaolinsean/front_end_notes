
## 1、外边距合并问题（collapsing_margins）

> 外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。
> 
> 
> 合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。
> 
> 可参考：
> - [http://www.w3school.com.cn/css/css_margin_collapsing.asp](http://www.w3school.com.cn/css/css_margin_collapsing.asp)
>
> - [https://geekplux.com/2014/03/14/collapsing_margins](https://geekplux.com/2014/03/14/collapsing_margins)
> 


## 2、对BFC规范(块级格式化上下文：block formatting context)的理解
> 可参考：
> 
> - [https://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html](https://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)
> - [https://segmentfault.com/a/1190000009545742](https://segmentfault.com/a/1190000009545742)
> - [http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html](http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html)

## 3、怎么让Chrome支持小于12px 的文字？

>- 用图片：如果是内容固定不变情况下，使用将小于12px文字内容切出做图片，这样不影响兼容也不影响美观。（缺点：灵活性不够，图片相对于代码实现还是太大）
>- -webkit-text-size-adjust:none; 字体大小 就不受限制了。（缺点：只在chrome 27版本之前有效）
>- 使用css3属性 -webkit-transform : scale()  方法来解决呢。
>  
>   `{font-size : 12px;webkit-transform : scale(0.84,0.84) ;}`



## 4、text-size-adjust的取值及作用
>  取值：
>
>- auto：文本大小根据设备尺寸进行调整。（默认）
>
>- none：文本大小不会根据设备尺寸进行调整。
>
>- `<percentage>`：用百分比来指定文本大小在设备尺寸不同的情况下如何调整。
>
> 检索或设置移动端页面中对象文本的大小调整。
>- 该属性只在移动设备上生效；
>  
>- 如果你的页面没有定义meta viewport，此属性定义将无效；


## 5、css 伪类与伪元素区别

> - 伪类: 用于向某些选择器增加样式效果，需要添加样式的选择器元素已经存在于DOM中，
> - 伪元素: 用于将样式效果添加到某些选择器上，需要添加样式的选择器还不存在于DOM中。
> 
>  伪类的效果可以通过添加一个实际的类来达到，而伪元素的效果则需要通过添加一个实际的元素才能达到，这也是为什么他们一个称为伪类，一个称为伪元素的原因。
>
>  为了区分两者，CSS3 中已经明确规定了伪类用一个冒号来表示，而伪元素则用两个冒号来表示，当然，伪元素用一个冒号来表示也是能识别的。但是一些特别低版本的浏览器不支持两个冒号，如果抛开兼容性，为了区分，我们在写代码的时候还是应该做区分
>
> 参考：[https://www.jianshu.com/p/686c22e2c76b](https://www.jianshu.com/p/686c22e2c76b)

## 6、对盒模型的理解 // TODO
> - 标准模型：盒模型的宽高只是内容（content）的宽高，
> - IE模型: 盒模型的宽高是内容(content)+填充(padding)+边框(border)的总宽高
>
> ```css
> /* 标准模型 */
> box-sizing:content-box;
>
> /*IE模型*/
> box-sizing:border-box;
> ```
>

## 7、说一下什么是重绘和重排（回流），哪些操作会造成重绘重排，以及如何优化
>
> - 重绘:当元素的一部分属性发生改变，如外观、背景、颜色等不会引起布局变化，只需要浏览器根据元素的新属性重新绘制
    ，使元素呈现新的外观叫做重绘
> - 重排（回流）:当render树中的一部分或者全部因为大小边距等问题发生改变而需要DOM树重新计算的过程
> 重绘不一定需要重排（比如颜色的改变），重排必然导致重绘（比如改变网页位置）
>
> 参考:[https://www.cnblogs.com/qing-5/p/11341196.html](https://www.cnblogs.com/qing-5/p/11341196.html)
>  

## 8、通过 link 进来的 css 会阻塞页面渲染嘛，Js 会阻塞吗，如果会如何解决？ // TODO


## 9、Css 选择器都有什么，权重是怎么计算的 // TODO


## 10、nth-child和nth-type-of 有什么区别 // TODO


## 11、Css 超出省略怎么写，三行超出省略怎么写 // TODO


## 12、如何实现文本单行时居中显示，多行时左对齐 // TODO


## 13、屏幕占满和未占满的情况下，使 footer 固定在底部，尽量多种方法 // TODO


## 14、响应式布局用到的技术，移动端需要注意什么 // TODO


## 15、移动端适配 1px 的问题 // TODO


## 16、Css 实现 div 宽度自适应，宽高保持等比缩放 // TODO



## 17、