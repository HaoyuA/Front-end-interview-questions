### CSS

**cascading style sheet**

层叠式样式表

样式语法:

选择器 {
	属性名: 属性值;
}



at语法

@charset "UTF-8";

@import url(2.css);

@media (min-width:100px) and (max-width: 200px){

样式语法

}

书写g

**引用：**

行内/内部/外部

> 内联样式 > 内部样式表 > 外部样式表

```HTML
<link rel="stylesheet" type="text/css" href="CSS/index.css">
  
<style type="text/css">

</style>

<div class="box" id="box" style="width: 100px; height: 100px;"></div>
```



### 浏览器渲染原理

步骤

* 根据HTML构建一个HTML树🌲(DOM)
* 根据CSS构建一个CSSDOM树（CSSDOM）
* 将两棵树合并成一颗渲染树(render tree)
* Layout 布局（文档流，盒模型，计算大小位置）
* paint绘制 （把边框颜色 文字颜色 阴影等画出来）
* Compose 合成 （根据层叠关系来展示画面）



![css-2](/assets/css-2.png) 

[渲染优化google文档](https://developers.google.com/web/fundamentals/performance/rendering)



### CSS动画

#### transition

transform + transition

transform属性可以跟：

> 只能转换由盒模型定位的元素。根据经验，如果元素具有`display: block`，则由盒模型定位元素。

位移translate

缩放scale 

旋转rotate

倾斜skew

```css
transform: translate(120px, 0); /* （x方向,y方向） 可以是 px 或者百分比 */

transform: scale(2, 0.5); /* （整体倍率）or（x,y） 数字不能写成百分比 */

transform: rotate(0.5turn); /* （x deg）转几度角 */

transform: skew(30deg); /* （x deg）倾斜几度角? */

transform: scale(0.5) translate(-100%, -100%); /* 组合 */

transform: none;  /* 取消所有 */
```



**`transition`** [CSS](https://developer.mozilla.org/en/CSS) 属性是 [`transition-property`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-property)，[`transition-duration`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-duration)，[`transition-timing-function`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-timing-function) 和 [`transition-delay`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-delay) 的一个[简写属性](https://developer.mozilla.org/en-US/docs/CSS/Shorthand_properties)。

```css
/* property name | duration */
transition: margin-right 4s;

/* property name | duration | delay */
transition: margin-right 4s 1s;

/* property name | duration | timing function */
transition: margin-right 4s ease-in-out;

/* property name | duration | timing function | delay */
transition: margin-right 4s ease-in-out 1s;
```

注意：并非==所有属性==都能过渡 

display:none到blcok就不行

需要用 visibility:hidden => visible

[跳动的心transition版]( http://js.jirengu.com/nufid/3/edit?html,css,js,output)



#### animation

keyframe 语法

```css
@keyframes slidein {
  from {
    transform: translateX(0%);
  }

  to {
    transform: translateX(100%);
  }
}

@keyframes identifier {
  0% { top: 0; left: 0; }
  30% { top: 50px; }
  68%, 72% { left: 50px; }
  100% { top: 100px; left: 100%; }
}
```

animation语法

animation:时长|过渡方式|延迟|次数|方式|填充模式|是否暂停|动画名

```css
/* 如果想让动画来回动direction就是alternate */
/* @keyframes duration | easing-function | delay |
iteration-count | direction | fill-mode | play-state | name */
animation: 3s ease-in 1s 2 reverse both paused slidein;

/* @keyframes name | duration | easing-function | delay */
animation: slidein 3s linear 1s;

/* @keyframes name | duration */
animation: slidein 3s;
```

[跳动的心animation版]( http://js.jirengu.com/kevuv/1/edit?html,css,js,output)



### 选择器

#### 分类

* 通配符选择器` *{} `
* 属性选择器 `[id = "box"]{} `
* 类选择器` .container{}`
* id 选择器 `#box {}`
* 并列选择器 `<p class="tip tip-success">tip success</p> .tip{} .tip.tip-success{} `  
* 派生/父子选择器 
* 伪类选择器 `.btn.btn.btn-warning:hover{}`

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    
        /* !important 权限最高 但是慎用  */
        *{
            margin: 0;
        }
        p {
            color: green !important;
        }

        #box {
            width: 100px;
            height: 100px;
            background-color: green;
            color: red;
        }
        .container{
            width: 200px;
            height: 200px;
            background-color: rosybrown;
        }

        h1{
            font-weight: normal ;
        }
  
        /* 属性选择器
        比如要选id = box 的 */
        [id = "box"]{
            width: auto;
        }
  
        /* 带href的都怎么怎么样 */
        [href]{
            
        }
    </style>

</head>

<body>
    <div></div>
    <p style="color: red">    
    </p>
    <!-- id 选择器 -->
    <div id="box" style="display: block;">color 设置的颜色</div>
    <!-- class 类选择器 -->
    <div class = "container"></div>
    <br>
    <div class = "container"></div>
    <!-- 标签选择器 一般用于初始化标签样式-->
    <h1>123</h1>
    <h2>123</h2>
  
</body>

</html>
```

##### 伪类选择器(伪元素）

[伪类伪元素](https://juejin.im/post/5ca22bae6fb9a05e1e523a5d)

###### 啥是伪类？

> CSS伪类是用来添加一些选择器的特殊效果。
> 伪类是基于元素的特征而不是他们的id、class、属性或者内容。由于状态的变化是非静态的，所以元素达到一个特定状态时，它可能得到一个伪类的样式，所以它是基于文档之外的抽象。

**:focus ** 聚焦效果

```html
<style type="text/css">
        /* ：disabled 禁用的伪类 */

        .checkbox{
            font-size: 16px;
            width: 40px;
            height: 40px;
            min-width: 40px;
            border: 1px solid #000;
            border-radius: 50%;
        }

        .checkbox label{
            display: block;
            width: 20px;
            height: 20px;
            margin: 10px;
            background-color: #000;
            border-radius: 50%;
            opacity: 0;


        }

        .checkbox input[type = "checkbox"]{
            display: none;

        }

        /* 相邻兄弟选择器：
         1. 同父级元素 
         2. 相邻 紧挨着
         3. 在其之后 */

        .checkbox input[type = "checkbox"]:checked + label{
            opacity: 1;
        }

        input{
            outline: none;

        }
        
        /* :focus */
        input:focus{
            border: 3px solid #eee;
        }

</style>
<body>
    <div class="checkbox">
        <input type="checkbox" id="checkbox"/>
        <label for="checkbox"></label>
     </div>
</body>
```

**:hover**  鼠标移上去的效果

```html
<style type="text/css">
        .btn{
            display: inline-block;
            width: 200px;
            height: 40px;
            
            border-width: 1px;
            
            border-radius: 4px;
            color: #fff;
            font-size: 14px;
            text-align: center;
            line-height: 40px;
            cursor: pointer;
            text-decoration: none;
            
        }
        .btn.btn-warning{
            background-color: #f0ad4e;
            border-color: #eea236;
        }

        /* 鼠标移上去的效果 */
        .btn.btn-warning:hover{
            background-color: #ec971f;
            border-color: #d58512;
        }   

</style>
<body>
    <a href="http:www.baidu.com" class="btn btn-warning" target="_blank">百度一下，你就知道</a>
</body>
```

**:disabled**  禁用的效果

```html
<style type="text/css">
   

</style>
<body>
    
</body>
```

**:last-child :first-child :nth-child**首个末个子元素选择器

```html
<style type="text/css">
   
        /* :last-child :first-child  首个末个子元素选择器*/
        p span:last-child{
            color: blue;
        }

        p span:first-child{
            color: red;
        }
</style>
<body>
     <P>
        <span>123</span>
        <span>123</span>
        <span>123</span>
        <span>123</span>
        <span>123</span>
    </P>
    

</body>
```

**table tr:nth-child**   tr 第几行 td 第几列 odd even 最常见

```html
<style type="text/css">
        table{
            width: 300px;
        }
        table tr td{
            border-bottom: 1px solid #ccc;
        }

         /* tr 第几行 td 第几列 odd even 最常见 */
        table tr:nth-child(odd){
            background-color: #ddd;
        }

        table tr:hover{
            background-color: #ddd;
        }

</style>
<body>
    <table>
        <tr>
            <td>1</td>
            <td>2</td>
            <td>3</td>
        </tr>
        <tr>
            <td>4</td>
            <td>5</td>
            <td>6</td>
        </tr>
        <tr>
            <td>7</td>
            <td>8</td>
            <td>9</td>
        </tr>
        <tr>
            <td>7</td>
            <td>8</td>
            <td>9</td>
        </tr>
    </table>
</body>
```

###### 啥是伪元素？

> 伪元素是创造DOM之外的对象
> 伪元素可以为一些在源文档中不存在的内容分配样式。
> 伪元素的内容实际上和普通DOM元素是相同的，但是它本身只是基于元素的抽象，并不存在于文档中，所以叫伪元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>伪类</title>
    <style type="text/css">
        //不能复制
        p::before{
            content: "我"
        }

        p::after{
            content: "真帅";
        }
        p::first-letter{
            color: orange;
            
        }

        span::before{
            content: "";
            display: inline-block;
            width: 100px;
            height: 100px;
            background-color: red;
        }

        span::after{
            content: "";
            display: inline-block;
            width: 100px;
            height: 100px;
            background-color: orange;
        }

        .welcome::before{
            content: url(img/盒子.png);
            /* content:attr(username); 传数据 比如欢迎user */
            width: 15px;
            height: 16px;
        }
    </style>
</head>
<body>
    <!-- 伪类
         :hover
         :nth-child -->

    <!-- 伪元素
         ::before
         ::after 
         一定要加content
         内联元素
         可以用display：inline-block改
         如果都是绝对定位 before在下层 after 在上层
         -->
    <p>很牛X，</p>

    <span>很牛X，</span>

    <p username = "" class = "welcome">欢迎</p>
</body>
</html>
```

![伪元素](/assets/css-3.png)

##### 并列选择器

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>并列选择器</title>
    <style type="text/css">
        .box{
            width: 100px;
            height: 100px;
        }

        .big-box{
            width: 200px;
            height: 200px;
        }

        .box.box1{
            background-color: red;
        }

        .box.box2{
            background-color: rosybrown;
        }

        .big-box.box1{
            background-color: blue;

        }

        .big-box.box2{
            background-color: purple;
        }

        .tip{
            font-weight: bold;
        }

        .tip.tip-success{
            color: green;
        }

        .tip.tip-warning{
            color: yellow;
        }

        .tip.tip-danger{
            color: red;
        }

        input,textarea{
    
            outline: none;
        }

        strong em{
            color: blue;
        }

    </style>
</head>
<body>
    <div class="box box1"></div>
    <div class="box box2"></div>
    <div class="big-box box1"></div>
    <div class="big-box box2"></div>

    文本提示样式类
    1 success 成功的提示 green
    2 warning 警告的提示 orange
    3 danger 失败的提示 red
    <p class="tip tip-success">tip success</p>
    <p class="tip tip-warning">tip warning</p>
    <p class="tip tip-danger">tip danger</p>

    <input type="text">
<br>
    <textarea id="" cols="30" rows="10"></textarea>
     
    浏览器对父子选择器的匹配规则
    从右到左 从下到上
</body>
```

##### 派生/父子选择器

* 标签+标签
* 标签+class 反之亦然
* 标签 + id等等

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" type="text/css" href="CSS/index.css">
    <!-- 文本样式表 -->
    <style type="text/css">
        strong em{
            color: green;
        }
        strong .text1{
            color: red;
        }
        div span{
            color: red;
        }
    </style>

</head>

<body>
    <p>派生选择器/父子选择器</p>
    标签+标签
    <strong>
        <em>你好 zhy</em>
    </strong>
     
    <p>
        <em>你好 zhy</em>
    </p>

    标签+class 反之亦然
    <strong>
        <em class="text">你好 zhy</em>
    </strong>
    <strong>
        <em class="text1">你好 zhy</em>
    </strong>
    id + id 不对
    <!-- div span {} 也能找到 越级关系 -->
    <div>
        <h1>你好，
            <span>zhy</span>
        </h1>
    </div>
</body>
```

* 如何算权重

    256进制
    
    数学 正无穷 = 正无穷+1
    计算机  正无穷 < 正无穷 +1


#### 选择器之间优先级

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" type="text/css" href="CSS/index.css">
    <!-- 文本样式表 -->
    <style type="text/css">
        div {
            background-color: blue;
        }

        .box {
            background-color: red;
        }

        #box {
            background-color: yellow;
        }

        [class="box"] {
            background-color: violet;
        }
    </style>

</head>

<body>
    
    !important >id > 属性|类选择器 > 标签选择器 > 通用选择器
    <div class="box" id="box" style="width: 100px; height: 100px;"></div>
</body>

</html>
    CSS          权重 
    *            0
    标签，伪元素   1
    类 属性 伪类   10
    id           100
    内联          1000
    ！important   正无穷
```



### 文档流

从左到右 从上到下

行内元素 从左到右直到占满一行

块级元素 每个都占一行

内联元素 从左到右

 •流动方向

inline 元素从左到右，到达最右边才会换行

block 元素从上到下，每一个都另起一行

inline-block 也是从左到右

•宽度

inline 宽度为内部 inline 元素的和，不能用 width 指定

block 默认自动计算宽度，可用 width 指定

inline-block 结合前两者特点，可用 width

•高度

inline 高度由 line-height 间接确定，跟 height 无关

block 高度由内部文档流元素决定，可以设 height

inline-block 跟 block 类似，可以设置 height

#### 脱离文档流

float

position: absolute / fixed

### 盒模型

•分别是

content-box 内容盒 - 内容就是盒子的边界

border-box 边框盒 - 边框才是盒子的边界  （比较好用）

•公式

content-box width = 内容宽度

border-box width = 内容宽度 + padding + border

![css- 1](/assets/css-1.png)

### Margin 合并

•哪些情况会合并

父子 margin 合并

兄弟 margin 合并

•如何阻止合并

父子合并用 padding / border 挡住

父子合并用 overflow: hidden 挡住

父子合并用 display: flex，不知道为什么

兄弟合并是符合预期的

兄弟合并可以用 inline-block 消除

总之要一条一条死记

而且 CSS 的属性逐年增多，每年都可能有新的







### CSS中需要注意的属性

#### min-width/height max..



#### overflow

```html
<style type="text/css">
    p{
            width: 200px;
            min-height: 400px;
            max-height: 500px;
            background-color: green;
            /* 溢出隐藏 */
            overflow: hidden;
            /* 一直有滚动条 */
            overflow: scroll;
            /* 溢出才有滚动条 */
            overflow: auto;
        }
</style>
```

#### border

```html
<style type="text/css">
   div{
            background-color: transparent;
            width: 100;
            height: 60px;
            font-size: 40px;
            min-width: 1400px;
            max-width: 1600px;
            /* 宽度 样式 颜色 可视区域其实是本身宽加边框宽 */
            /* border: 1px solid #000; */
            /* border-top: 1px solid red;
            border-right: 3px solid blue;
            border-bottom: 5px solid green; */
            /* border-width: 1px; 上下左右 */
            /* border-width: 2px 10px; 上下  左右 */
            /* border-width: 5px 2px 10px; 上 左右 下  */
            /* border-width: 1px 2px 5px 10px; 上 右 下 左 顺时针 */
            /* style还有dashed double */
            border-style: solid;
            border-color: #000;
        }
</style>
```

#### font

```html
<style type="text/css">
   #font{
            /* 12/14/16 常用 */
           font-size: 16px;
           /* lighter normal bold bolder 100-900*/
           font-weight: lighter;
           /* 可以多个字体 防止字体不被兼容 有空格/中文字体要加引号*/
           font-family: Arial, Helvetica, sans-serif,"Times New Roman";
           color: red;
        }
    .text{
            width: 200px;
            height: 200px;
            min-width: 200px;
            font-size: 16px;
            text-align: center;
            border:1px solid #000;
        }
    .text2{
            width: 200px;
            min-width: 200px;
            line-height: 1.2em; 
            height: 22px; /*默认21 22 最好整数倍*/
            font-size: 16px;
            border: 1px solid #000;
            /* 文本溢流 */
            white-space: nowrap;  /*不换行*/
            overflow: hidden; 
            text-overflow: ellipsis;  /*隐藏部分省略号*/
        }
    span{
            display: inline-block;
            width: 100px;
            height: 100px;
            background-color: green;
        }

</style>
<body>
     <p id = "font">
         字体
     </p>
    <div class = "text2"><span>
         我非常想成为一个成功的web前端工程师
     </span></div>
</body>
```

#### position

static

absolute

relative

fixed

sticky



z-index

```html
<style type="text/css">
   
</style>
```

###阴影

```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style type="text/css">
      body {
        margin: 0;
      }

      .box1 {
        width: 300px;
        height: 150px;
        margin: 100px;
        background-color: orange;
        /* box-shadow: 水平位置(必) 垂直位置(必) 模糊距离 阴影尺寸 阴影颜色 阴影种类; */
        box-shadow: 20px 10px;
        /* 右下偏移 */
        box-shadow: 10px 10px 5px;
      }

      .box2 {
        width: 300px;
        height: 150px;
        margin: 100px;
        background-color: orange;
        /* box-shadow: 水平位置(必) 垂直位置(必) 模糊距离 阴影尺寸 阴影颜色 阴影种类; */
        box-shadow: 20px 10px;
        /* 阴影尺寸可以实现扩张功能 */
        box-shadow: 0 0 10px 10px;
      }

      .box3 {
        width: 300px;
        height: 150px;
        margin: 100px;
        background-color: orange;
        /* box-shadow: 水平位置(必) 垂直位置(必) 模糊距离 阴影尺寸 阴影颜色 阴影种类; */
        box-shadow: 20px 10px;
        /* inset 向内扩散 */
        box-shadow: 0 15px 10px 10px #f40 inset;
      }

      .header {
        position: relative;
        z-index: 1;
        width: 100%;
        height: 60px;
        background-color: orange;
      }

      .box {
        width: 200px;
        height: 100px;
        margin-left: 100px;
        background-color: yellow;
        box-shadow: 0 0 20px 10px;
        /* -webkit-box-shadow: ;
            -moz-box-shadow ;  
            -o-box-shadow */
      }

      .box4 {
        width: 200px;
        height: 100px;
        background-color: orange;
        /* 左右半圆设置成height一半 纯圆 宽高一样*/
        border-radius: 50px;
      }

      .box-picture {
        width: 200px;
        height: 100px;
        border: 1px solid #000;
        overflow: hidden;
      }

      img {
        width: 100%;
      }

      .backgroud-img {
        width: 300px;
        height: 300px;
        margin: 100px;
        border: 1px solid #000;
        background-image: url(img/1.png);
        /* 占盒子尺寸 可用百分比 */
        background-size: 50% 50%;
        /* repeat | no-repeat | repeat-x | repeat-y  */
        background-repeat: no-repeat;
        /* 在盒子中的位置 left bottom 若10px 20px 则是距左10距上20 */
        background-position: center center;
      }

      .banner {
        width: 100%;
        height: 500px;
        background-color: orange;
        background-image: url();
        /* background-size: 100% 100%; 这样图片会压缩 */
        /* cover 是 不管 盒子多大 图片多大 必须占满整个盒子 而 contain是 不管盒子多大必须显示全部图片 比例两者皆不变 */
        background-size: cover;
        background-position: center center;
        /* 背景图片不滚动 */
        background-attachment: fixed;
      }

      .logo {
        width: 142px;
        height: 58px;
        /* border: 1px solid #000; */
      }

      h1 {
        margin: 0;
      }

      /* 如果没加载出就是一个a标签 */
      .logo h1 .logo-hd {
        display: block;
        width: 142px;
        height: 0;
        padding-top: 58px;
        background: url(img/logo.png) no-repeat 0 0/142px 58px;
        overflow: hidden;
      }
    </style>
  </head>

  <body>
    <!-- 清除浮动 -->
    <div class="box1">box1</div>
    <div class="box2">box2</div>
    <div class="box3">box3</div>
    <!-- 阴影 -->
    <div class="header"></div>
    <div class="box">标题挡住阴影</div>
    <!-- 圆角 -->
    <div class="box4"></div>

    <!-- 容器里放图片 -->
    <div class="box-picture">
      <img src="img/1.png" alt="" />
    </div>

    <!-- background img-->
    <div class="backgroud-img"></div>

    <!-- 如何让 backgroud image 不随浏览器缩放而缩放 -->
    <div class="banner"></div>

    <!-- 预防css加载不出 -->
    <div class="logo">
      <h1>
        <a class="logo-hd" href="https://www.taobao.com">淘宝网</a>
      </h1>
    </div>
  </body>
</html>

```



### 其他

#### 统一样式(global.css)

```html
<style type="text/css">
/* 一些默认的样式初始化 放在global.css里 */
        h1,
        h2,
        h3,
        h4,
        h5,
        h6{
            font-weight: normal;
        }

        ul{
            padding: 0;
            margin: 0;
            list-style: none;
        }

        input,
        textarea,
        button{
            outline:none;
        }

        a{
            text-decoration: none;
        }

        em{
            font-style: normal;  /* 去斜体*/
          }
 </style>
```



#### 浮动

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>浮动</title>
    <style>
        img {
            
            float: left;
            width: 300px;
            border: 1px solid #000;
        }

        /* .box1{
            width: 100px;
            height: 100px;
            background-color: green;
        }
        .box2{
            width: 200px;
            height: 200px;
            background-color: orange;
        } */

        .box{
            margin-top: 20px;
            width: 200px;
            border: 10px solid #000;
        }

        .box .inner-box{
            float: left;
            width: 100px;
            height: 100px;  
        }

        .box .inner-box.box1{
             background-color:  orange; 
        }

        .box .inner-box.box2{
            background-color: orchid;
        }

        .clearfix{
            clear: both;
        }

        /* 最好的方法不多写一个clearfix 而是直接在要清除浮动的父元素上加clearfix */
        .clearfix::after {
            content: "";
            display: block;
            clear: both;
        }


        ul{
            padding: 0;
            margin: 0;
            list-style: none;
        }

        ul{
            width: 300px;
            margin-top: 20px;
            border: 1px solid #000;
        }

        ul li{
            float: left;
            width: 100px;
            height: 100px;
        }

        .header{
            width: 100%;
            height: 60px;
            background-color: #000;
        }

        .header .left{
            /* float: left; */
            width: 200px;
            height: 100%;
            background-color: green;
        }

        .header .right{
            float: right;
            width: 200px;
            height: 100%;
            background-color: orange;
        }

    </style>
</head>

<body>
    <!-- <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
    <span>原标题：特朗普动真格！宣布暂停资助世卫组织；美国确诊已超60万；印度确诊数还在激增 来源：人民日报等美国约翰斯·霍普金斯大学实时数据显示跌超1%。</span> -->
    <!-- <br>
    <div class="box1"></div>
    <div class="box2"></div> -->
    <!-- 浮动流 块级元素无法识别浮动流 -->
    <!-- 如何清除浮动  clear:both 用伪类-->
    <!-- 一个元素加了浮动后会变成内联块级元素 -->

    <div class="box">
        <div class="inner-box box1"></div>
        <div class="inner-box box2"></div>
        <!-- <span>加了clearfix 没用 因为清除浮动借用元素必须是块元素 -->
        <!-- <span class="clearfix"></span> -->
        <p class="clearfix"></p> 
        <!-- <p></p> 其实不好因为加了一个新元素 最好是在伪元素上加-->
    </div>

    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <p class="clearfix"></p>
    </ul>

    <br>

        <!-- 左右浮动 -->

    <div class="header">
        <div class="left"></div>
        <div class="right"></div>
        <!-- 如果只加 .right{float：right} 左右会不在一行 因为左边没有float 所以还是块级元素 -->
    </div>

    <br>

    <!-- 如果是<span></span>就不需要左边加float:left 因为本身就是内联级元素 -->
    <div class="header">
        <span class="left">123</span>
        <span class="right">123</span>
    </div>

</body>

</html>
```

##### 清除浮动

```html
<style type="text/css">
         /* 最好的方法不多写一个clearfix 而是直接在要清除浮动的父元素上加clearfix */
        .clearfix::after {
            content: "";
            display: block;
            clear: both;
        }
        table {
            width: 300px;
            height: 300px;
            caption-side: bottom;
            /* 合并边框 变成一根线*/
            border-collapse: collapse;
            /* 不改变没列的宽度 默认automatic */
            table-layout: fixed;
        }

</style>
<body>
    <ul class="table1 clearfix">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</body>
```



#### 表格

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style type="text/css">
        table {
            width: 300px;
            height: 300px;
            caption-side: bottom;
            /* 合并边框 变成一根线*/
            border-collapse: collapse;
            /* 不改变没列的宽度 默认automatic */
            table-layout: fixed;
        }

        table tr td:nth-child(2) {
            text-align: center;
        }

        table tr:nth-child(even) {
            background-color: #eee;
        }

        ul {
            padding: 0;
            margin: 0;
            list-style: none;
        }

        .clearfix::after {
            content: "";
            display: block;
            clear: both;
        }

        .table1 {
            width: 300px;
        }

        .table1 li {
            float: left;
            width: 101px;
            height: 101px;
            /* 把中间重复边框去掉 */
            margin: -1px 0 0 -1px;
            border: 1px solid #000;
            box-sizing: border-box;
        }

        .box {
            width: 300px;
            overflow: hidden;
        }

        .box .table2 {
            /* 整体加宽2px 再加右下的边框 再向左偏移1px */
            width: 302px;
            margin-left: -1px;
            border-right: 1px solid #000;
            border-bottom: 1px solid #000;
        }

        .box .table2 li {
            float: left;
            width: 33.33%;
            height: 100px;
            /* 只加上右边边框 */
            border-top: 1px solid #000;
            border-right: 1px solid #000;
            box-sizing: border-box;
        }
    </style>
</head>

<body>
    <table border="1">
        <caption>表格标题</caption>
        <tr>
            <td>1</td>
            <td>2</td>
            <td>3</td>
        </tr>
        <tr>
            <td>4</td>
            <td>5</td>
            <td>6</td>
        </tr>
        <tr>
            <td>7</td>
            <td>8</td>
            <td>9</td>
        </tr>
        <tr>
            <td>1</td>
            <td>2</td>
            <td>3</td>
        </tr>
        <tr>
            <td>4</td>
            <td>5</td>
            <td>6</td>
        </tr>
        <tr>
            <td>7</td>
            <td>8</td>
            <td>9</td>
        </tr>
        <tr>
            <td>1</td>
            <td>2</td>
            <td>3</td>
        </tr>
        <tr>
            <td>4</td>
            <td>5</td>
            <td>6</td>
        </tr>
        <tr>
            <td>7</td>
            <td>8</td>
            <td>9</td>
        </tr>



    </table>
    <!-- 用ul li 模仿表格 -->
    <ul class="table1 clearfix">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>

    <br>

    <!-- 如何让一个盒子两边无边框 -->
    <div class="box">
        <ul class="table2 clearfix">
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
        </ul>
    </div>

</body>

</html>
```

![表格](/assets/css-4.png)

#### 聊天气泡

> 1. 浮动 + 伪元素 + 画三角形
> 2. 父元素相对定位 + 子元素绝对定位

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>气泡</title>
    <style type="text/css">

        .chat-right {
            width: 150px;
            height: 50px;
            background-color: #1AAD19;
            border-radius: 7px;
            float: right;

        }

        .chat-right::after {

            content: "";
            width: 0;
            height: 0;
            border: 5px solid #1AAD19;
            border-color: transparent transparent transparent #1AAD19;
            float: right;
            position: relative;
            top: 10px;
            right: -10px;

        }

        .chat-left {
            width: 150px;
            height: 50px;
            background-color: #1AAD19;
            border-radius: 7px;

            margin-top: 50px;
            margin-right: 50px;
        }

        .chat-left::after {
            content: "";
            width: 0;
            height: 0;
            border: 5px solid #1AAD19;
            border-color: transparent #1AAD19 transparent transparent;
            float: left;
            position: relative;
            top: 10px;
            left: -10px;

        }

        .chat {
            width: 150px;
            height: 50px;
            background-color: #1AAD19;
            border-radius: 7px;
            margin-top: 50px;
            position: relative;
        }

        .left-triangle {
            position: absolute;
            left: -10px;
            top: 10px;
            width: 0;
            height: 0;
            border: 5px solid #1AAD19;
            border-color: transparent #1AAD19 transparent transparent;
        }

        .right-triangle {
            position: absolute;
            right: -10px;
            top: 10px;
            width: 0;
            height: 0;
            border: 5px solid #1AAD19;
            border-color: transparent transparent transparent #1AAD19;
        }
    </style>
</head>

<body>
    <div class="chat-right"></div>
    <br />
    <div class="chat-left"></div>

    <div class="chat">
        <div class="left-triangle"></div>
    </div>

    <br />
    <div class="chat">
        <div class="right-triangle"></div>
    </div>

</body>

</html>
```

#### BFC

```html
 <!-- BFC block formatting context：
    BODY  
    float right left
    position absolute fixed
    diplay inline-block table-cell
    overflow hidden auto scroll-->
    <!-- 文档流
    浮动流
    绝对、相对定位 -->
    
    <!-- 可用BFC解决的问题： -->
    <!-- 1.margin 合并 -->

    <!-- 2.float造成父级元素高度坍塌 -->
    <!-- 也可以通过bfc解决 但是一般都避免 -->
    <!-- 3.margin塌陷 -->
    <!-- 4.浮动元素覆盖 -->
```

**浏览器**

组成成分

    内核：
    渲染引擎 rendering engine 一行行去打印出来 -> 样式兼容性
    JS 引擎 对网页动态化和特效有很大贡献 google V8


| shell         | 内核              |
| ------------- | ----------------- |
| Google chrome | blink（自己搞的） |
| Safari        | webkit            |
| IE            | trident           |
| firefox       | gecko             |