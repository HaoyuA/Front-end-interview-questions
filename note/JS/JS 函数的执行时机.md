## JS函数的调用时机

```javascript
let i = 0
for(i = 0; i<6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```

上述代码会打印出

 0、1、2、3、4、5 吗？ 并不是

而是 6 个 6

为什么

因为setTimeout()中的回调函数式最后才执行的 而此时for 的循环执行到了 6才停下， 而此时i 的作用在全局作用域中的 所以setTimeout才会被执行6次 也就是打印出 6个6

#### 那么如何打出0-5呢

方法1

把变量声明let i = 0 放在for 里面 形成自己的块级作用域



```javascript
for (let i = 0; i < 6; i++) {
    setTimeout(function () {
        console.log(i);
    }, 0)
}

```



方法2

用立即执行函数将局部变量i保存起来

```javascript
for (var i = 0; i < 6; i++) {
	(function (a) {
		setTimeout(function () {
        	console.log(a);
    	}, 0);
	}(i));  
}
```