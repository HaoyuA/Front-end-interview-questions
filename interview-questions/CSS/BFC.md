BFC （Block Formatting Context ）块格式化上下文

**块格式化上下文（Block Formatting Context，BFC）** 是Web页面的可视CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。



如何创建快格式上下文：

- [`overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow) 计算值(Computed)不为 `visible` 的块元素
- 浮动元素（元素的 [`float`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float) 不是 `none`）
- 行内块元素
- 弹性元素（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 为 `flex` 或 `inline-flex `元素的直接子元素）



解决什么问题？

* BFC清除浮动

  给父元素设置`over-flow:auto` 来简单的实现BFC清除浮动

* BFC解决外边距合并问题

  两个块同一个BFC会造成外边距折叠，但如果对这两个块分别设置BFC，那么边距重叠的问题就不存在了。

  ```html
  <body>
    <div class="container">
      <div class="inner">1</div>
      <div class="inner">2</div>
      <div class="inner">3</div>
    </div>
  </body>
  ```

  此时三个元素的上下间隔都是10px, 因为三个元素同属于一个BFC。 现在我们做如下操作:

  ```html
    <div class="container">
      <div class="inner">1</div>
      <div class="bfc">
        <div class="inner">2</div>
      </div>
      <div class="inner">3</div>
    </div>
  ```

  style增加:

  ```css
  .bfc{
      overflow: hidden;
  }
  ```

  就能看到间隔变成原来的两倍

  

  [🌰](https://jsbin.com/jedavif/edit?html,css,output)



[BFC MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

