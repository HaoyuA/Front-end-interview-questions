### Cookie

Cookie 是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向**同一服务器**再发起请求时被携带并发送到服务器上。

Cookie 最开始设计出来 是为了弥补 HTTP 协议 **无状态**的这一缺陷的

为什么说无状态是缺陷呢？

因为这严重影响了可交互式Web 应用程序的实现，打个比方我在某购物网站买了一件衣服和一条裤子， 在最后结账时 由于HTTP 的无状态性，如果不通过额外手段，服务器压根就不知道我买了什么

这时就需要Cookie来帮忙了 在我向服务器发请求的时候 还发送了一段 Cookie 里面包含了商品的信息

服务器就知道我买了什么了 这样不就解决了HTTP 无状态的缺陷了吗 

Cookie还有一种常见的**使用场景**就是 当登陆一个网站的时候， 网站会要求用户输入用户名密码，下面一般还有一个 **下次自动登录的**选项，用户勾选了下次在访问同一网站时就会自动登录，这是因为用户在第一次登陆时服务器已经将带有登陆凭据（用户名加密码的某种加密形式）的Cookie 发送到了用户的硬盘上 第二次登陆 如果该Cookie未过期 那么浏览器就会发送该Cookie，服务验证过后就会让用户登录了



#### set-cookie

服务器一般会使用 响应头的 Set-Cookie 字段来发送cookie信息

```javascript
// Node.js
response.setHeader("Set-Cookie", "<cookie名>=<cookie值>; Max-Age=<non-zero-digit>; Secure " );
```

Cookie 有一些特殊的属性需要注意 

有效期（`Max-Age`） 帮助判断Cookie 是否过期

`HttpOnly` 属性 无值 

 --  JavaScript `Document.cookie` API 无法访问带有 `HttpOnly` 属性的cookie 此类 Cookie 仅作用于服务器

`Secure` 的 Cookie 只应通过被 HTTPS 协议加密过的请求发送给服务端

`SameSite` Cookie 允许服务器要求某个 cookie 在跨站请求时不会被发送

```js
Set-Cookie: key=value; SameSite=Strict
```

`SameSite` 有下面三种值

- **`None`**。浏览器会在同站请求、跨站请求下继续发送 cookies，不区分大小写。
- **`Strict`。**浏览器将只在访问**相同站点**时发送 cookie。（在原有 Cookies 的限制条件上的加强，如上文 “Cookie 的作用域” 所述）
- **`Lax`。**与 **`Strict`** 类似，但用户从外部站点导航至URL时（例如通过链接）除外。 在新版本浏览器中，为默认选项，Same-site cookies 将会为一些跨站子请求保留，如图片加载或者 frames 的调用，但只有当用户从外部站点导航到URL时才会发送。如 link 链接

**SameSite = lax**

看到一个[例子](https://web.dev/samesite-cookies-explained/#what-are-first-party-and-third-party-cookies)比较形象的解释了 lax 和 strict 假设张三写了一篇博客 其中包含一张很amazing的🐱的图片 图片托管于`/blog/img/amazing-cat.png`   有一天李四看到了觉得这太amazing了 于是直接在他的博客上使用了它，并提供指向张三原始文章的链接。

后来王五访问了我的博客并拿了 一个 `promo_shown` 的 cookie 那么当他 每次去看李四的博客并看这张 amazing cat 的照片 请求不是就一定会带上这个 cookie吗？  这个 `promo_shown` 的cookie  在访问李四的博客时候压根一点用都没有 那么为什么要加进请求头呢

这时 如果将`promo_shown` cookie设置如下：

```js
Set-Cookie: promo_shown=1; SameSite=Lax
```

那么王五在访问李四的博客时 浏览器请求 amazing-cat  **将不会发送cookie**  但是如果王五点了指向张三原文章的链接 该请求**会包含cookie**  lax 在某种程度上可以提高网页加载性能

如果将`promo_shown` cookie改成 `strict`：

```text
Set-Cookie: promo_shown=1; SameSite=Strict
```

那么当别人通过外部🔗 比如邮件中或者是李四网站中指向张三的链接 **初始请求中不会发送**cookie

Strict 的场景 一般用于 更改密码 或者是 下单付款



#### Cookie缺点

1. 容量缺陷。Cookie 的体积上限只有**4KB**，只能用来存储少量的信息。

2. 同域名请求总是附带完整的 cookie 随着请求数的增多 或造成性能上的浪费 [Path](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#cookie_%E7%9A%84%E4%BD%9C%E7%94%A8%E5%9F%9F) 也许可以用来解决
3. 安全问题 [CSRF](https://developer.mozilla.org/en-US/docs/Glossary/CSRF)  [XSS](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting) 



### LocalStorage

`localStorage` 和cookie 相似也是用来储存当前域名下的数据 但需要注意的是 只有**同一个源**中的localStorage 才是相同的 

`localStorage`对于一个域名的数据是持久储存的 即便会话结束也不会清除

`localStorage` 一般以**键值对**的方式来储存 其中的键值对总是以**字符串**的形式存储。

语法:

```javascript
localStorage.setItem('myCat', 'Tom');
let cat = localStorage.getItem('myCat');
```



#### LocalStorage VS Cookie

1. 容量  [Chrome](https://en.wikipedia.org/wiki/Web_storage#cite_note-9) 的 LocalStorage 为 **5M** 完虐cookie 

2. 只存在于客户端 随读随取 能提升性能同时避免安全问题



### SessionStorage

session 就是会话的意思 因此`sessionStorage` 在会话结束后就会清空

接口的封装和 `localStorage` 一样



#### LocalStorage VS SessionStorage

`localStorage` 与 `sessionStorage` 一样 容量上限 储存方式 操作方式 都一样

但是后者者会话结束了就不存在了 而前者则是持久化储存

