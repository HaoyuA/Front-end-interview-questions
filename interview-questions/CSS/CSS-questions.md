如何让文字不换行

```css
<style>
#xxx{
  white-space: nowrap; /*不要换行...*/
  text-overflow: ellipsis; /*如果溢出就省略*/
  overflow: hidden;/*省略变...*/
}
</style>
```



清除默认样式：

```css
<style>
  *{
    margin: 0;
    padding:0;
    box-sizing: border-box;
  }
  a{
    color: inherit;
    text-decoration: none;
  }
  ul,ol{
    list-style:none;
  }
  table{
    border-collapse: collapse;
    barder-spacing:none;
  }
</style>
```

<style>
  *{
    margin: 0;
    padding:0;
    box-sizing: border-box;
  }
  a{
    color: inherit;
    text-decoration: none;
  }
  ul,ol{
    list-style:none;
  }
  table{
    border-collapse: collapse;
    barder-spacing:none;
  }
</style>

优化浏览器渲染加载

简单来说就是

js优化

使用 requestAnimationFrame 代替 setTimeout 和setInterval

css优化

使用will-change或translate

[渲染优化google文档](https://developers.google.com/web/fundamentals/performance/rendering)