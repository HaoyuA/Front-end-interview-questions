这道题的考的是从输入url 到页面渲染经历了哪些过程 经典题 考的就是你的web基础

我觉得作为初级前端 首先要对其大概的过程有一个清晰的认知 然后再一步步去扣细节

其中经历了如下过程：



### 构建请求

浏览器会构建请求行:

```js
// 请求方法是GET，路径为根路径，HTTP协议版本为1.1
GET / HTTP/1.1
```

* `curl`命令 可以发http请求 我试过发 `curl -v https://www.baidu.com` 和 `curl -v http://www.baidu.com`  是不一样的 能看到有ssl 的过程



### 查找强缓存

先检查强缓存，如果命中直接使用，否则进入下一步。

* 基本所有情况下 index.html 都不会缓存 为什么index.html不能缓存

如果首页都缓存了 那么怎么知道去更新某个资源呢 所以还是不能缓存



### DNS解析

由于我们输入的是一个域名，这时候如果需要数据通信的话 实际上是客户端和服务端的通信 而服务端就是服务器 所以我们需要一个服务器的 ip 地址。 **DNS**(Domain Name System) 就是一个将域名和ip地址一一对应的系统 将域名解析成 ip 的过程就是 DNS 解析

* 浏览器是怎么知道DNS解析服务器的？ 

本机的网络设置可以看到当前的DNS服务器IP 浏览器能获取系统的DNS服务器 具体不探究

* 一般宽带服务商都会提供DNS服务器 那么入网设备是这么获取到DNS服务器地址呢？

通过动态主机配置协议(DHCP)，当一台设备连到路由器之后，路由器通过DHCP给它分配一个IP地址，并告诉它DNS服务器，如下路由器的DHCP设置：

* DNS 有缓存吗？

有  浏览器会会记住已经解析过的域名 cache，下次直接走缓存，不需要经过 `DNS解析`。



### TCP连接

**TCP**传输控制协议 (Transmission Control Protocol) 连接的目的是为了保证 信息有序不丢包 建立可靠的数据传输

其中经历的过程又分为：

1. 三次握手

![http-2](assets/http-2.png)

2. 数据传输

3. 四次挥手

![http-3](assets/http-3.png)

