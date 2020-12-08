
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

## 6、对盒模型的理解 
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

## 8、通过 link 进来的 css 会阻塞页面渲染嘛，Js 会阻塞吗，如果会如何解决？ 
> - css加载不会阻塞DOM树的解析
> - css加载会阻塞DOM树的渲染
> - css加载会阻塞后面js语句的执行
> 
> 为了避免页面空白时间过长，尽量提高CSS的加载
> - 使用CDN(因为CDN会根据你的网络状况，替你挑选最近的一个具有缓存内容的节点为你提供资源，因此可以减少加载时间)
> - 对css进行压缩(可以用很多打包工具，比如webpack,gulp等，也可以通过开启gzip压缩)
> - 合理的使用缓存(设置cache-control,expires,以及E-tag都是不错的，不过要注意一个问题，就是文件更新后，你要避免缓存而带来的影响。其中一个解决防范是在文件名字后面加一个版本号)
> - 减少http请求数，将多个css文件合并
>  
> 参考：[https://zhuanlan.zhihu.com/p/43282197](https://zhuanlan.zhihu.com/p/43282197)

## 9、Css 选择器都有什么，权重是怎么计算的
> - 第一等：代表内联样式，如: style=””，权值为1000。
> - 第二等：代表ID选择器，如：#content，权值为0100。
> - 第三等：代表类，伪类和属性选择器，如.content，权值为0010。
> - 第四等：代表类型选择器和伪元素选择器，如div p，权值为0001。
> - 通配符、子选择器、相邻选择器等的。如*、>、+,权值为0000。
> - 继承的样式没有权值。
> - !important 的作用是提升优先级,其优先级最高
>
> 权重比较是左往右逐个等级比较，前一等级相等才往后比，在权重相同的情况下，后面的样式会覆盖掉前面的样式。
> 
> 参考：[https://www.cnblogs.com/dq-Leung/p/4213375.html](https://www.cnblogs.com/dq-Leung/p/4213375.html)

## 10、nth-child和nth-type-of 有什么区别
> 
> - nth-child 是根据元素的个数来计算的,前面加标签限制也没有用，例如p:nth-child，会将同级的所有标签元素都计算进入
> - nth-type-of 按照类型来选择。例如p:nth-type-of 只会计算标签p，但是如果前面是用类名来限制的，那么同类名的不同标签元素会单独计算
> 
> 参考[https://www.cnblogs.com/pssp/p/5991029.html](https://www.cnblogs.com/pssp/p/5991029.html)

## 11、Css 超出省略怎么写，三行超出省略怎么写
> - 单行
> ```css
>    white-space: nowrap;
>    overflow: hidden;
>    text-overflow: ellipsis;
> ```
>
> - 多行 
> ```css
>    overflow: hidden;
>    text-overflow: ellipsis;
>    display: -webkit-box;
>    -webkit-line-clamp: 2;（行数）
>    -webkit-box-orient: vertical;
> ```
> 需要注意的是 -webkit-line-clamp 是一个 不规范的属性（unsupported WebKit property），存在兼容性问题，只支持 webkit 内核的浏览器
>

## 12、如何实现文本单行时居中显示，多行时左对齐

> ``` html
> <div>
>    <p>标题内容</p>
> </div>
> ```
>
> ```css
> div {
>     padding : 0 2vw;
>     margin : 0 auto; // 使div框居中
>     width : 80vw; 
>     text-align : center; // 文本居中显示
> }
> p {
>     width : auto;  // 必设
>     display : inline-block; // 不能设置为block
>     text-align : left; // 居左显示
> }
> ```


## 13、响应式布局方案有哪些？
> - px和视口
> - 媒体查询
> - 百分比
> - 自适应场景下的rem解决方案
> - 通过vw/vh来实现自适应

参考:[https://juejin.im/post/6844903630655471624?spm=a2c6h.12873639.0.0.6fec2e94fBM0Jl](https://juejin.im/post/6844903630655471624?spm=a2c6h.12873639.0.0.6fec2e94fBM0Jl)


## 14、移动端适配 1px 的问题 以及解决方案
> 
> 设备像素比(dpr) ＝ 物理像素 / 设备独立像素(css像素px)，在 dpr > 1 的屏幕上，1px 像素会显示成多个（dpr个）物理像素，所以会导致看起来较粗。解决方法有如下几种：
>
> - （1）border-image：圆角无法实现
> 
> - （2）background-image
> 
> - （3）背景图渐变：background-image: linear-gradient(top, transparent 50%, $color 50%),
> 
> - （4）0.5px方案
> 
> - （5）伪元素 + transform scale方案
> 
> 其实很多移动端的知名网站，如天猫，淘宝、京东都没有单独处理 1px 像素的问题，腾讯新闻使用的 伪元素 + transform scale方案 

参考：[Web移动端适配方案](https://juejin.im/post/6894044091836563469)

## 15、如何处理移动端适配问题？
> 
> - （1）meta viewport 设置 scale=1.0 配合flex、百分比、媒体查询等，一般设计稿是 750 * 1334 的，代码里写的 px 值需要在设计稿基础上除以 2；（这种方案更像是响应式布局）
> 
> - （2）手淘的 Flexible 方案，该方案基于rem, 根据屏幕宽度和DPR，动态设置 meta viewport 中的 scale （1/DPR）,以及动态设置 html 的 font-size (document.documentElement.clientWidth / 10),该方法只针对 IOS 有高清显示效果，因为安卓的DPR不规范，所以安卓里都认为 DPR = 1；
> 
> - （3）viewport 方案，即使用 vw 和 vh 单位，实际上 Flexible 方案是 viewport 方案的过渡方案
> 
> - （4）高清显示方案，同样是基于rem，安卓和IOS的DPR都考虑进来了，基准值为100（为了好计算），其html的 font-size 只跟 dpr 有关
> 
> （2）和（3）的理念是一样的，在越宽的屏幕上，元素会越大，因为 html 的 font-size 的值是跟屏幕宽度有关；而（4）是屏幕越大，元素大小不变，因为 html 的 font-size 的值是跟DPR有关
> 
> 

参考：[使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)、[Web移动端适配方案](https://juejin.im/post/6894044091836563469)、[手机端页面自适应解决方案—rem布局](https://segmentfault.com/a/1190000007350680)

## 16、Css 实现 div 宽度自适应，宽高保持等比缩放
> - （1） 使用 padding-top 或 padding-bottom
>       一个元素的 padding (margin)，如果值是一个百分比，那这个百分比是相对于其父元素的宽度而言的，padding-bottom 也是如此。
> - (2) vw 和 vh
>       - vw 相对于视窗的宽度(分成100等分)
>       - vh 相对于视窗的高度(分成100等分)
>       元素的宽高都用 vw 单位，按照对应的比例设置对应的值

## 17、CSS 选择符有哪些？

> 1.id选择器（ # myid）
> 2.类选择器（.myclassname）
> 3.标签选择器（div, h1, p）
> 4.相邻选择器（h1 + p）
> 5.子选择器（ul > li）
> 6.后代选择器（li a）
> 7.通配符选择器（ * ）
> 8.属性选择器（a[rel = "external"]）
> 9.伪类选择器（a: hover, li:nth-child）


## 18、哪些属性可以继承？

> 可继承的样式：font-size font-family color, text-indent;
> 
> 不可继承的样式：border padding margin width height ;


## 19、为什么要清除浮动，以及清除浮动的方法有哪些？ // TODO


## 20、三列布局，左右固定，中间自适应 // TODO


## 21、水平垂直居中的方法有哪些？ // TODO


## 22、css3新增了哪些属性？列举一些？ // TODO


## 23、css 中的 currentColor 是干什么的？ // TODO
> 
> currentColor 当前元素的color值。如果当前元素没有在CSS里显示地指定一个color值，那它的颜色值就遵从CSS规则，从父级元素继承而来。可以在任何需要写颜色的地方使用currentColor这个变量
> 
> 参考：[CSS currentColor 变量的使用](https://www.cnblogs.com/Wayou/p/css-currentColor.html)