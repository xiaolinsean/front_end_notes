## 外边距合并问题（collapsing_margins）

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


## 对BFC规范(块级格式化上下文：block formatting context)的理解


## 怎么让Chrome支持小于12px 的文字？

>- 用图片：如果是内容固定不变情况下，使用将小于12px文字内容切出做图片，这样不影响兼容也不影响美观。（缺点：灵活性不够，图片相对于代码实现还是太大）
>- -webkit-text-size-adjust:none; 字体大小 就不受限制了。（缺点：只在chrome 27版本之前有效）
>- 使用css3属性 -webkit-transform : scale()  方法来解决呢。
>  
>   `{font-size : 12px;webkit-transform : scale(0.84,0.84) ;}`



## text-size-adjust的取值及作用
> 取值：
>
>- auto：文本大小根据设备尺寸进行调整。（默认）
>
>- none：文本大小不会根据设备尺寸进行调整。
>
>- `<percentage>`：用百分比来指定文本大小在设备尺寸不同的情况下如何调整。
>
>检索或设置移动端页面中对象文本的大小调整。
>- 该属性只在移动设备上生效；
>  
>- 如果你的页面没有定义meta viewport，此属性定义将无效；