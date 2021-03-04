### 异步

[网道同步异步](https://wangdoc.com/javascript/async/general.html#%E5%90%8C%E6%AD%A5%E4%BB%BB%E5%8A%A1%E5%92%8C%E5%BC%82%E6%AD%A5%E4%BB%BB%E5%8A%A1)

异步vs同步

如果能直接拿到结果

那就是同步

比如你在医院挂号 你拿到号才能离开窗口 

同步任务可能消耗10ms 也可能花3s

总之不拿到结果你是不会离开的



如果不能直接拿到结果

那就是异步

比如你在餐厅等位 你拿到号可以去逛街

什么时候才能真正吃饭呢

你10分钟可以去餐厅问题下(轮询)

你也可以扫码用微信接受通知(回调)





例子

request.send() 之后 并不能直接得到response 

必须得到readyState 为4 后 浏览器回头调用requestonreadystatechange函数

我们猜得到request.reponse

这和餐厅给你发微信提醒的过程是类似的



### 回调

写了不调用 而是给别人调用的函数 就是回调

把函数1给另一个函数2

```javascript
function f1(){}
function f2(fn){
  fn()
}
f2(f1)
```

灵魂自问：

1. 我调用f1了吗? 没有

2. 我把f1传给f2(别人了吗)？ 传了

3. f2调用f1了没？调用了

那么f1是不是写给f2调用的函数？ 是的

f1是回调



*arrary.forEach(n=> console.log(n)) ==同步回调==



#### Promise

为什么要用promise？

普通的回调函数 不规范名称五花八门  名称可以是success + error done+fail 等等

容易出现回调地狱 代码看不懂

很难进行错误处理



用法

```javascript
ajax = (method,url,options) => {
  const {sucess,fail} = options
  const request = new XMLHttpRequest()
  request.open(method,url)
  request.onreadystatechange = () => {
    if(request.readystate === 4){
      if(request.status < 400){
        sucess.call(null,request.response)
      }else if(request.status >= 400){
        fail.call(null,request,request.status)
      }
    }
  }
  request.send()
}
ajax.('GET','/xxx',{
  success(response){},fail(request,status)=>{}
})

// 改成promise

ajax('get','/xxx')
    .then((reponse) => {},(request) => {})  //then 的第一个参数成功 第二个失败 都只接收一个参数

// 需要把 ajax 改成 下面这样才能用then 方法
ajax = (method,url,options) => {
  return new Promise((resolve,reject) => {
    const {sucess,fail} = options
  	const request = new XMLHttpRequest()
  	request.open(method,url)
  	request.onreadystatechange = () => {
    	if(request.readystate === 4){
     	 if(request.status < 400){
       	 resolve.call(null,request.response)
       }else if(request.status >= 400){
      	 reject.call(null,request)
       }
     }
  }
  request.send()
  }) 
}
```

