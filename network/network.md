- [1、介绍下 Https，和 http 的区别是什么？https 为什么比 http 安全？如何进行配置？](#1介绍下-https和-http-的区别是什么https-为什么比-http-安全如何进行配置)
- [2、PWA 是什么？对 PWA 有什么了解](#2pwa-是什么对-pwa-有什么了解)
- [3、CDN 是什么？描述下 CDN 原理？为什么要用 CDN?](#3cdn-是什么描述下-cdn-原理为什么要用-cdn)
- [4、常见的 http 请求头都有哪些，以及它们的作用 // TODO](#4常见的-http-请求头都有哪些以及它们的作用--todo)
- [5、强缓存都有哪些方法来控制？](#5强缓存都有哪些方法来控制)
- [6、协商缓存都有哪些参数](#6协商缓存都有哪些参数)
- [7、浏览器出现 from disk、from memory 的策略是啥](#7浏览器出现-from-diskfrom-memory-的策略是啥)
- [8、说一下 Nginx 的缓存策略，强缓存与协商缓存的区别，二者的使用场景](#8说一下-nginx-的缓存策略强缓存与协商缓存的区别二者的使用场景)
- [9、请描述 CSRF 的基本概念、攻击原理和防御措施？](#9请描述-csrf-的基本概念攻击原理和防御措施)
- [10、请描述 XSS 的基本概念、攻击原理和防御措施？](#10请描述-xss-的基本概念攻击原理和防御措施)
- [11、localstorage、sessionStorage、indexDB 和 cookie 的区别 // TODO](#11localstoragesessionstorageindexdb-和-cookie-的区别--todo)
- [12、介绍下数字签名、数字证书的原理 // TODO](#12介绍下数字签名数字证书的原理--todo)
- [13、对称加密和非对称加密的区别和用处](#13对称加密和非对称加密的区别和用处)
- [14、cookie、token 和 session 的比较](#14cookietoken-和-session-的比较)
- [15、http 常见的状态码以及代表的意义](#15http-常见的状态码以及代表的意义)
- [16、简单请求和复杂请求](#16简单请求和复杂请求)
- [17、](#17)

## 1、介绍下 Https，和 http 的区别是什么？https 为什么比 http 安全？如何进行配置？

- HTTP和HTTPS的主要区别如下包括:

    - HTTPS协议需要到CA申请证书,而HTTP协议则不用；
    - HTTP是超文本传输协议，信息是明文传输，而HTTPS则是加密传输，比HTTP协议安全；
    - HTTP和HTTPS使用完全不同的连接方式，所占用的端口也不一样，前者占用80端口，后者占用443端口；
    - HTTPS传输过程比较复杂，对服务端占用的资源比较多，由于握手过程的复杂性和加密传输的特性导致HTTPS传输的效率比较低。

- https = http + TLS/SSL

    - https 使用了 对称加密和非对称加密
        - 非对称加密由于较复杂，且较安全，所以用于验证数字签名和传输对称秘钥
        - 对称加密：由于较为简单，用于传输内容的加密
  
    - https 需要使用到第三方认真机构（CA）的证书（下载浏览器端），数字证书 = 网站信息（含公钥） + 数字签名
        - 数字签名：网站的信息加密后通过第三方机构的私钥再次进行加密，生成数字签，这样可以防止中间人串改公钥

参考：[看图学HTTPS](https://juejin.cn/post/6844903608421449742)、[深入理解HTTPS工作原理](https://juejin.cn/post/6844903830916694030)

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



参考：[前端基础篇之HTTP协议](https://juejin.cn/post/6844903844216832007#heading-13)
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

    F5刷新，各浏览器不一样，默认是跳过强缓存，走协商缓存，但是有些浏览器也会先走强缓存。

参考：[https://www.jianshu.com/p/fb59c770160c](https://www.jianshu.com/p/fb59c770160c)



## 7、浏览器出现 from disk、from memory 的策略是啥 

    from disk cache 和 from memory cache 都是属于 强缓存 （form cache）

    （1）200 form memory cache : 不访问服务器，一般已经加载过该资源且缓存在了内存当中，直接从内存中读取缓存。浏览器关闭后，数据将不存在（资源被释放掉了），再次打开相同的页面时，不会出现from memory cache，而是 from disk cache。在 chrome 中，一般脚本文件（js）、图片等，会缓存到内存中；

    （2）200 from disk cache： 不访问服务器，已经在之前的某个时间加载过该资源，直接从硬盘中读取缓存，关闭浏览器后，数据依然存在，此资源不会随着该页面的关闭而释放掉下次打开仍然会是from disk cache。在 chrome 中，一般 html、css 文件，会缓存到硬盘中；

    from disk cache 和 from memory cache 在 network 面板里 都显示在 size 一栏， size 一栏还会显示确切的数字大小，也分两种情况：

    （3）code为200： 网络资源的真实大小，没有从缓存里取。

    （4）code为304：请求报文的大小，此时走的是协商缓存，真实资源还是从本地缓存里取的。

## 8、说一下 Nginx 的缓存策略，强缓存与协商缓存的区别，二者的使用场景 

    Nginx 默认会给静态资源加上协商缓存，即 last-modified 和 etag。但是对于一些静态资源，我们希望直接走强缓存从本地取资源，这时候，一般可以设置过期时间：

    location ~ .*\.(js|css)?${ expires 90d; }  设置过期时间 90 天， 比如一些 js 、 css 文件、图片等静态资源，我们希望内容没有改变的情况下，走强缓存，这样可以增加速度，虽然有 last-modified 和 etag 保证，正常情况下，我们为了区分版本，会给需要强缓存的文件加上 contentHash 值。

    location ~ /web/config/urlconfig.js$ { expires -1; }  对于一些配置文件，我们希望能实时更新，可以设置不走强缓存，同时如果要每次请求都获取最新值的话，我们可以再文件请求后面加上时间戳

## 9、请描述 CSRF 的基本概念、攻击原理和防御措施？

- 跨站请求伪造（英语：Cross-site request forgery），也被称为 one-click attack 或者 session riding，通常缩写为 CSRF 或者 XSRF， 是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法。跨站请求攻击，简单地说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并运行一些操作（如发邮件，发消息，甚至财产操作如转账和购买商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。这利用了web中用户身份验证的一个漏洞：简单的身份验证只能保证请求发自某个用户的浏览器，却不能保证请求本身是用户自愿发出的。

- 举例：
  
    假如一家银行用以运行转账操作的URL地址如下：`http://www.examplebank.com/withdraw?account=AccoutName&amount=1000&for=PayeeName`

    那么，一个恶意攻击者可以在另一个网站上放置如下代码： `<img src="http://www.examplebank.com/withdraw?account=Alice&amount=1000&for=Badman">`

    如果有账户名为Alice的用户访问了恶意站点，而她之前刚访问过银行不久，登录信息尚未过期，那么她就会损失1000资金。

- 防御措施
  
  - 检查Referer字段: Referer 字段应和请求的地址位于同一域名下，但是部分浏览器中 Referer 字段也不固定。
  - 添加校验token

参考：[Web安全漏洞之CSRF](https://juejin.cn/post/6844903681591083015)
## 10、请描述 XSS 的基本概念、攻击原理和防御措施？

- 跨站脚本攻击（Cross-site scripting，XSS）是一种安全漏洞，攻击者可以利用这种漏洞在网站上注入恶意的 Script代码。当被攻击者登陆网站时就会自动运行这些恶意代码，从而，攻击者可以突破网站的访问权限，冒充受害者，这些脚本可以任意读取 cookie，session tokens，或者其它敏感的网站信息，或者让恶意脚本重写HTML内容。

- 预防
  
  增加对数据的编码和解码

## 11、localstorage、sessionStorage、indexDB 和 cookie 的区别 // TODO




## 12、介绍下数字签名、数字证书的原理 // TODO

- 数字签名：


- 数字证书：


参考：[数字签名，数字证书，HTTPS](https://juejin.cn/post/6844904049993580552)


## 13、对称加密和非对称加密的区别和用处

- 对称加密:
  
    - 加密（encryption）与解密（decryption）用的是同样的密钥（secret key），发送方使用密钥将明文数据加密成密文，然后发送出去，接收方收到密文后，使用同一个密钥将密文解密成明文读取

    - 优点：算法公开、计算量小、加密速度快、加密效率高，适合对大量数据进行加密的场景。

    - 缺点：安全性不高，只要拿到秘钥就可以把数据解开。
  
    - 常见的对称加密算法有 DES、3DES、AES、Blowfish、IDEA、RC5、RC6，

- 非对称加密
  
    - 非对称加密算法需要一对密钥：公开密钥（publickey：简称公钥）和私有密钥（privatekey：简称私钥）。公钥与私钥是一对由加密算法计算得出的密钥，如果用公钥对数据进行加密，只有用对应的私钥才能解密。同理，私钥加密的数据，只有公钥能解密。 因为加密和解密使用的是两个不同的密钥，所以这种算法叫作非对称加密算法
  
    - 优点：安全性更高，公钥是公开的，私钥是自己保存的，不需要将私钥提供给别人
  
    - 加解密速度慢，只适合应对小数据加解密
  
    - 常见的非对称加密有 RSA、ECC（椭圆曲线加密算法）、Diffie-Hellman、El Gamal、DSA（数字签名用）， git 仓库托管平台配置 ssh 公钥时使用的就是 RSA

- 混合加密：结合对称加密和非对称加密的优点，对称加密的密钥使用非对称加密的公钥进行加密，然后发送出去，接收方使用私钥进行解密得到对称加密的密钥，然后双方可以使用对称加密来进行沟通。

`crypto-js` 库包含了常用的加密算法。


## 14、cookie、token 和 session 的比较

- cookie
    
    cookie 是一个非常具体的东西，指的就是浏览器里面能永久存储的一种数据。跟服务器没啥关系，仅仅是浏览器实现的一种数据存储功能。服务器和客户端都可以设置，客户端可以通过 `document.cookie = "name=xiaoming; age=12 "`，服务端可以通过 `set-cookie`,可设置的属性包括 expires, domain, path, secure（https中）, HttpOnly（只能服务端设置）

    cookie 会跟随每次请求发送，且是明文的，所以，不应该保存太多数据，不保存重要信息（例如账号密码等）。

- session（服务端session + 客户端 sessionId）
    
    - 服务器向用户返回一个session_id, 写入用户的cookie
    - 用户随后的每一次请求, 都会通过cookie, 将session_id传回服务器
    - 服务端收到 session_id, 找到前期保存的数据, 由此得知用户的身份
    
    扩展性不好，单机服务没有问题，涉及到多机器时，需要考虑均衡及集群
    - 同一个 session 只会打到同一个服务节点上，NGINX 可以设置 IP_HASH，这样同一个 IP 都会打到相同的服务节点上，但是如果碰到很多人使用同一个IP，或者服务前面需要经过一层防火墙服务，导致所有的请求都是相同 IP，这样 NGINX 如果使用 IP_HASH 就起不到均衡的作用，可以使用第三方模块`nginx-sticky-module-ng` 会在 cookie 中增加一个 route 字段来区分是否为同一个会话。
  
    - 共享Session：将Session Id 集中存储到一个地方，所有的机器都来访问这个地方的数据，例如使用 redis 集群来存储 session 信息。

- token 
    
    客户端发送登陆请求后，服务端验证并返回一个签名的token给客户端，客户端储存token, 并且每次用每次发送请求，服务端验证Token并返回数据，由于 token 是服务端添加过签名的，所以可以防串改。较常用的就是 JWT（JSON Web Token）：

    - JWT 由三部分组成：Base64（Header）（头部）. Base64（Payload）（负载）. Signature（签名）,中间用 `.` 连接的字符串。
    - Header 是一个 JSON 对象
        ```json
        {
            "alg": "HS256", // 表示签名的算法，默认是 HMAC SHA256（写成 HS256）
            "typ": "JWT"  // 表示Token的类型，JWT 令牌统一写为JWT
        }
        ```
    - Payload 部分也是一个 JSON 对象，用来存放实际需要传递的数据
      ```json
        {
            // 7个官方字段
            "iss": "a.com", // issuer：签发人
            "exp": "1d", // expiration time： 过期时间
            "sub": "test", // subject: 主题
            "aud": "xxx", // audience： 受众
            "nbf": "xxx", // Not Before：生效时间
            "iat": "xxx", // Issued At： 签发时间
            "jti": "1111", // JWT ID：编号
            // 可以定义私有字段
            "name": "John Doe",
            "admin": true
        }
        ```
    - Signature 是对前两部分的签名，防止数据被篡改。需要指定一个密钥(secret)。这个密钥只有服务器才知道，不能泄露给用户。然后，使用Header里面指定的签名算法（默认是 HMAC SHA256），按照下面的公式产生签名。
        ```json
        Signature = HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)
        ```
    客户端收到服务器返回的 JWT，可以储存在 Cookie 里面，也可以储存在 localStorage。此后，客户端每次与服务端通信，都要带上这个JWT。更好的做法是放在HTTP请求的头信息 Authorization 字段里面。
    ```js
    Authorization: Bearer <token>
    ```

    需要注意的是 JWT 中的数据是不加密的（只做了Base64编码），客户端可以查看到，所以不应该保存敏感信息。

参考：[详解 Cookie，Session，Token](https://juejin.cn/post/6844903864810864647)
## 15、http 常见的状态码以及代表的意义

- 1xx表示继续发送请求
    - 100（Continue）
    - 101（Switching Protocols）

- 2xx表示请求成功
    - 200（OK）
    - 201（Created）
    - 202（Accepted）
    - 204（No Content）
    - 205（Reset Content）
    - 206（Partial Content）

- 3xx表示资源已找到但需要继续进行其他操作
    - 301（Moved Permanently）
    - 302（Found）
    - 303（See Other）
    - 304（Not Modified）

- 4xx表示客户端错误
    - 400（Bad Request）
    - 401（Unauthorized）
    - 403（Forbidden）
    - 404（Not Found）
    - 405（Method Not Allowed）

- 5xx表示服务器错误。
    - 500（Internal Server Error）
    - 502（Bad Gateway）
    - 504（Gateway Time-out）

参考：[如何理解HTTP响应的状态码？](https://harttle.land/2015/08/15/http-status-code.html)
## 16、简单请求和复杂请求

浏览器发送 CORS 请求（跨域请求）时, 会将请求分为简单请求与复杂请求.

- 常用的简单请求可以将其归为以下几点:

    - 请求的方法只能为HEAD、GET、POST

    - 无自定义请求头
  
    - Content-Type只能是这几种：
        text/plain
        multipart/form-data
        application/x-www-form-urlencoded

- 复杂请求（可以理解为除以上的简单请求都是复杂请求）:

    - PUT, Delete 方法的 ajax 请求
    - 发送 JSON 格式的 ajax 请求(比如post数据)
    - 带自定义头的 ajax 请求

如果是简单请求, 则会先执行, 后判断。执行的过程大致如下:

- 浏览器检测到请求是 CORS 请求, 添加一个origin字段(其中包含页面源信息: 协议、域名、端口) ====> 服务端收到后作相应的处理(对比origin, 服务端判断这个源是否接受)返回结果给浏览器  ====> 浏览器检查响应头是否允许跨域信息  ====> 允许, 那就当做没事发生。 不允许, 浏览器抛出相应的错误信息。

- 复杂请求在发生请求时, 如果是 CORS 请求，浏览器预先发送一个 option 请求。浏览器这种行为被称之为预检请求（注意如果不是跨域请求就不会发生预检请求，比如反向代理）。

## 17、


