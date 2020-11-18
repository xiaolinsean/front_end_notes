## 1、介绍下 Https，和 http 的区别是什么？https 为什么比 http 安全？如何进行配置？ // TODO




## 2、PWA 是什么？对 PWA 有什么了解 

    Google 在 2015 年开始着手推广的一类无需下载的应用，命名为 PWA（Progressive Web Apps，渐进式 Web 应用），使用到 service worker 功能。

参考：[https://www.zhihu.com/question/59108831](https://www.zhihu.com/question/59108831)


## 3、CDN 是什么？描述下 CDN 原理？为什么要用 CDN?

    CDN即内容分发网络。其目的是在现有Internet中增加一层新的网络架构，将网站内容发布到最接近用户的网络"边缘"，使用户可以就近取得所需内容，解决Internet网络拥塞状态，提高用户访问网站的响应速度。从技术上全面解决网络带宽小、用户访问量大、网点分布不均等原因造成的用户访问网站响应慢等问题。

    （1）用户在浏览器输入要访问的域名

    （2）浏览器向本地DNS请求域名解析，DNS会将域名解析权转交给CNAME指定的CDN专用的DNS服务器

    （3）CDN专用的DNS服务器将CDN的全局负载均衡设备IP地址返回给浏览器

    （4）浏览器向全局负载均衡设备发出访问请求

    （5）CDN全局负载均衡设备根据请求的URL和用户的IP地址，将用户请求转发到用户所在区域的区域负载均衡设备

    （6）区域负载均衡设备根据URL、用户IP和缓存服务器的负载情况，返回一台合适的服务器IP给用户

    （7）用户向缓存服务器发出访问请求

    （8）缓存服务器响应用户请求，如果用户请求的内容不再缓存服务器上，则缓存服务器要向上一级缓存服务器请求内容，直到追溯到网站的源服务器

参考：[https://www.jianshu.com/p/b5b25b804c62](https://www.jianshu.com/p/b5b25b804c62)

## 4、常见的 http 请求头都有哪些，以及它们的作用 // TODO




## 5、强缓存都有哪些方法来控制？ 

    强缓存：直接从本地副本比对读取，不去请求服务器，返回的状态码是 200。通过 Response 中的 expires 和 cache-control 来控制

    （1） expires ：HTTP1.0 中定义的缓存字段，它是一个时间戳（准确点应该叫格林尼治时间），当客户端再次请求该资源的时候，会把客户端时间与该时间戳进行对比，如果大于该时间戳则已过期，否则直接使用该缓存资源。由于客户端本地时间的不确定性，所以不一定能达到预期效果。

    （2）cache-control： HTTP1.1 中新增的字段，优先级高于expires；它的值常用的有：
        max-age （表示过多少秒后过期）
        no-cache 表示的是不直接询问浏览器缓存情况，而是去向服务器验证当前资源是否更新（即协商缓存）
        no-store 完全不使用缓存策略，不缓存请求或响应的任何内容，直接向服务器请求最新。
        no-cache 和 no-store 直接跳过了浏览器的判断，直接与服务器交互，优先级要高于 max-age

    Cache-Control 的优先级要高于 expires

## 6、协商缓存都有哪些参数

    协商缓存：不直接从浏览器端取缓存，而是会去服务器比对，若没改变才直接读取本地缓存，返回的状态码是 304。通过 last-modified 和 etag 来控制。

    （1）last-modified：记录资源最后修改的时间。启用后，请求资源之后的响应头会增加一个 last-modified 字段，当再次请求该资源时，请求头中会带有 if-modified-since 字段，值是之前返回的 last-modified 的值，如：if-modified-since:Thu, 20 Dec 2018 11:36:00 GMT。服务端会对比该字段和资源的最后修改时间，若一致则证明没有被修改，告知浏览器可直接使用缓存并返回 304；若不一致则直接返回修改后的资源，并修改 last-modified 为新的值。

        但 last-modified 有以下两个缺点：

            只要编辑了，不管内容是否真的有改变，都会以这最后修改的时间作为判断依据，当成新资源返回，从而导致了没必要的请求响应，而这正是缓存本来的作用即避免没必要的请求。
            
            时间的精确度只能到秒，如果在一秒内的修改是检测不到更新的，仍会告知浏览器使用旧的缓存。

    （2）etag： 会基于资源的内容编码生成一串唯一的标识字符串，只要内容不同，就会生成不同的 etag。启用 etag 之后，请求资源后的响应返回会增加一个 etag 字段，当再次请求该资源时，请求头会带有 if-no-match 字段，值是之前返回的 etag 值，如：if-no-match:"FllOiaIvA1f-ftHGziLgMIMVkVw_"。服务端会根据该资源当前的内容生成对应的标识字符串和该字段进行对比，若一致则代表未改变可直接使用本地缓存并返回 304；若不一致则返回新的资源（状态码200）并修改返回的 etag 字段为新的值。

    etag 比 last-modified 的优先级跟高，不过 etag 也增加了服务器的开销。

    先走强缓存，没有设置强缓存或者强缓存失效了，再走协商缓存，所以总的优先级应该是： Cache-Control  > expires > Etag > Last-Modified

    ctrl + F5 强制刷新，直接跳过强缓存和协商缓存，请求新的网络资源；

    F5刷新，各浏览器不一样，默认是跳过强缓存，走协商缓存，但是有些浏览器也会先走协商缓存。

参考：[https://www.jianshu.com/p/fb59c770160c](https://www.jianshu.com/p/fb59c770160c)



## 7、浏览器出现 from disk、from memory 的策略是啥 

    from disk cache 和 from memory cache 都是属于 强缓存 （form cache）

    （1）200 form memory cache : 不访问服务器，一般已经加载过该资源且缓存在了内存当中，直接从内存中读取缓存。浏览器关闭后，数据将不存在（资源被释放掉了），再次打开相同的页面时，不会出现from memory cache，而是 from disk cache。在 chrome 中，一般脚本文件（js）、图片等，会缓存到内存中；

    （2）200 from disk cache： 不访问服务器，已经在之前的某个时间加载过该资源，直接从硬盘中读取缓存，关闭浏览器后，数据依然存在，此资源不会随着该页面的关闭而释放掉下次打开仍然会是from disk cache。在 chrome 中，一般css文件，会缓存到硬盘中；

    from disk cache 和 from memory cache 在 network 面板里 都显示在 size 一栏， size 一栏还会显示确切的数字大小，也分两种情况：

    （3）code为200： 网络资源的真实大小，没有从缓存里取。

    （4）code为304：请求报文的大小，此时走的是协商缓存，真实资源还是从本地缓存里取的。


## 8、说一下 Nginx 的缓存策略，强缓存与协商缓存的区别，二者的使用场景 

    Nginx 默认会给静态资源加上协商缓存，即 last-modified 和 etag。但是对于一些静态资源，我们希望直接走强缓存从本地取资源，这时候，一般可以设置过期时间：

    location ~ .*\.(js|css)?${ expires 90d; }  设置过期时间 90 天， 比如一些 js 、 css 文件、图片等静态资源，我们希望内容没有改变的情况下，走强缓存，这样可以增加速度，虽然有 last-modified 和 etag 保证，正常情况下，我们为了区分版本，会给需要强缓存的文件加上 contentHash 值。

    location ~ /web/config/urlconfig.js$ { expires -1; }  对于一些配置文件，我们希望能实时更新，可以设置不走强缓存，同时如果要每次请求都获取最新值的话，我们可以再文件请求后面加上时间戳


## 9、请描述 CSRF 的基本概念、攻击原理和防御措施？ // TODO

    


## 10、请描述 XSS 的基本概念、攻击原理和防御措施？ // TODO

    跨站脚本攻击（Cross-site scripting，XSS）是一种安全漏洞，攻击者可以利用这种漏洞在网站上注入恶意的 Script代码。当被攻击者登陆网站时就会自动运行这些恶意代码，从而，攻击者可以突破网站的访问权限，冒充受害者，这些脚本可以任意读取 cookie，session tokens，或者其它敏感的网站信息，或者让恶意脚本重写HTML内容。


## 11、localstorage、sessionStorage、indexDB 和 cookie 的区别 // TODO




## 12、介绍下数字签名的原理 // TODO




## 13、对称加密和非对称加密的区别和用处 // TODO




## 14、cookie、token 和 session 的比较  // TODO




## 15、http 常见的状态码以及代表的意义 // TODO





## 16、简单请求和复杂请求 // TODO




## 17、


