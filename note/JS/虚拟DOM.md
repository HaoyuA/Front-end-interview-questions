虚拟dom (virtual dom)是什么？ 

一个代表dom树的对象， 通常含有**标签名 标签上的属性 事件监听和子元素们** 以及其他属性



Dom操作慢？ 

相对于JS原生API 如数组操作

任何基于DOM 的库（Vue/React) 都不可能在操作DOM时比DOM快



#### 优点

1. 减少 DOM 操作

虚拟 DOM 可以将多次操作合并为一次操作，比如你添加 1000 个节点，dom只能一个接一个操作 虚拟dom能一次性放上去（减少频率）

虚拟 DOM 借助 DOM diff 可以把多余的操作省掉，比如你添加 1000 个节点，其实只有 10 个是新增的（减少范围）

2. 跨平台

虚拟 DOM 不仅可以变成 DOM，还可以变成小程序、iOS 应用、安卓应用，因为虚拟 DOM 本质上只是一个 JS 对象



#### 缺点



虚拟dom长什么样子

React

```javascript
const vNode = {
  key: null,
  props: {
    children: [  // 子元素们
       { type: 'span', ... }, 
       { type: 'span', ... }
    ],
    className: "red" // 标签上的属性
    onClick: () => {} // 事件
  },
  ref: null,
  type: "div", // 标签名 or 组件名
  //...
}
```

Vue

```javascript
const vNode = {
  tag: "div", // 标签名 or 组件名
  data: {
    class: "red", // 标签上的属性
    on: {
      click: () => {} // 事件
    }
  },
  children: [ // 子元素们
    { tag: "span", ... },
    { tag: "span", ... }
  ],
  //...
}

```





#### dom diff

就是一个函数 我们称之为patch

pacthes = patch(oldVNode,newVNode)

patches 就是要运行的 DOM 操作，可能长这样：

```javascript
[

 {type: 'INSERT', vNode: ... },

 {type: 'TEXT', vNode: ... },

 {type: 'PROPS', propsPatch: [...]}

]
```



dom diff 的逻辑：

[dom diff源码剖析](https://zhuanlan.zhihu.com/p/20346379)

Tree diff

将新旧两棵树逐层对比 找出那些节点需要更新

如果节点是组件就看Component diff

如果节点是标签就看Element diff



Component diff

先看组件类型

类型不同直接替换（删除旧的）

类型相同只更新属性

然后再深入组件做 Tree diff



Element diff

如果节点是原生标签 则看标签名

标签名不同直接替换相同则只更新属性

然后深入标签后代做Tree diff



**Key**

dom diff里面

用一个例子来说明：

如下图，老集合中包含节点：A、B、C、D，更新后的新集合中包含节点：B、A、D、C，此时新老集合进行 diff 差异化对比，发现 B != A，则创建并插入 B 至新集合，删除老集合 A；以此类推，创建并插入 A、D 和 C，删除 B、C 和 D。

![vdom-1](/assets/vdom-1.png)

React 发现这类操作繁琐冗余，因为这些都是相同的节点，但由于位置发生变化，导致需要进行繁杂低效的删除、创建操作，其实只要对这些节点进行位置移动即可。

针对这一现象，React 提出优化策略：允许开发者对同一层级的同组子节点，添加唯一 **key** 进行区分，虽然只是小小的改动，性能上却发生了翻天覆地的变化！

新老集合所包含的节点，如下图所示，新老集合进行 diff 差异化对比，通过 key 发现新老集合中的节点都是**相同的节点**，因此无需进行节点**删除和创建**，只需要将老集合中节点的位置进行移动，更新为新集合中节点的位置，此时 React 给出的 diff 结果为：B、D 不做任何操作，A、C 进行移动操作，即可。

![vdom-2](/assets/vdom-2.png)

