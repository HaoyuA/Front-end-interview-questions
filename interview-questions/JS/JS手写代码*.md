#### 手写call bind apply 

**call**

1. call接收第一个参数为this 如果不是对象的话就用包装类包装 如果传null的话this 指向window
2. call能把剩下参数传给函数
3. 可以将函数当做第一个参数对象的一个属性 然后执行该函数 再删除这个函数

```javascript
Function.prototype.myCall = function(context,...args){
  var context = context || window; // 看是不是空
  context.fn = this;// 函数调用的call  this 就指该函数
  let result = context.fn(...args);
  delete context.fn;
  return result
}
```

同理 **apply**

```javascript
Function.prototype.myApply = function(context,args){
  var context = context || window;
  context.fn = this;
  let result = context.fn(...args);
  delete context.fn;
  return result;
}
```



**bind**

要麻烦很多 原因就是它返回的是一个函数 这个函数在传参的时候和 原来bind除this后的**参数连起来**

```javascript
Function.prototype.myBind = function(context,...args){
  var self = this;
  var fbound = function(){
    // 假设fbound 是作为一个构造函数 那么这个this就指向的是fbound 的实例
    // 所以我们要先去判断 假设var obj = new fbound() 如果是那么这里传给apply的this就指向这个实例 
    // 否则还是指向绑定的this
    self.apply(this instanceof self ? this : context,args.concat(bindArgs))
  }
  fbound.prototype = this.prototype
  return fbound
}
```



#### 深浅拷贝

**浅拷贝**

如果是数组，实现浅拷贝，比可以`slice` ，`concat`返回一个新数组的特性来实现

如果是对象，遍历对象，然后把属性和属性值都放在一个新对象就OK了

```javascript
var shallowCopy = function(obj){
  // 只拷贝对象
  if(typeof obj !== 'object') return;
  // 根据obj的类型判断是新建一个数组还是对象
  var newObj = obj instanceof Array ? [] : {};
  // 遍历obj,并且判断是obj的属性才拷贝
  for(var key in obj){
    if(var key in obj){
      newObj[key] = obj[key];
    }
  }
  return newObj;
}
```



**深拷贝**

对象&数组都可以用 JSON.parse 和 JSON.stringfy 来实现

先判断一下属性值类型，如果是对象

```javascript
var deepCopy = function(obj){
  if(typeof obj !== 'object') return;
  var newObj = obj instanceof Array ? [] : {};
  for(var key in obj){
    if(obj.hasOwnProperty(key)){
      newObj[key] = typeof obj[key] === 'object' ? deepCopy(obj[key]) : obj[key];
    }
  }
  return newObj;
}
```







#### 事件委托



#### 防抖节流

**防抖 debounce** 顾名思义就是防止抖动 以免把一次事件误认为多次 **敲键盘**就是每天都会接触到的防抖操作 你细品 

我听到的一个理解是 一个外卖员 如果他送出外卖过一分钟又有一单外卖不是加这单一起送更划算？ 

那么于是外卖员就开始等 只要1分钟内有一单新的外卖 他就会加到自己这 然后再等一分钟 

直到一分钟内没有新外卖 他再出发



实际前端开发应用的场景有：

登陆 发短信等按钮为避免用户点击过快 以至于发送多次请求 需要防抖

文本编辑器实时保存 当无任何操作后一秒进行保存



代码的重点在于清零 **clearTimeout(timer)**

```javascript
function debounce(f,wait){
  let timer
  return (...args) => {
    if(timer) { clearTimeout(timer) }
    timer = setTimeout(()=>{
      f(...args)
    },wait)
  }
}
// 处理函数
function handle() {
    console.log(Math.random()); 
}
// 滚动事件
window.addEventListener('scroll', debounce(handle, 1000));
```

记忆点 送外卖



**节流 throttle** 就好比是**技能cd** 你在触发了这个回调后的 多少时间内都不能触发这个回调

```javascript
function throttle(f,wait){
  let timer
  return (...args)=>{
    if(timer){ return }
    timer = setTimeout(()=>{
      f(...args)
      timer = null
    },wait)
  }
}

// 处理函数
function handle() {
    console.log(Math.random()); 
}
// 滚动事件
window.addEventListener('scroll', throttle(handle, 1000));
```

记忆点 技能cd



#### AJAX

```javascript
var request = new XMLHttpRequest()
request.onreadystatechange = function(){
  if(request.readyState === 4){
    if(request.status === 200 ){
      console.log(request.responseText)
    }
  }else if(request.status >= 400){
    console.log('请求失败')
  }
}
request.open('GET','./xxx')
request.send()
```

 记忆点 readystate === 4



#### 继承

第一种不用class

```javascript
function Animal(color){
  this.color = color
}
Animal.prototype.move = function(){}

function Dog(color,name){
  Animal.apply(this,arguments)
  this.name = name
}
Dog.prototype = Object.create(Animal.prototype)
Dog.prototype.constructor = Dog
Dog.prototype.bark = function(){console.log('旺旺')}
```

记忆点 call 

Object.create

constructor



用class  extends

```javascript
class Animal{
  constructor(color){
    this.color = color
  }
  move(){}
}

class Dog extends Animal{
  constuctor(color,name){
    super(color)
    this.name = name
  }
  bark(){console.log('旺旺')}
}
```

记忆点  **extends** super 

