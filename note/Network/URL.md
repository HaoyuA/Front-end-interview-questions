## 浅析URL

李爵士发明了

WWW = URL  + HTTP + HTML



### IP

全称 Internet protocol  互联网协议

主要约定两件事

一、如何定位一台设备

二、 如何封装数据报文以及和其他设备交流

**如何获取==外网IP==**

* 只要路由器连接上电信的服务器 那么路由器就会有一个外网ip 比如 14.17.32.211 这就是你在互联网中的地址

* 但是如果重启路由器 那么可能被重新分配一个外网IP 所以一般来说路由器是没有固定的外网ip

手机和电脑的 IP怎么区分呢 用内网IP 

* 路由器会在你家里创建一个内网 内网中设备使用内网IP 一般来说这个IP 地址是192.168.xxx.xxx

* 路由器会给自己分配一个好记的内网IP 如192.168.1.1

* 然后会给每一个内网的设备分配一个不同的内网IP 如电脑是192.168.1.2 手机是192.168.1.3

==**特殊的IP**==

127.0.0.1 表示自己

localhost表示hosts指定为自己

0.0.0.0不表示任何设备



IP有了还需要什么？ ==端口==

port

一台机器可以提供不同服务：

1.HTTP服务最好使用80端口

2.HTTPS服务最好使用443端口

3.FTP服务最好使用21端口

一共有65535端口

端口使用规则：

* 0到1023 号端口是留给系统使用的

* 只有拥有管理员权限后才能使用1024端口

* 其他端口可以给普通用户使用

* 一般调试会用 比如http-sever 8080端口

* 如果一个端口被占用 需要换一个端口



### 域名

域名就是对IP的别称

baidu.com 对应什么IP

`ping baidu.com`

一个域名可以对应不同IP

这个叫做均衡负载 防止一台机器扛不住

一个IP可以对应不同域名

这个叫做共享主机 穷开发者会这么做



那么域名和IP是这么对应起来的？

通过DNS domain name system 域名系统



### DNS

它是怎么运作的呢

当你输入一个 域名后 比如 baidu.com

你的chrome 浏览器会向电信/联通 的dns 服务器询问baidu.com 对应什么IP （nslookup xxx.com）

2.电信联通会回答一个IP

3.然后chrome才会对相应IP的80/443端口发送请求

4.请求内容是查看baidu.com 的首页

为什么是80或443端口

服务器用默认80端口提供HTTP服务

服务器用默认443端口提供HTTPS服务

可以在开发者工具看到具体端口



域名分类：

- .com 是顶级域名
- xiedaimala.com 二级域名 （俗称一级域名）
- www.xiedaimala.com 三级域名（俗称二级）



URL（**U**niform **R**esource **L**ocator - 统一资源定位符）

url：协议 + 域名或IP + 端口号 + 路径  + 查询字符串 + 锚点

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1787194/1596361987003-df0a56ed-f1fb-4c16-90db-44e523f3e986.png)

基于 TCP 和 IP 两个协议



**如何请求不同的页面：**

路径是没有后缀的

eg：https://developer.mozilla.org/zh-CN/docs/Web/HTML



同一个页面不同内容

使用查询参数

eg: http://www.baidu.com/s?wd=hi

http://www.baidu.com/s?wd=hello



同一个内容，不同位置

使用锚点

[https://developer.mozilla.org/zh-CN/docs/Web/CSS#](https://developer.mozilla.org/zh-CN/docs/Web/CSS#参考书)参考书

[https://developer.mozilla.org/zh-CN/docs/Web/CSS#](https://developer.mozilla.org/zh-CN/docs/Web/CSS#参考书)教程



注意：

- 锚点看起来有中文，实际上不支持中文
- 锚点后内容 会被浏览器"吃掉" 只是下载在本地内容中显示那一部分的内容 是==不会发送给服务器==的 



### curl命令

用 curl 可以发HTTP请求

```
curl -v  [域名]
curl -s -v  [域名]
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1787194/1596411521468-afb6bf37-40b9-492e-8458-0fa0e83363db.png)

一些概念：

- url会被curl工具重写，先请求DNS获得IP
- 先进行TCP连接，TCP连接成功后，开始发送HTTP请求
- 响应结束后，关闭TCP连接

