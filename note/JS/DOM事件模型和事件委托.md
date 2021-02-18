外部引用：

​	[网道事件模型](https://wangdoc.com/javascript/events/model.html)

​    [JS事件模型 - SegmentFault 思否](https://www.baidu.com/link?url=svFjdJSwcnuzrbKCxHMREj7UIfoVYPxF4hCbZwG5DpxbJaKsoistvUyqoGibWgm_GYzTm3wrvEvTyb1kBNGCea&wd=&eqid=d54e378b000241c900000004602e17c8)

### 事件模型

   所谓事件应该就是浏览器与文档交互的瞬间 比如点击按钮 填写表格等 

   那么浏览器的事件模型，就是通过监听函数（listener）对事件做出反应。事件发生后，浏览器监听到了这个事件，就会执行对应的监听函数。



#### 绑定监听函数

 JavaScript 有三种方法，可以为事件绑定监听函数。

1. **HTML 的 on- 属性**

HTML 语言允许在元素的属性中，直接定义某些事件的监听代码。

```HTML
<body onload="doSomething()">
<div onclick="console.log('触发事件')">
```

上面代码为`body`节点的`load`事件、`div`节点的`click`事件，指定了监听代码。一旦事件发生，就会执行这段代码。

元素的事件监听属性，都是`on`加上事件名，比如`onload`就是`on + load`，表示`load`事件的监听代码。

注意，这些属性的值是将会执行的代码，而不是一个函数。

使用这个方法指定的监听代码，只会在冒泡阶段触发。

```html
<div onclick="console.log(2)">
  <button onclick="console.log(1)">点击</button>
</div>
```

上面代码中，`<button>`是`<div>`的子元素。`<button>`的`click`事件，也会触发`<div>`的`click`事件。由于`on-`属性的监听代码，只在==冒泡阶段==触发，所以点击结果是==先输出1，再输出2==，即事件从子元素开始冒泡到父元素。

2. **元素节点的事件属性**

元素节点对象的事件属性，同样可以指定监听函数。

```javascript
window.onload = doSomething;

div.onclick = function (event) {
  console.log('触发事件');
};
```

同样只在冒泡阶段触发

3. **EventTarget.addEventListener()**

所有 DOM 节点实例都有`addEventListener`方法，用来为该节点定义事件的监听函数。

```javascript
window.addEventListener('load', doSomething, false);
```

`EventTarget.addEventListener()`用于在当前节点或对象上，定义一个特定事件的监听函数。一旦这个事件发生，就会执行监听函数。该方法没有返回值。

该方法接受三个参数。

- `type`：事件名称，大小写敏感。
- `listener`：监听函数。事件发生时，会调用该监听函数。
- `useCapture`：布尔值，表示监听函数是否在捕获阶段（capture）触发（参见后文《事件的传播》部分），默认为`false`（监听函数只在==冒泡==阶段被触发）。该参数可选。



==this的指向==

监听函数内部的`this`指向触发事件的那个==元素节点==。

```
<button id="btn" onclick="console.log(this.id)">点击</button>
```

执行上面代码，点击后会输出`btn`。



那么绑定了监听函数 怎么去判断执行的先后顺序呢？



#### 事件的传播

一个事件发生后，会在子元素和父元素之间传播（propagation）。这种传播分成三个阶段。

- **第一阶段**：从`window`对象传导到目标节点（上层传到底层），称为“==捕获阶段==”（capture phase）。
- **第二阶段**：在目标节点上触发，称为“目标阶段”（target phase）。
- **第三阶段**：从目标节点传导回`window`对象（从底层传回上层），称为“==冒泡阶段==”（bubbling phase）。

通俗的来讲就是

先从**爷爷=>爸爸=>儿子** 顺序看有没有函数监听

后按**儿子=>爸爸=>爷爷** 顺序看有没有函数监听 

有监听函数就调用，并提供事件信息，没有就跳过

从外到内叫事件捕获

从内到外叫事件冒泡

> **注解**: 为什么我们要弄清楚捕捉和冒泡呢？那是因为，在过去糟糕的日子里，浏览器的兼容性比现在要小得多，Netscape（网景）只使用事件捕获，而Internet Explorer只使用事件冒泡。当W3C决定尝试规范这些行为并达成共识时，他们最终得到了包括这两种情况（捕捉和冒泡）的系统，最终被应用在现在浏览器里。

![js-2](assets/js-2.png)

[样例](http://js.jirengu.com/zirozujedu/1/edit?html,js,output)



那么捕获是不是就一定比冒泡先发生呢？ 不一定

==特例==

如果一个元素满足了下面条件可能冒泡比捕获先执行

* 只有一个div被监听(不考虑父子同时被监听)
* fn分别在捕获阶段和冒泡阶段被监听
* 用户点击的元素就是开发行者监听的元素

在这种情况下那个先监听就那个先执行

[例子](http://js.jirengu.com/tiket/10/edit)



#### e.currentTarget vs e.target vs this

在绑定的监听函数中：

e.target 是用户操作的元素

e.currentTarget 是程序员监听的元素 也就是绑定这个事件处理函数的元素

this 是绑定这个事件处理函数的元素

==结论==

所以 监听函数内 this === e.currentTarget

但是 e.target ==不一定== === e.currentTarget 一般是 currentTarget 的后代元素



### 事件委托(事件代理)



冒泡还允许我们可以利用事件委托  事件委托在某些场景中可以优化代码 比如如果你想要在大量子元素中单击任何一个都可以运行一段代码，或者是要监听动态元素（要监听的元素还没创建）



一个很好地例子是监听一个列表的每一项 你只需要吧监听器挂在父元素 ul 上 就可以在冒泡中捕捉到子元素的事件 用 e.target 可以获取到子元素



```html
<ul id="parent-list">
    <li id="post-1">Item 1</li>
    <li id="post-2">Item 2</li>
    <li id="post-3">Item 3</li>
    <li id="post-4">Item 4</li>
    <li id="post-5">Item 5</li>
    <li id="post-6">Item 6</li>
</ul>
```



```javascript
document.getElementById("parent-list").addEventListener("click", function(e) {
    // e.target is the clicked element!
    // If it was a list item
    if(e.target && e.target.nodeName == "LI") {
        // List item found!  Output the ID!
        console.log("List item ", e.target.id.replace("post-", ""), " was clicked!");
    }
});
```



**事件委托封装**

要求：

写出一个函数 on('click','#test','li',fn)

当用户点击 #test 里的 li 元素时 调用fn 函数

要求用到事件委托

```javascript
window.on = function(eventType, element, selector, fn) {
    element.addEventListener(eventType, e => {
      let el = e.target
      while (!el.matches(selector)) {  //判断 e.target 是不是匹配 element 
        if (element === el) {   // 如果找到爸爸这一层了 就停止 中断循环
          el = null
          break
        }
        el = el.parentNode // 当前层没有就去父辈元素找
      }
      el && fn.call(el, e, el) // 如果找到了就执行fn this就是当前的元素
    })
    return element
  }
```