# 基础题

## <xx.com> 向 <mp.xx.com> 发请求跨域吗？<mp.xx.com> 的服务器能接收到请求吗？是怎样的请求？

跨域，因为域名不同。

服务器端可以接收到请求。

![跨域后端获取到的请求](front-interview-cross-domain.png)

跨域请求，后端拿不到 cookie，x-requested-with，新增 referer 字段。

返回的都是 200 OK。

## 请解释 XSS 与 CSRF 分别是什么？两者有什么联系，如何防御？

[参考链接](fontend-security.html)

## 关乎 Javascript Bridge。

1、解释一下什么是 Javascript Bridge。

2、Javascript Bridge 的实现原理。

3、你所了解的 Javascript Bridge 通讯中的优化方案。

JSBridge 是一座用 JavaScript 搭建起来的桥，一端是 web，一端是 native。我们搭建这座桥的目的也很简单，让 native 可以调用 web 的 js 代码，让 web 可以 “调用” 原生的代码。请注意这个我加了 引号的调用，它并不是直接调用，而是可以根据 web 和 native 约定好的规则来通知 native 要做什么，native 可以更具这个来执行相应的代码。

![jsbridge原理](front-interview-jsbridge.png)

## TCP/UDP 是什么？

### TCP：

优点：可靠 稳定

TCP 的可靠体现在 TCP 在传输数据之前，会有三次握手来建立连接，而且在数据传递时，有确认. 窗口. 重传. 拥塞控制机制，在数据传完之后，还会断开来连接用来节约系统资源。

缺点：慢，效率低，占用系统资源高。

在传递数据之前要先建立连接，这会消耗时间，而且在数据传递时，确认机制. 重传机制. 拥塞机制等都会消耗大量时间，而且要在每台设备上维护所有的传输连接。然而，每个连接都会占用系统的 CPU，内存等硬件资源。

### UDP：

优点：快。

UDP 没有 TCP 拥有的各种机制，是一种无状态的传输协议，所以传输数据非常快，没有 TCP 的这些机制，被攻击利用的机会就少一些，但是也无法避免被攻击。

缺点：不可靠，不稳定。

因为没有 TCP 的这些机制，UDP 在传输数据时，如果网络质量不好，就会很容易丢包，造成数据的缺失。

## 如何处理高流量，高并发？

1、减少请求数（合并 js，css，图片等）。

2、减少资源大小（压缩，删掉无用代码）。

3、静态资源放 CDN。

4、过滤请求，使用本地缓存（缓存策略），减少服务器压力。

5、使用压力测试，测试单个服务器的最大 QPS，从而计算出后端多台服务器集群的抗压能力。

6、前端错误日志（监听 window.onerror 等）。

7、后端错误日志记录（process.on('uncaughtException')等）。

8、nginx 负载均衡。

9、后端守护进程（pm2），心跳检测。

10、Varnish，Stupid 后端缓存。

11、数据库读写分离。

## 反爬虫方案？

1、通过 User-Agent 来控制访问（可以被伪造）。

2、通过 IP 限制来反爬虫。（如果一个固定的 ip 在短暂的时间内，快速大量的访问一个网站，那自然会引起注意，管理员可以通过一些手段把这个 ip 给封了）。

3、通过 JS 脚本来防止爬虫。（如验证码，滑动解锁等）。

4、通过 robots.txt 来限制爬虫。（君子协议）。

## 什么是 BOM？

BOM（Browser Object Model）即浏览器对象模型。
BOM 提供了独立于内容 而与浏览器窗口进行交互的对象；
由于 BOM 主要用于管理窗口与窗口之间的通讯，因此其核心对象是 window；
BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性；
BOM 缺乏标准，JavaScript 语法的标准化组织是 ECMA，DOM 的标准化组织是 W3C。

常用对象：location，navigation，history，screen，frames。

![bom模型图](https://images2015.cnblogs.com/blog/997049/201608/997049-20160830235030324-1067760196.jpg)

## HTTP 请求 OPTION

HTTP 请求方法并不是只有 GET 和 POST，只是最常用的。据 RFC2616 标准（现行的 HTTP/1.1）得知，通常有以下 8 种方法：OPTIONS、GET、HEAD、POST、PUT、DELETE、TRACE 和 CONNECT。

OPTIONS 请求方法的主要用途有两个：

1、获取服务器支持的 HTTP 请求方法。

2、用来检查服务器的性能。例如：AJAX 进行跨域请求时的预检，需要向另外一个域名的资源发送一个 HTTP OPTIONS 请求头，用以判断实际发送的请求是否安全。