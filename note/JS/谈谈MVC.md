MVC 

#### 概念：

mvc 是一种架构设计模式，我觉得它是一种很抽象的结构 在我自己写的实际代码中可能难以体现 但是无疑现在web前端开发也越来越向传统应用发展，传统应用中的设计模式也逐渐在融入web前端开发， 因为这种模式能使代码的结构更明确，做到高内聚，低耦合，将 m v c 三者分开也能能提高灵活性和复用性，在现在的前端框架中，也从经典的MVC模式衍生了诸多MV*模式 (mvvm) 



如果看实际代码的话：

假设要设计一个计算器的组件 其中有加减乘除四个按钮 基数是100 每按一个按钮数字就会相应的更新 那么如果用mvc实现的话就是

**model**   数据模型 负责操作数据

```javascript
// 数据相关的放到m
const m = {
  data: {
    n: parseInt(localStorage.getItem("n") ?? 100),
  },
  create() {},
  delete() {},
  update(data) {
    Object.assign(m.data, data);
    eventBus.trigger("m:updated");
    localStorage.setItem("n", m.data.n);
  },
  get() {},
};
```



**view** 视图 负责UI界面

```javascript
const v = {
  el: null,
  html: `
		<div>
      <div class="output"><span id="number">{{n}}</span></div>
      <div class="actions">
        <button id="add1">+1</button>
        <button id="minus1">-1</button>
        <button id="mul2">*2</button>
        <button id="divide2">/2</button>
      </div>
    </div>
	`,
  init(container) {
    v.el = $(container);
  },
  render(n) {
    if (v.el.children.length !== 0) v.el.empty();
    $(v.html.replace("{{n}}", n)).appendTo($(v.el));
  },
};
```



**controller** 控制器 负责其他

```javascript
const c = {
  init(container) {
    v.init(container);
    v.render(m.data.n); // view = render(data)
    eventBus.on("m:updated", () => {
      v.render(m.data.n);
    });
    c.autoBindEvents();
  },
  events: {
    "click #add1": "add",
    "click #minus1": "minus",
    "click #mul2": "mul",
    "click #divide2": "div",
  },
  add() {
    m.update({ n: m.data.n + 1 });
  },
  minus() {
    m.update({ n: m.data.n - 1 });
  },
  mul() {
    m.update({ n: m.data.n * 2 });
  },
  div() {
    m.update({ n: m.data.n / 2 });
  },

  autoBindEvents() {
    for (let key in c.events) {
      const value = c[c.events[key]];
      const spliceIndex = key.indexOf(" ");
      const part1 = key.slice(0, spliceIndex);
      const part2 = key.slice(spliceIndex + 1);
      v.el.on(part1, part2, value);
    }
  },
}
```

如同上面代码所示， 如果是提到javascript的 MVC 模式的话  view承接了部分controller的功能，负责处理用户输入(事件events)   它依赖于一个controller为她做决定或处理用户事件。(v.autobindEvents())同时，view也可以委托controller处理model的更改(m.update(...))。model数据变化后通知view进行更新(v.render())，显示给用户。这个过程是一个圆，一个循环的过程。

![mvc-1](/assets/mvc-1.png)



#### 观察者模式

其中mvc中的 view 和model 之间的模式就是观察者模式

观察者模式(observer pattern) 既然是观察者模式 那么就一定有观察的人(obeserver)和被观察的人-目标(object) 当观察对象的状态发生改变时 所有依赖于它的对象将会的到通知并被自动更新 这也是一个相对抽象的概念 所以需要用实际例子去说明：

其中在刚刚写的代码中view观察model 事先在model 上注册 一旦model上的数据改变 view就会接受到通知去更新页面 那么 是怎么做到 m触发了某方法 v也会相应的触发呢？ 答案是eventBus

```javascript
// m 中的 update 就会触发 v.render()
update(data) {
    Object.assign(m.data, data);
    eventBus.trigger("m:updated");
    localStorage.setItem("n", m.data.n);
  },
```



#### EventBus

是用来做组件通信的 这里更像是传递事件的一个对像

方法为 on off trigger 和jquery 的事件监听很像 其实dom的addEventListener 和 dispatchEvent也是eventBus的体现 这些方法挂在 EventTarget 这层原型上(倒数第二层 Object 原型前面)

```javascript
const eventBus = $(window)
// c中初始化的方法就用 on 监听了 m:updated 这个事件 所以在用户操作时 m 的data确实改变了 就会触发 trigger 方法 更新v 
const m = {
  update(data) {
    Object.assign(m.data, data);
    eventBus.trigger("m:updated");
    localStorage.setItem("n", m.data.n);
  },
  get() {},
};
const c = {
  init(container) {
    eventBus.on("m:updated", () => {
      v.render(m.data.n);
    }),
  },
}
```





#### 表驱动编程

其中在绑定controller 的方法中有这么一段代码 实际上就是表驱动编程

如果不用的话可能就会造成大量的代码重复 而且一旦数据改变 代码的变动也会很多 

我觉得表驱动编程就是把思维逻辑 和 数据分离解耦 这样即使数据更新了也可以做到很快的更新代码

```javascript
autoBindEvents() {
    for (let key in c.events) {
      const value = c[c.events[key]];
      const spliceIndex = key.indexOf(" ");
      const part1 = key.slice(0, spliceIndex);
      const part2 = key.slice(spliceIndex + 1);
      v.el.on(part1, part2, value);
    }
  },
```

