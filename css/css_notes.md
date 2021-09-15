- [1、外边距合并问题（collapsing_margins）](#1外边距合并问题collapsing_margins)
- [2、对BFC规范(块级格式化上下文：block formatting context)的理解](#2对bfc规范块级格式化上下文block-formatting-context的理解)
- [3、怎么让Chrome支持小于12px 的文字？](#3怎么让chrome支持小于12px-的文字)
- [4、text-size-adjust的取值及作用](#4text-size-adjust的取值及作用)
- [5、css 伪类与伪元素区别](#5css-伪类与伪元素区别)
- [6、对盒模型的理解](#6对盒模型的理解)
- [7、说一下什么是重绘和重排（回流），哪些操作会造成重绘重排，以及如何优化](#7说一下什么是重绘和重排回流哪些操作会造成重绘重排以及如何优化)
- [8、通过 link 进来的 css 会阻塞页面渲染嘛，Js 会阻塞吗，如果会如何解决？](#8通过-link-进来的-css-会阻塞页面渲染嘛js-会阻塞吗如果会如何解决)
- [9、Css 选择器都有什么，权重是怎么计算的](#9css-选择器都有什么权重是怎么计算的)
- [10、nth-child和nth-type-of 有什么区别](#10nth-child和nth-type-of-有什么区别)
- [11、Css 超出省略怎么写，三行超出省略怎么写](#11css-超出省略怎么写三行超出省略怎么写)
- [12、如何实现文本单行时居中显示，多行时左对齐](#12如何实现文本单行时居中显示多行时左对齐)
- [13、响应式布局方案有哪些？](#13响应式布局方案有哪些)
- [14、移动端适配 1px 的问题 以及解决方案](#14移动端适配-1px-的问题-以及解决方案)
- [15、如何处理移动端适配问题？](#15如何处理移动端适配问题)
- [16、Css 实现 div 宽度自适应，宽高保持等比缩放](#16css-实现-div-宽度自适应宽高保持等比缩放)
- [17、CSS 选择符有哪些？](#17css-选择符有哪些)
- [18、哪些属性可以继承？](#18哪些属性可以继承)
- [19、为什么要清除浮动，以及清除浮动的方法有哪些？](#19为什么要清除浮动以及清除浮动的方法有哪些)
- [20、三列布局，左右固定，中间自适应](#20三列布局左右固定中间自适应)
- [21、水平垂直居中的方法有哪些？](#21水平垂直居中的方法有哪些)
- [22、css3新增了哪些属性？列举一些？ // TODO](#22css3新增了哪些属性列举一些--todo)
- [23、css 中的 currentColor 是干什么的？](#23css-中的-currentcolor-是干什么的)
- [24、为什么要使用 css 预处理器？](#24为什么要使用-css-预处理器)
- [25、浏览器将rem转成px时有精度误差怎么办？](#25浏览器将rem转成px时有精度误差怎么办)
- [26 z-index 的理解？](#26-z-index-的理解)
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

- 概念
  Block-Formatting-context 块级格式化上下文，格式化上下问的意思是：在浏览器的某一块区域里面有一套自己的渲染规则，不影响外部，BFC内部元素不影响外部元素。
  
- 作用
  - 垂直方向上margin重叠 ，若不想重叠可以放在两个不同的BFC里面

  - 利用浮动元素不覆盖BFC的特点做双栏布局

  - 可让仅包含浮动元素的div自适应高度

  - BFC清除浮动

- BFC可以解决的问题
  - 1.（BFC与margin）同一个父级块框下，兄弟元素和父子元素的margin会发生重叠问题
  - 2.（BFC与float）父元素高度塌陷问题、兄弟元素覆盖问题
  
- BFC触发条件
  - float 不为none （浮动元素）
  - overflow 不为 visible
  - display 为 flex ,inline-flex,inline-block,table-cell,table-caption
  - position为 absolute , fixed （绝对定位元素）
  - 根元素

可参考：

- [https://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html](https://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)
- [https://segmentfault.com/a/1190000009545742](https://segmentfault.com/a/1190000009545742)
- [http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html](http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html)


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
> - （2）手淘的 Flexible 方案，该方案基于rem, 根据屏幕宽度和 DPR，动态设置 meta viewport 中的 scale （1/DPR）,以及动态设置 html 的 font-size (document.documentElement.clientWidth / 10),该方法只针对 IOS 有高清显示效果，因为安卓的DPR不规范，所以安卓里都认为 DPR = 1；
> 
> - （3）viewport 方案，即使用 vw 和 vh 单位，实际上 Flexible 方案是 viewport 方案的过渡方案
> 
> - （4）高清显示方案，同样是基于rem，安卓和IOS的DPR都考虑进来了，基准值为100（为了好计算），其html的 font-size 只跟 dpr 有关
> 
> （2）和（3）的理念是一样的，在越宽的屏幕上，元素会越大，因为 html 的 font-size 的值是跟屏幕宽度有关；而（4）是屏幕越大，元素大小不变，因为 html 的 font-size 的值是跟DPR有关
> 
> （2）有一个问题就是通过 rem 计算出来的 px 单位会有小数问题
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


## 19、为什么要清除浮动，以及清除浮动的方法有哪些？

- 浮动会使得元素脱离普通的文档流布局，会导致父元素"高度塌陷"（即父元素的高度不会被子元素撑起来），这样会影响到父元素后面元素的布局。
  
- 清除方法
  - clear：both，添加空元素或者使用伪类
    ```css
    ::after{
        content: "";
        clear: both;
        display: block;
        visibility: hidden;
        height: 0;
    }
    ```
  - overflow:hidden 

## 20、三列布局，左右固定，中间自适应

- flex
  ```html
  <div class="contain">
    <div class="left"></div>
    <div class="main"></div>
    <div class="right"></div>
  </div>
  ```

  ```css
  .contain{
      display: flex;
      height: 600px;
  }
  .left, .right{
      width: 200px;
      height: 100%;
      border: 2px solid green;
  }
  .main{
      flex: 1;
      border: 2px solid red;
  }
  ```

- padding + absolute
  ```css
  .contain{
      height: 600px;
      padding: 0 200px;
      position: relative;
  }
  .left, .right{
      position: absolute;
      width: 200px;
      height: 100%;
      top: 0;
      border: 2px solid green;
  }
  .left{
      left: 0;
  }
  .right{
      right: 0;
  }
  .main{
      width: 100%;
      height: 100%;
      border: 2px solid red;
  }
  ```

- margin + absolute
  ```css
  .contain{
      height: 600px;
      position: relative;
  }
  .left, .right{
      position: absolute;
      width: 200px;
      height: 100%;
      border: 2px solid green;
      top: 0;
  }
  .left{
      left: 0;
  }
  .right{
      right: 0;
  }
  .main{
      margin: 0 200px;
      height: 100%;
      border: 2px solid red;
  }
  ```

- float + margin
  ```html
  <div class="contain">
    <div class="left"></div>
    <div class="right"></div>
    <div class="main"></div>
  </div>
  ```
  ```css
  .contain{
      height: 600px;
  }
  .left, .right{
      width: 200px;
      height: 100%;
      border: 2px solid green;
  }
  .left{
      float: left;
  }
  .right{
      float: right;
  }
  .main{
      margin: 0 200px;
      height: 100%;
      border: 2px solid red;
  }
  ```
...


## 21、水平垂直居中的方法有哪些？

- absolute + 负margin (需知道宽高)
  ```css
  .inner{
    position:absolute;
    margin-left:-1/2*width;
    margin-top:-1/2*width;
    top:50%;
    left:50%; 
  }
  ```

- absolute + auto margin
  ```css
  .inner{
    position:absolute;
    margin: auto;
    top:0;
    left:0;
    right :0;
    bottom:0;
  }
  ```

- absolute + transform
  ```css
  .inner{
    position: absolute;
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
  }
  ```
- table-cell
  ```css
  .outer {
    display: table-cell;
    text-align: center;
    vertical-align: middle;
  }
  ```

- flex
  ```css
  .outer {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  ```
## 22、css3新增了哪些属性？列举一些？ // TODO



## 23、css 中的 currentColor 是干什么的？
> 
> currentColor 当前元素的color值。如果当前元素没有在CSS里显示地指定一个color值，那它的颜色值就遵从CSS规则，从父级元素继承而来。可以在任何需要写颜色的地方使用currentColor这个变量
> 
> 参考：[CSS currentColor 变量的使用](https://www.cnblogs.com/Wayou/p/css-currentColor.html)

## 24、为什么要使用 css 预处理器？

主要是四方面：变量（variables），代码混合（ mixins），嵌套（nested rules）以及 代码模块化(Modules)。

- 变量（variables）
  
  当某个特定的值在多处用到时，变量就是一种简单而有效的抽象方式，设置变量后，不仅便于记忆、阅读和理解，特别是在批量修改起来会很方便。css 中也开始引入变量了（[使用CSS自定义属性（变量）](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties)）

- 代码混合（ mixins）
  
  将相同功能实现的代码提取出来，然后在需要的地方调用，在调用时可以接受参数。

- 嵌套（nested rules）

    选择符嵌套是文件内部的代码组织方式，它可以让一系列相关的规则呈现出层级关系。在之前的话需要手动维护，且不便于修改；在 css 预处理器中，这种嵌套管理很好管理；

- 代码模块化(Modules)
    
    首先，可以将大文件按照一定功能范围切割成不同的小文件，然后在入口文件中，逐层引入所依赖的文件，这样的优势在于：（1）小文件相对于大文件更容易管理；（2）按照功能结构划分后，目录结构上更为清晰。
    css 中也开始支持 @import 引入了（[@import CSS@规则](https://developer.mozilla.org/zh-CN/docs/Web/CSS/%40import)）


## 25、浏览器将rem转成px时有精度误差怎么办？

当使用 Rem 作为 css 单位，其最终转换成 px 显示时，可能会出现小数，会出现显示误差，例如导致原本一行可以放下的由于小数的四舍五入，现在出现换行了。

这种情况，在 REM 布局时，建议，外层容器使用百分比或者 flex, 具体固定宽高的元素再使用 REM。

## 26 z-index 的理解？

1、首先先看要比较的两个元素是否处于同一个层叠上下文中：
  1.1如果是，谁的层叠等级大，谁在上面（怎么判断层叠等级大小呢？——看“层叠顺序”图）。
  1.2如果两个元素不在统一层叠上下文中，请先比较他们所处的层叠上下文的层叠等级。

2、当两个元素层叠等级相同、层叠顺序相同时，在DOM结构中后面的元素层叠等级在前面元素之上。

3、层叠顺序规则

  z-index > 0 大于 z-index: auto/z-index:0 大于 inline/inline-block水平盒子 大于 float浮动盒子 大于 block块级盒子 大于 z-index < 0 


参考：[彻底搞懂CSS层叠上下文、层叠等级、层叠顺序、z-index](https://juejin.cn/post/6844903667175260174)