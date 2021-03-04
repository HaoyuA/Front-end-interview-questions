### AJAX 

Asynchronous JavaScript and XML (异步的 JavaScript 和 XML)

AJAX 是与服务器交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下，对网页的某部分进行更新。



> 如果要让用户留在当前页面中，同时发出新的HTTP请求，就必须用JavaScript发送这个新请求，接收到数据后，再用JavaScript更新页面，这样一来，用户就感觉自己仍然停留在当前页面，但是数据却可以不断地更新。
>
> 最早大规模使用AJAX的就是Gmail，Gmail的页面在首次加载后，剩下的所有数据都依赖于AJAX来更新。  [廖雪峰AJAX](https://www.liaoxuefeng.com/wiki/1022910821149312/1023022332902400#0)



### 实现AJAX

在现代浏览器上写AJAX主要依靠`XMLHttpRequest`对象：

分以下步骤：

1. 创建HttpRequest 对象 全称是XMLHttpRequest 对象

2. 调用对象的 open 方法

3. 监听对象的 onload && onerror 事件 

   一般现在会 监听onreadystatechange 事件

4. 调用对象的 send方法

### readystate

| 值   | 状态               | 描述                                                |
| ---- | ------------------ | --------------------------------------------------- |
| `0`  | `UNSENT`           | 代理被创建，但尚未调用 open() 方法。                |
| `1`  | `OPENED`           | `open()` 方法已经被调用。                           |
| `2`  | `HEADERS_RECEIVED` | `send()` 方法已经被调用，并且头部和状态已经可获得。 |
| `3`  | `LOADING`          | 下载中； `responseText` 属性已经包含部分数据。      |
| `4`  | `DONE`             | 下载操作已完成。                                    |