详细见[TCP三次握手和四次挥手](https://www.yuque.com/haoyu-rqded/cdsnq6/qvytfy)



### HTTP请求和响应

#### HTTP请求

TCP 链接建立完毕后 浏览器和服务器就可以开始通信了 

HTTP请求过程 就是浏览器会设置好请求报文 并通过TCP协议发送到服务器指定端口过程

请求报文 = **请求行** + **请求头** + **空行** + **请求体**   下面一个个看

空行: 协议中规定请求头和请求主体间必须用一个空行隔开

**请求行** = 请求方法 + 请求路径 + 协议版本

```javascript
// 请求方法是GET，路径为根路径，HTTP协议版本为1.1
GET / HTTP/1.1
```



 **请求头** 则是一些附加的信息 一般以键值对形式存在

以访问 https://www.baidu.com/ 为例 请求头中常见字段是

``` javascript
Host: www.baidu.com
Connection: keep-alive
Cache-Control: max-age=0
User-Agent: Mozilla/5.0 .... 
Accept:text/html,application/xhtml+xml,application/xml;q=0.9...
Cookie: ....
```



**请求体**

对于POST请求，所需要的参数都不会放在url中，这时候就需要一个载体了，这个载体就是请求主体

GET 一般没有请求体



#### HTTP响应

服务端收到这个请求后，会根据url匹配到的路径做相应的处理，最后返回浏览器需要的页面资源。处理后，浏览器会收到一个响应报文

响应报文 = **响应行** + **响应头** +  **空行** + **响应体**

状态行 = 协议版本 + 状态码 + 状态描述

```js
HTTP/1.1 200 OK
```



响应头和请求头类似 也会有服务器返回的一些信息

以访问 https://www.baidu.com/ 为例 响应头中常见字段是

```javascript
Cache-Control: private
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html;charset=utf-8
Date: Tue, 09 Mar 2021 11:16:47 GMT
Expires: Tue, 09 Mar 2021 11:16:46 GMT
Server: BWS/1.1
...
```



### HTML词法、语法解析



拿到了HTML 后 由于浏览器不知道HTML是什么 这只是一堆"字符流"

于是 浏览器需要将字符流解析程可悲浏览器识别的结构 这种数据结构就是`DOM树` 需要做的事情分两步



**词法分析**：把字符流初步解析成我们可理解的"词"，学名叫token。 (像是HTML从一片白到高亮？)

**语法分析**：把开始结束标签配对、属性赋值好、父子关系连接好、构成DOM树 。



以Chrome 为例

构成DOM树 后  也需要确定元素的布局才行 于是 就是开始解析CSS 生成一颗  CCSOM树 然后将其合成为渲染树

[google官方文档](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction?hl=zh-cn)

![http-4](assets/http-4.png)





### 渲染



![http-1](assets/http-1.png)

有了渲染树，我们就可以进入“**布局**”阶段。   （**layout**）

这个阶段浏览器会计算它们在设备[视口](https://developers.google.com/web/fundamentals/design-and-ux/responsive?hl=zh-cn#set-the-viewport)内的确切位置和大小---这就是“布局”阶段，也称为“自动重排”。

布局流程的输出是一个“盒模型”，它会精确地捕获每个元素在视口内的确切位置和尺寸: 所有相对测量值都转换为屏幕上的绝对像素。

然后，可以将这些信息传递给最后一个阶段，将渲染树中的每个节点转换成屏幕上的实际像素。这一步通常称为“**绘制**”或“栅格化”。它涉及绘出文本、颜色、图像、边框和阴影。(**Paint**)

然后就是我们看到的网页了 



#### 重绘与重排

重绘replaint，屏幕的一部分重新绘制，不影响整体布局，比如某个CSS的背景色变了，但元素的几何尺寸和位置不变。

重排eflow： 意味着元件的几何尺寸变了，我们需要重新验证并计算渲染树。是渲染树的一部分或全部发生了变化



其中在官方关于[渲染性能](https://developers.google.com/web/fundamentals/performance/rendering)的文档中提到:

不管用JavaScript 、 CSS或是网络动画， 在实现视觉变化时，通常会改变浏览器的这几个阶段

1. **JS / CSS > 样式 > 布局 > 绘制 > 合成**

![http-5](/assets/http-5.png)

如果修改元素的“layout”属性，也就是改变了元素的几何属性（例如宽度、高度、左侧或顶部位置等），那么浏览器将必须检查所有其他元素，然后“**自动重排**”页面。任何受影响的部分都需要**重新绘制**，而且最终绘制的元素需进行合成。

2. **JS / CSS > 样式 > 绘制 > 合成**

![http-6](/assets/http-6.png)

如果修改“paint only”属性（例如背景图片、文字颜色或阴影等），即不会影响页面布局的属性，则浏览器会跳过布局，但仍将执行绘制。

3. **JS / CSS > 样式 > 合成**

![http-7](/assets/http-7.png)

如果更改一个既不要布局也不要绘制的属性，则浏览器将跳到只执行合成。

这个最后的版本开销最小，最适合于应用生命周期中的高压力点，例如动画或滚动。



#### 实践意义

1. 避免频繁使用style 而是采用修改class方式（减少style步骤的时间）

2. 用 `transform` 和 `opacity` 属性更改来实现动画 

   因为这能直接跳过 **Layout** 和 **Paint** 步骤 由合成器来单独处理 目前只有两个属性符合条件

   这有一个前提需要在动画所在的元素上加一个合成器层 也就是`will-change: transform`

   ```css
   .moving-element {
     transform: ... 
     will-change: transform;
   }
   // 旧版或不支持will-change的浏览器
   .moving-element {
     transform: translateZ(0);
   }
   ```

   至于其能优化原因 是因为合成器会调用GPU加速 具体见[官方文档](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)

3. 对于 resize、scroll 等进行防抖/节流处理。



**参考**

[输入url到页面发生了什么？](http://47.98.159.95/my_blog/blogs/browser/browser-render/001.html#%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82)

[一份面试答案](https://juejin.cn/post/6844904110471249934#heading-9)