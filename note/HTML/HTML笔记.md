# HTML笔记

[TOC]



## 快捷键

* **html:5  或者 ！然后tab 自动生成网页**
* **Ctrl + / 注释**
* **Ctrl + S 保存**
* **Ctrl + P 找文件**
* **Ctrl + Shift + P 输命令**  
* **Shift + Alt + F 对齐**
* **Ctrl + [  or ]   实现文本 向左或向右移动**
* **[emmet语法](https://github.com/HaoyuA/Front-end-interview-questions/blob/main/note/HTML/HTML%E7%AC%94%E8%AE%B0.md#emmet%E8%AF%AD%E6%B3%95)**

## HTML简介

### 历史

1990年诞生

Tim Berners-Lee 李爵士

做了啥？

* 写了第一个浏览器
* 写了第一个服务器
* 用自己浏览器访问了自己的服务器
* 发明了WWW 同时发明了HTML HTTP 和URL

### 起手式

```html
<!-- html 5 声明方式 CSS1Compat W3C标准兼容性模式 
    BackCompact 怪异兼容性模式 如果不加DOCTYPE document.compatMode-->
<!DOCTYPE HTML>
<!-- lang = "zh-CN" 简体中文 或 "EN" 英文 zh-Hans zh-CHS 纯简体中文 zh-Hant zh-CHS 繁体-->
<html>

<head>
    <!-- GB2312 - 中国信息处理国家标准码 -> 简体中文编码
 GBK 汉字扩展规范 -> 扩大汉字收录 增加繁体中文、少数民族文字
    UTF-8 万国码 几乎认识全部在用的字体 -->
    <!-- 防止乱码 -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  <!-- 禁用缩放 -->
    <!-- 优先级title> 描述> 关键字 -->
    <meta name = "description" content="描述"/>
    <meta name="keywords" content="关键字"/>
    <title>Document</title>
    <!-- title: 网站名称 + 主要关键字/关键词描述
         详情页： 详情名称 + 简介 + 网页名称
         列表页：分类 + 关键字 + 网站名称 
         文章页 标题 + 分类 + 网站名称  -->
</head>
```

## 表示文章/书籍

h1~h6 标题

section章节 

article 文章

p 段落 

header 页眉

footer 页脚

main 主要内容

aside 次要内容

div（division） 划分



## 全局属性

即为所有标签都有的属性

class 

contenteditable

用户可以直接编辑页面上的内容

hidden 

隐藏一个元素

id 

不一定是唯一的 可以用两个一样id的元素却不会报错 加一个名字 

1. 标识符 加css
2. 在js中获取元素 id = xxx 的元素可以通过 ```XXX.style.color = bule``` 的方式获取

style 

行内样式

tabindex

即用户访问页面按tab 访问的元素顺序

* 可以是正数，不必是连续的
* 可以是 0，表示最后才被 tab 访问
*  可以是 -1，表示不可被 tab 访问

title

标题

## 常用标签

[a](https://github.com/HaoyuA/Front-end-interview-questions/blob/main/note/HTML/HTML%E7%AC%94%E8%AE%B0.md#a)

[img](https://github.com/HaoyuA/Front-end-interview-questions/blob/main/note/HTML/HTML%E7%AC%94%E8%AE%B0.md#img)

[table](https://github.com/HaoyuA/Front-end-interview-questions/blob/main/note/HTML/HTML%E7%AC%94%E8%AE%B0.md#%E8%A1%A8%E6%A0%BCtable)

[form](https://github.com/HaoyuA/Front-end-interview-questions/blob/main/note/HTML/HTML%E7%AC%94%E8%AE%B0.md#%E8%A1%A8%E5%8D%95form)

## 标签

 **双标签/单标签**

```html
<p> </p>
```

应该闭合  因为html 标准

```html
<html> </html>  <!-- 根标签 -->
```

pre标签

可以保留 空格 tab 换行

### 内联元素

#### 语义化元素

```html
    <!-- 语义化标签 vs 物理性标签 一般用语义化标签 因为更好可读性 以及爬虫 -->
    <!-- 加粗 -->
    <strong>我是strong</strong>
    <br />
    <b>我是bold</b>
    <br />
    <!-- 斜体 -->
    <em>emphsize</em>
    <br />
    <i class="fa fa-star">italic</i>
    <br />
    <!-- 秒杀里吧价格划掉 -->
    <del>delete</del>
    <br />
    <!-- 下划线 文章里可以用 -->
    <ins>insert</ins>
    <br />
    <!-- 用P去做出delete&insert -->
    <p style="text-decoration: line-through;">模仿delete</p>
    <br />
    <p style="text-decoration: underline;">模仿insert</p>
```

#### &lt;a&gt;

常用属性

* href 超链接

  * 网址

    https://google.com

    http://google.com

    //google.com 会自动选择http 或https

  * 路径

    绝对路径或相对路径

  * 伪协议

    javascript:;

    mail:to邮件

    tel:手机

  * id

    href = #xxx 跳到某个叫xxxid 的位置

* target 指定在那个窗口打开  
  * target = _black 就是在**空白页**打开
  * _self 默认值 **当前页面**打开
  * _top **顶层页面**打开 iframe中 才会体现
  * _parent  **父级页面**打开 iframe中 才会体现
  * target = 'xxx'呢 找一个叫xxx的窗口打开 没有就新建一个 （```window.name``` =>xxx）
* rel =noopener ? 

```html
    <p>anchor 锚点标签
        超链接标签</p>
    <p>1.超链接标签</p>
    <!-- href: hypertext reference -->
    <a href="http://www.baidu.com">123</a>
    <p>2.打电话</p>
    <a href="tel：12312313"></a>
    <p>3.发邮件</p>
    <a href="mailto:123@qq.com"></a>
    <p>4.锚点定位</p>
    <a href="#h1">返回顶部</a>
    <p>5.协议限定符</p>
    <a href="javascript:;">打开弹窗</a>
```

#### &lt;label&gt;

见[form](#### 表单(form))

#### &lt;span&gt;

```html
    <!--span 内联元素 inline element -->
    <span>123</span>
    <p>
        123<span>
            123
        </span>123
    </p>
```



#### 其他(<sub&gt; <sup&gt;)

```html
    <!-- 很少用 却不得不用的标签
    sub sup 内联元素 inline element -->
    <!-- superscripted 脚注 可用于注解 次方 -->
    百度<sup><a href="https://www.baidu.com" target="_blank">[1]</a></sup>
    <br />
    5<sup>2</sup>=25
    <br />
    Na <sup>+</sup>
    <br />
    <!-- subscripted -->
    H<sub>2</sub>SO<sub>4</sub>
```

#### 多个内联 内联块元素之间有空格？

```html
<!-- 多个行内元素之间总是会有空格 因为一旦换行就会有文本分隔符 怎么办? 元素间不换行 -->
    <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
    <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
    <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
    <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
    <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
    <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
```

![捕获](C:\Users\asus\Desktop\前端笔记\捕获.PNG)

```html
<img src="https://www.baidu.com/img/bd_logo1.png" alt=""><img src="https://www.baidu.com/img/bd_logo1.png" alt=""><img src="https://www.baidu.com/img/bd_logo1.png" alt=""><img src="https://www.baidu.com/img/bd_logo1.png" alt=""><img src="https://www.baidu.com/img/bd_logo1.png" alt=""><img src="https://www.baidu.com/img/bd_logo1.png" alt="">
```



### 块级元素

#### 表单(form)

属性 

* action 目标地址

* method  就是请求的属性

  POST/GET

* autocomplete 自动填充
* target 同 a 标签

事件

 onsubmit 

```html
<input type = 'submit' value = '提交'>  或者
<button type = 'submit'> 提交</button>  两者区别是button 里可以加任何标签 input 不行
```

在多选题时 **name** **必须统一** 这样就只会选一个 value 去传值

##### text(disable/readonly)

```html
    <!-- 块级元素 block element
    url : 提交数据接收的地址 
    value 可以传的参数 或者可以预先设置成默认值
    -->
    <form method = "GET" action="">
    <!-- input 内联块级元素 inline-block element -->
    <input name = "user"  type="text" style="width: 400px; height: 100px;"/>123
    
    123
    <p>
        用户名：<input type="text" name="username" maxlength="5"/>
    </p>
    <p>
        密码： <inPut type="password" name = "password" id = "password1">       </inPut>
    </p>
    <p>
        <input type="submit" value = "登录"  id="">
    </p>
    </form>
    <form method = "GET" action="">
        <!-- 内联元素 label 的for属性与某一个input值相同时 点击label可以聚焦该input输入值 -->
        <label for="username">用户名</label>
        <input type="text" id="username" name = "name"><br>
        <label for="pass">用户名只读</label>
        <!-- disable 不可输入无法提交数据 readonly 不可输入 然而表单还是能提交这个数据 -->
        <input type="text" id="pass"  name = "pass" disabled readonly/>
        
    </form>
```

##### radio

```html
    <form method = "GET" action="">
        <!-- 在多选题时 name 必须统一 这样就只会选一个 value 去传值 -->
        <input type="radio" name="sex" id="male" value = "male" checked>
        <label for="male">男士</label>
        <input type="radio" name="sex" id="female" value="female">
        <label for="female">女士</label>
    </form>
```

##### checkbox

```html
     <form method = "GET" action="">
        <h3>你喜欢的编程语言</h3>
        <p>
            <input type="checkbox" name="favLang" id="Java" value="Java">
            <label for="Java">Java</label>
            <input type="checkbox" name="favLang" id="JS" value="JS">
            <label for="JS">JS</label>
            <input type="checkbox" name="favLang" id="Python" value="Python">
            <label for="Python">Python</label>
            <input type="checkbox" name="favLang" id="C" value="C">
            <label for="C">C</label><br>
            <input type="submit">
        </p>
    </form>
```

##### textarea(内联块 方便起见放这)

```html
    <form method = "GET" action="">
        <!-- 内联块级元素 placeholder 不方便改样式 所以一般不用 用js写 -->
        <!-- cols rows 决定长宽 8px*30 +17(scroll bar宽度) -->
        <!-- 注意：标签间不能换行空格 因为这些会被识别成textarea的内容 -->
        <textarea name="" id="" cols="30" rows="10"> 123123</textarea><br>
        <input type="submit">
    </form>
```



##### select(内联块 方便起见放这)

```html
    <form method = "GET" action="">
        <!-- 提交值 如果不加value值 则会默认为标签内文本 加了就只看value -->
        <select name="lang" id="">
            <option value="">请选择</option>
            <option value="JavaScript">JavaScript</option>
            <option value="JS">JS</option>
            <option value="Python">Python</option>
            <option value="C">C</option>
        </select><br>
        <input type="submit" name="" id="">
    </form>
```

##### fieldset legend(内联块 方便起见放这)

```html
   <form method = "GET" action="">
        <!-- 块级元素 -->
        <fieldset>
            <legend>用户登录</legend>
            <p>
                <label for="name1">用户名</label>
                <input type="text" name="" id="name1">
            </p>    
            <p>
                <label for="pass1">密码</label>
                <input type="text" name="" id="pass1">
            </p>
            <input type="submit">
        </fieldset>
    </form>
    <form action="" style="background-color: green;">123</form> 123
```

##### “请输入关键字”搜索框

```html
    <form method = "GET" action="">
        <input type="text" value = '请输入关键字' onfocus="focusiInput(this)" onblur="blurInput(this)">
        
    </form>
    <script>
        function focusiInput(obj){
            if(obj.value === '请输入关键字'){
                obj.value = '';
            }
        }
        function blurInput(obj){
            if(obj.value === ''){
                obj.value = '请输入关键字';
            }
        }
    </script>
```



#### 标题(h1, h2,h3..)

```html
<!-- heading 标题标签 h1 的 font-size：2em 2em这里意思是16px*2 em 可以在 font-size 设置 -->
    <h1 id = "h1">h1</h1>
    <h2>h2</h2>
    <h3>h3</h3>
```

#### 列表(ul ol li)

```html
 <style type="text/css">
        ul {

            padding: 0;
            margin: 0;
            list-style: none;
        }

        a {
            text-decoration: none;
        }

        .wrapper {
            width: 100%;
            height: 66px;
            box-shadow: 0 0 5px #999;
        }

        .wrapper ul {
            height: 100%;
        }

        .wrapper ul li {
            float: left;
            height: 100%;
            line-height: 66px;
        }

        .wrapper ul li a {
            display: block;
            height: 100%;
            font-size: 18px;
            padding: 0 15px;
            color: #666;
        }
    </style>
<!-- 块级元素 block element 序列列表ol 有序列表 ul 无序列表 li dl definition list 定义列表 -->
    <ol type="a">
        <li>HTML</li>
        <li>CSS</li>
        <li>javaScript</li>
        <li>JAVA</li>
        <li>Python</li>
    </ol>
    <!-- 超出26个怎么办？ 27 个aa -->
    <!-- start只有数字才行 字母不行 会减到复数-->
    <ol type="1" start="4" reversed="reversed">
        <li>HTML</li>
        <li>CSS</li>
        <li>javaScript</li>
        <li>JAVA</li>
        <li>Python</li>
    </ol>
    <!-- type disk/square/circle -->
    <div class="wrapper">
        <ul>
            <li><a href="javascript:;">前端</a></li>
            <li><a href="javascript:;">实用英语</a></li>
            <li><a href="javascript:;">英语口语</a></li>
            <li><a href="javascript:;">日语</a></li>
        </ul>
    </div>
```

#### 表格(table)

头身尾 thead tbody tfoot

tbody  > tr/th/td  > td 

table-layout: auto 宽度由内容决定 /fixed 某一列的宽度仅由该列首行的单元格决定	

```html
    <!-- 虽然现在不常用 会用来布局 -->
    <!-- caption 标题标签
         tr table row 
         th table heading cell 
         td table data cell
         cell padding 单元格内边距 -->
    <!-- align="left|right|center" -->
    <!-- thead:
         tfoot:
         tbody: 加载顺序 头 尾 身 语义化好-->

    <table border="1" cellpadding="10px">
        <caption>
            表题
        </caption>
        <thead>
            <tr>
                <th>ID</th>
                <th>名字</th>
                <th> 邮箱</th>
                <th>备注</th>
            </tr>

        </thead>
        <tbody align="center">
            <tr>
                <td>1</td>
                <td>zhy</td>
                <td>123@q.com</td>
                <td>半藏</td>
            </tr>
            <tr>
                <td>2</td>
                <td>samp</td>
                <td>1123@q.com</td>
                <td>源氏</td>
            </tr>
            <tr>
                <td>3</td>
                <td>samp</td>
                <td colspan="2">1123@q.com</td>
                <!-- 同理 rowspan -->
            </tr>
        </tbody>
        <tfoot>
            <tr>

                <td colspan="4">1123@q.com</td>
                <!-- 同理 rowspan -->
            </tr>
        </tfoot>
    </table>
```



#### fieldset legend

见[form](#### 表单(form))

#### 其他(div/p/address/dl dt dd) 

##### div

```html
    <!-- 容器 盒子 有宽高 结构化标签 因此是布局用的 -->
    <div style="width: 200px; height: 200px; border: 1px solid #000">
        我是一名前端工程师我是一名前端工程师我是一名前端工程师我是一名前端工程师我是一名前端工程师我是一名前端工程师我是一名前端工程师
    </div>
    <!-- 文本会溢出因为浏览器认为这是一个单词 需要在字间加空格 -->
    <div style="width: 200px; height: 200px; border: 1px solid #000">
        asdasdasdjaishfuefeiaugaisdasuhd;qwe;qwerq;wrqerq'qweqwe;qwe;q;we
    </div>
    <!-- 不论换行还是空格 都会被识别为文本分隔符 所以任然只空一格 -->
    <div style="width: 200px; height: 200px; border: 1px solid #000">
        I
        am a front end developer I am a front end developer
    </div>
```

##### p 

```html
    <!-- paragraph 段落标签  -->
    <p></p>
    <div style="width: 200px; height: 200px; border: 1px solid #000">
        <!-- 首行缩进2个 -->
        <p style="text-indent: 2em;">我是一名前端工程师
            我是一名前端工程师我是一名前端工程师
            我是一名前端工程师我是一名前端工程师</p>
    </div>
    <!-- 不对 < > 显示不出 -->
    <p>我在学习<div>标签</p>
    <!-- 用html实体符来表示 -->
    <p>我在学习&lt;div&gt;标签&nbsp;</p>
    <!-- 分割线 一般不用 渲染效果不一样-->
    <br />
    <hr />
```

##### adress

##### dl dt dd

```html
<!-- dl 定义列表 defination list dt term dd description -->
    <dl>
        <dt>
        <dd>
            描述1
        </dd>
        <dd>
            描述2
        </dd>
        </dt>
    </dl>
```



### 内联块级元素

#### &lt;img&gt;

src (source)

地址/相对/绝对路径

alt (alteranative)

图片加载失败显示的内容

onload 

```javascript
xxx.onload = function (){

console.log('图片加载成功');
}；
```

onerror

```  javascript
xxx.onerror = function (){

console.log('图片加载失败');
xxx.src = '/404.png'; // 可以作为图片加载失败的措施
}
```

响应式

```html
<style>
  *{
    margin:0;
    padding:0;
      box-sizing:border-box;
  }
  img {
    max-width:100%;
  }
</style>
```



```html
    <!-- 内联块级元素 inline-block element
    不独占一行 可以定义宽高 -->
    alt若图片加载不出可以知道它关于什么
    <img id = "xxx" src="1" alt="1" style="width: 200px; height: 200px;" /> 123
    <!-- 123不换行 因此不独占一行-->
```



#### &lt;iframe&gt; vs frameset

```html
<frameset rows = "10%, 90%">
    <frame src = "top.html">
        <frameset cols = "20%, 80%">
            <frame src = "left.html">
                <frame name = "mainFrame" src = "http://www.taobao.com">
        </frameset>
</frameset>
    <!-- frame 缺点
    1 动态化很难处理 因为本身是两个html 如果要通过js j控制页面效果交互 很难做好
    2 会增加http请求数量 加载慢
    3 对搜索引擎 爬虫 不友好 全是引用的
    4 没body 里的结构  -->
    <!-- iframe 内联块级元素 内联框架
    scrolling no|yes|auto -->
    <p>
        <a href="http://www.taobao.com" target="mainFrame">淘宝</a>
    </p>
    <p>
        <a href="http://www.jd.com" target="mainFrame">京东</a>
    </p>
    <iframe name="mainFrame" src="http://www.taobao.com" width="100%" height="1000" frameborder="1"
        scrolling="yes"></iframe>
    <!-- 减少http请求 功能性导航
    缺点：对搜素引擎/爬虫 不友好 滚动条体系混乱 很难监控到数据加载状态 -->

```



#### <input&gt;

见[form](#### 表单(form))

#### &lt;textarea&gt;

见[form](##### textarea(内联块 方便起见放这))

#### 其他(select)

见[form](##### select(内联块 方便起见放这))

### 区别？区分？

* 内联元素 行间元素 行内元素 inline element

  **不独占一行、无法定义宽高**

  定义了宽高也没用 

  ```html
  <strong style="width: 200px; background-color: slategray;">123</strong>
  ```
  
  <strong style="width: 200px; background-color: slategray;">123</strong>

> em del ins

* 块级元素 block element

  独占一行 可以定义宽高

> p div address

*  内联块级元素 inline-block elemet

  不独占一行 可以定义宽高

> image iframe

## h5新元素

声明方法变了

```html
<!DOCTYPE html>

<meta charset = "UTF-8">
```

删除了一些**标签**：

```html
<center></center> <font></font> <big></big> <small></small> 
<frameset></frameset> <frame></frame>
```

增加了一些**音视频标签**:

```html
<video> <!-- 代替flash 安全因素 性能优化 -->
<audio>
```

增加了一些**结构化标签**:

```html
<header> <footer> <section> <nav> <aside>
```

**JavaScript API**:

地理定位 离线储存 canvas  应用缓存 拖拽

**CSS3**

过渡 转换 动画

html5 技术可以干什么

游戏 单页面应用 移动端 前端框架 h5 app - > web app

## emmet语法

### Nesting operators 嵌套操作符

Nesting operators are used to position abbreviation elements inside generated tree: whether it should be placed inside or near the context element.

#### Child: `>` 子元素

You can use `>` operator to nest elements inside each other:

```
div>ul>li
```

...will produce

```
<div>
    <ul>
        <li></li>
    </ul>
</div>
```

#### Sibling: `+` 兄弟元素

Use `+` operator to place elements near each other, on the same level:

```
div+p+bq
```

...will output

```
<div></div>
<p></p>
<blockquote></blockquote>
```

#### Climb-up: `^` 返回上层

With `>` operator you’re descending down the generated tree and positions of all sibling elements will be resolved against the most deepest element:

`>` 操作符加深结构层次：

```
div+div>p>span+em
```

...will be expanded to

```
<div></div>
<div>
    <p><span></span><em></em></p>
</div>
```

With `^` operator, you can climb one level up the tree and change context where following elements should appear:

`>` 操作符返回上一层：

```
div+div>p>span+em^bq
```

...outputs to

```
<div></div>
<div>
    <p><span></span><em></em></p>
    <blockquote></blockquote>
</div>
```

You can use as many `^` operators as you like, each operator will move one level up:

多个`^`连写将向上一层层返回：

```
div+div>p>span+em^^^bq
```

...will output to

```
<div></div>
<div>
    <p><span></span><em></em></p>
</div>
<blockquote></blockquote>
```

#### Multiplication: `*` 乘法

With `*` operator you can define how many times element should be outputted:

```
ul>li*5
```

...outputs to

```
<ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

#### Grouping: `()` 分组

Parenthesises are used by Emmets’ power users for grouping subtrees in complex abbreviations:

```
div>(header>ul>li*2>a)+footer>p
```

...expands to

```
<div>
    <header>
        <ul>
            <li><a href=""></a></li>
            <li><a href=""></a></li>
        </ul>
    </header>
    <footer>
        <p></p>
    </footer>
</div>
```

If you’re working with browser’s DOM, you may think of groups as Document Fragments: each group contains abbreviation subtree and all the following elements are inserted at the same level as the first element of group.

可以将分组当作 Document Fragments，后续元素将与分组第一个元素同级。

You can nest groups inside each other and combine them with multiplication `*` operator:

分组嵌套，并且使用 `*` 操作法：

```
(div>dl>(dt+dd)*3)+footer>p
```

...produces

```
<div>
    <dl>
        <dt></dt>
        <dd></dd>
        <dt></dt>
        <dd></dd>
        <dt></dt>
        <dd></dd>
    </dl>
</div>
<footer>
    <p></p>
</footer>
```

With groups, you can literally write full page mark-up with a single abbreviation, but please don’t do that.

使用分组后，可以用一个缩写来生成整个页面，不过不要这么做。

### Attribute operators 属性操作符

Attribute operators are used to modify attributes of outputted elements. For example, in HTML and XML you can quickly add `class` attribute to generated element.

#### ID and CLASS

In CSS, you use `elem#id` and `elem.class` notation to reach the elements with specified `id` or `class` attributes. In Emmet, you can use the very same syntax to *add* these attributes to specified element:

Emmet 使用类似于 CSS 选择器的语法给元素添加属性：

```
div#header+div.page+div#footer.class1.class2.class3
```

...will output

```
<div id="header"></div>
<div class="page"></div>
<div id="footer" class="class1 class2 class3"></div>
```

#### Custom attributes 自定义属性

You can use `[attr]` notation (as in CSS) to add custom attributes to your element:

```
td[title="Hello world!" colspan=3]
```

...outputs

```
<td title="Hello world!" colspan="3"></td>
```

- You can place as many attributes as you like inside square brackets.
- 方括号内属性数量不限。
- You don’t have to specify attribute values: `td[colspan title]` will produce `<td colspan="" title="">` with tabstops inside each empty attribute (if your editor supports them).
- 没有指定值的属性将生成插入占位（需要编辑器支持）。
- You can use single or double quotes for quoting attribute values.
- 属性值使用单引号或双引号。
- You don’t need to quote values if they don’t contain spaces: `td[title=hello colspan=3]` will work.
- 属性值如果不包含空格可以省略引号。

#### Item numbering: `$` 编号

With multiplication `*` operator you can repeat elements, but with `$` you can *number* them. Place `$` operator inside element’s name, attribute’s name or attribute’s value to output current number of repeated element:

`*` 操作符可以生成重复元素，而 `$` 可以对元素编号。将 `$` 放在元素名、属性名或属性值中：

```
ul>li.item$*5
```

...outputs to

```
<ul>
    <li class="item1"></li>
    <li class="item2"></li>
    <li class="item3"></li>
    <li class="item4"></li>
    <li class="item5"></li>
</ul>
```

You can use multiple `$` in a row to pad number with zeroes:

多个连写的 `$` 可以生成带有前导零的编号：

```
ul>li.item$$$*5
```

...outputs to

```
<ul>
    <li class="item001"></li>
    <li class="item002"></li>
    <li class="item003"></li>
    <li class="item004"></li>
    <li class="item005"></li>
</ul>
```

**Changing numbering base and direction**

With `@` modifier, you can change numbering direction (ascending or descending) and base (e.g. start value).

使用 `@` 修饰符，可以改变编号的方向（升序或降序）及起点。

For example, to change direction, add `@-` after `$`:

例如改变方向，将 `@-` 放在 `$` 后：

```
ul>li.item$@-*5
```

…outputs to

```
<ul>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
    <li class="item2"></li>
    <li class="item1"></li>
</ul>
```

To change counter base value, add `@N` modifier to `$`:

改变起点，将 `@N` 放在 `$` 后：

```
ul>li.item$@3*5
```

…transforms to

```
<ul>
    <li class="item3"></li>
    <li class="item4"></li>
    <li class="item5"></li>
    <li class="item6"></li>
    <li class="item7"></li>
</ul>
```

You can use these modifiers together:

混合使用这几种修饰符：

```
ul>li.item$@-3*5
```

…is transformed to

```
<ul>
    <li class="item7"></li>
    <li class="item6"></li>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
</ul>
```

#### Text: `{}` 文本

You can use curly braces to add text to element:

使用大括号为元素添加文本（译注：类似于模板的插入符）

```
a{Click me}
```

...will produce

```
<a href="">Click me</a>
```

Note that `{text}` is used and parsed as a separate element (like, `div`, `p` etc.) but has a special meaning when written right after element. For example, `a{click}` and `a>{click}` will produce the same output, but `a{click}+b{here}` and `a>{click}+b{here}` won’t:

注意 `{text}` 类似于独立元素（比如`div`, `p`），不过当它紧跟在元素后面时有特别的意义。比如 `a{click}` 与 `a>{click}` 结果一样，而 `a{click}+b{here}` 与 `a>{click}+b{here}` 结果不一样：

```
<!-- a{click}+b{here} -->
<a href="">click</a><b>here</b>

<!-- a>{click}+b{here} -->
<a href="">click<b>here</b></a>
```

In second example the `<b>` element is placed *inside* `<a>` element. And that’s the difference: when `{text}` is written right after element, it doesn’t change parent context. Here’s more complex example showing why it is important:

第二个例子里 `<b>` 位于 `<a>` 内。这便是不同点： 当 `{text}` 紧跟在元素后面时，它没有改变父元素的上下文。下面用一个复杂例子来说明：

```
p>{Click }+a{here}+{ to continue}
```

...produces

```
<p>Click <a href="">here</a> to continue</p>
```

In this example, to write `Click here to continue` inside `<p>` element we have explicitly move down the tree with `>` operator after `p`, but in case of `a` element we don’t have to, since we need `<a>` element with `here` word only, without changing parent context.

在这个例子中，为了让 `<p>` 包含 `Click here to continue`，`p` 后面使用了 `>` 以进入子级结构，而 `a` 只需要包含文本 `here`，不用改变父元素上下文，所以不需要这样做。

For comparison, here’s the same abbreviation written without child `>` operator:

下面不用 `>` 做下对比：

```
p{Click }+a{here}+{ to continue}
```

...produces

```
<p>Click </p>
<a href="">here</a> to continue
```



## 其他

### 标签中间加<> 空格 等等

```html
 <!-- 不对 < > 显示不出 -->
    <p>我在学习<div>标签</p>
    <!-- 用html实体符来表示 -->
    <p>我在学习&lt;div&gt;标签&nbsp;</p>
    <!-- 分割线 一般不用 渲染效果不一样-->
    <br />
    <hr />
```

