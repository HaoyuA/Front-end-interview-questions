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