> ==jQuery的基本设计思想和主要用法，就是**"选择某个网页元素，然后对其进行某种操作"**。==

理解：

提供一个函数 接收一个选择器 但是不返回elements 而是返回一个对象(jQuery 构造出来的对象) 这个对象中有一些方法比如 find end 等 这些方法会操作这些元素 (通过闭包 访问函数外部变量)

参考：

   [阮一峰jQuery设计思想](http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)

   [jQuery官方文档](https://www.jquery123.com/)



### jQuery 如何获取元素

* 用CSS选择器来获取



```javascript
　　$(document) //选择整个文档对象

　　$('#myId') //选择ID为myId的网页元素

　　$('div.myClass') // 选择class为myClass的div元素

　　$('input[name=first]') // 选择name属性等于first的input元素
```



* 用jQuery特有表达式来获取



```javascript
   $('a:first') //选择网页中第一个a元素

   $('tr:odd') //选择表格的奇数行

　　$('#myForm :input') // 选择表单中的input元素

　　$('div:visible') //选择可见的div元素

　　$('div:gt(2)') // 选择所有的div元素，除了前三个

　　$('div:animated') // 选择当前处于动画状态的div元素
```



### jQuery 的链式操作是怎样的

jQuery设计思想之三，就是最终选中网页元素以后，可以对它进行一系列操作，并且所有操作可以连接在一起，以链条的形式写出来，比如：

```javascript
　$('div').find('h3').eq(2).html('Hello');
```



分解开来，就是下面这样：

```javascript
　　$('div') //找到div元素

　　　.find('h3') //选择其中的h3元素

　　　.eq(2) //选择第3个h3元素

　　　.html('Hello'); //将它的内容改为Hello
```



这是jQuery最令人称道、最方便的特点。它的原理在于每一步的jQuery操作，返回的都是一个==jQuery对象==，所以不同操作可以连在一起。

jQuery还提供了[.end()](http://api.jquery.com/end/)方法，使得结果集可以后退一步：

```javascript
　　$('div')

　　　.find('h3')

　　　.eq(2)

　　　.html('Hello')

　　　.end() //退回到选中所有的h3元素的那一步

　　　.eq(0) //选中第一个h3元素

　　　.html('World'); //将它的内容改为World
```



### jQuery 如何创建元素

创建新元素的方法非常简单，只要把新元素直接传入jQuery的构造函数就行了：

```javascript
$('<p>Hello</p>');

$('<li class="new">new list item</li>');

$('ul').append('<li>list item</li>');
```



### jQuery 如何移动元素

假定我们选中了一个div元素，需要把它移动到p元素后面。

第一种方法是使用[.insertAfter()](http://api.jquery.com/insertAfter/)，把div元素移动p元素后面：

```javascript
$('div').insertAfter($('p'));
```

第二种方法是使用[.after()](http://api.jquery.com/after/)，把p元素加到div元素前面：

```javascript
$('p').after($('div'));
```

表面上看，这两种方法的效果是一样的，唯一的不同似乎只是操作视角的不同。但是实际上，它们有一个重大差别，那就是==返回的元素==不一样。第一种方法返回div元素，第二种方法返回p元素。

使用这种模式的操作方法，一共有四对：

[.insertAfter()](http://api.jquery.com/insertAfter/)和[.after()](http://api.jquery.com/after/)：在现存元素的外部，从后面插入元素

[.insertBefore()](http://api.jquery.com/insertBefore/)和[.before()](http://api.jquery.com/before)：在现存元素的外部，从前面插入元素

[.appendTo()](http://api.jquery.com/appendTo/)和[.append()](http://api.jquery.com/append)：在现存元素的内部，从后面插入元素

[.prependTo()](http://api.jquery.com/prependTo/)和[.prepend()](http://api.jquery.com/prepend)：在现存元素的内部，从前面插入元素



### jQuery 如何修改元素的属性

jQuery设计思想之四，就是使用同一个函数，来完成取值（getter）和赋值（setter），即"取值器"与"赋值器"合一。到底是取值还是赋值，由函数的参数决定。

```javascript
$('h1').html(); //html()没有参数，表示取出h1的值

$('h1').html('Hello'); //html()有参数Hello，表示对h1进行赋值
```



常见的取值和赋值函数如下：

　　[.html()](http://api.jquery.com/html/) 取出或设置html内容

　　[.text()](http://api.jquery.com/text/) 取出或设置text内容

　　[.attr()](http://api.jquery.com/attr/) 取出或设置某个属性的值

　　[.width()](http://api.jquery.com/width/) 取出或设置某个元素的宽度

　　[.height()](http://api.jquery.com/height/) 取出或设置某个元素的高度

　　[.val()](http://api.jquery.com/val/) 取出某个表单元素的值



### 手写 jQuery

https://github.com/HaoyuA/dom-2/blob/main/src/jquery.js