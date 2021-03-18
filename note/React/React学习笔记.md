#### React.createElement()

语法

```javascript
React.createElement(
  type,  // 标签名 比如'button'  也可以是React组件类型
  [props],
  [...children]
)
```

作用 创建并返回指定的新**React 元素**， 但由于这不是真正的element(DOM对象)，所以一般称为**虚拟DOM 对象**



**对比React元素和函数组件**

createElement直接返回一个 React元素

返回element的函数，可以多次执行每次得到**最新的虚拟div** 

然后React 虚拟dom diff 看下不同 局部更新视图

上面点了按钮 n不会加一 因为这个元素在被创建出来n 的值就已经确定了

```javascript
const root = document.querySelector("#root");
const App = 
  React.createElement("div", { className: "red" }, [
    n,
    React.createElement(
      "button",
      {
        onClick: () => {
          n += 1;
          ReactDOM.render(App, root);
        }
      },
      "+1"
    )
  ]);
ReactDOM.render(App, root);
```

下面面点了按钮 n会加一 因为App 是一个返回element 的函数， 每次能得到最新的虚拟div 所以n的值会更新

```javascript
const root = document.querySelector("#root");
const App = () =>
  React.createElement("div", { className: "red" }, [
    n,
    React.createElement(
      "button",
      {
        onClick: () => {
          n += 1;
          ReactDOM.render(App(), root);
        }
      },
      "+1"
    )
  ]);
ReactDOM.render(App(), root);
```



所以当传了 React.createElement的逻辑是：

1. 当传了一个字符串'div' 时 会创建一个div
2. 当传了一个函数时 会调用该函数 获取器返回值
3. 当传了一个类是 则会在类前面加个 new 执行constructor 获取一个组件对象 然后调用对象的render 方法获取其返回值







#### JSX

把 `<button onClick="add">+1</button>` 改成

`React.createElement('button',{onClick:...},'+1')`

这其中用到了编译原理  这是不是需要用到 jsx-loader 去转译呢

实时上不用 因为`jsx-loader` 已经被`babel-loader`取代了

而`babel-loader`被webpack内置了

如果之前的用JSX语法写就是

```javascript
      let n = 0;
      const App = () => (
        <div className="red">
          {n}
          <button
            onClick={() => {
              n += 1;
              render();
            }}
          >
            +1
          </button>
        </div>
      );
      const render = () => {
        ReactDOM.render(<App />, document.querySelector("#root"));
      };
      render();
```



**如何使用JSX**

1.CDN

 引入babel.min.js

把`<script>` 改成 `<script type = 'text/babel'>`

babel会自动进行转译

效率太低,它要下载一个 babel.min.is 它还要在浏览器端把 JSX 翻译成 JS 

2.webpack + babel-loader

3.create-react-app

脚手架 和 @vue/cli 用法类似

全局安装 `yarn global add create-react-app`

初始化目录 create-react-app react-demo-1

进入目录 cd react-demo-1

开始开发 yarn start

看看App.js 是不是默认用了 jsx语法

因为webpack 让JS默认走了babel-loader



注意事项

* 注意 classname

 `<div classname=red>n</div>` 被转译为 `React. Createelement ('div',{classname: red}, "n")`

* 插入变量

标签里面的所有 JS 代码都要用{}包起来

如果你需要变量 n，那么就用{}把 n 包起来

如果你需要对象，那么就要用把对象包起来，如`{{ name: "zhy"}}`

* 习惯 return 后面加()



JSX - if

```javascript
const Component = () => {
  const content = (
  	<div>
    	{ n%2 === 0 ? <div> n是偶数</div> : <span> n是奇数</span> }
    </div>
  )
  return content
}

// or
const Component = () => {
  const inner = n%2 === 0 ? <div> n是偶数</div> : <span> n是奇数</span>
  return <div>
        	{inner}
        </div> 
}
```



JSX - loop

```javascript
const component = (props) =>{
  return props.numbers.map((n,index)=>{
    return <div>下标为{index} 值为{n}</div>
  })
}

// or
const component = (props) =>{
  const array = []
  for(let i = 0;i<props.length;i++){
    array.push(<div>下标为{i} 值为{pros.numbers[i]}</div>)
  }
  return  <div>{array}</div>
}
```



#### 组件

函数组件

```javascript
function Welcome(props){
	return <h1>Hello,{props.name}</h1>
}	
<Welcome name="zhy">
```

类组件

```javascript
class Welcome extends React.Component{
  render(){
    return <h1>Hello,{this.props.name}</h1>
  }
}	
<Welcome name="zhy">
```



** `<Welcome/>` 会被**翻译**成什么？ 切记！ 是**createElement** 不是其他的

`</div>` 会被翻译成 `React.createElement('div')`

`<Welcome/>`翻译为`React.createElement(Welcome)`

直接用 [Babel online](https://www.babeljs.cn/repl) 就能看到结果

 ```javascript
function Welcome(props){
	return <h1>Hello,{props.name}</h1>
}	
<Welcome name="zhy"/>
// 被翻译成
"use strict";

function Welcome(props) {
  return /*#__PURE__*/React.createElement("h1", null, "Hello,", props.name);
}

/*#__PURE__*/
React.createElement(Welcome, {
  name: "zhy"
});
 ```



几个实际应用的问题

#### 用state 传内部数据

[代码示例](https://codesandbox.io/s/quiet-surf-lrnpf?file=/src/index.js)

```javascript
import React, { useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

function App() {
  return (
    <div className="App">
      爸爸
      <Son />
    </div>
  );
}

class Son extends React.Component {
  constructor() {
    super();
    this.state = {
      n: 0
    };
  }
  add() {
    console.log(this);
    // 1.为什么不用 this.state.n += 1
    this.setState({
      n: this.state.n + 1
    });
  }
  render() {
    console.log(this); // 2.为什么是指向组件实例Son
    return (
      <div className="Son">
        儿子 n:{this.state.n}
        // 2.为什么不能用 onClick={this.add} 
        <button onClick={() => this.add()}> +1 </button>
        <Grandson />
      </div>
    );
  }
}

const Grandson = () => {
  // 一个set 一个get?
  const [n, setN] = useState(0);
  return (
    <div className="Grandson">
      孙子 n:{n}
      <button onClick={() => setN(n + 1)}>+1</button>
    </div>
  );
};

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

1. 为什么不用 this.state.n += 1?

因为 如果React 不像vue会去进行数据的双向绑定 去劫持数据 然后监听该数据 即使你数据变了 不去触发渲染也不会更新视图 所以需要用setState 去异步更新视图

* 由于setState 是异步的 所以如果在下面打出n 的值 会发现还是旧的n 所以为了避免这种情况 可以这么写：

```javascript
add(){
  this.setState(state=>{
    const n = state.n+1
    console.log(n)
    return {n}
  })
}
```

2. 为什么用 `onClick={this.add}` add执行的时候内部this 是window 而用箭头函数就没问题

第一个log指向Son是因为 这时在 render 里 所以得看render会如何被调用，很明显render是被**React调用的** React 会把this设置成**组件实例** 所以指向对象实例Son

之后在绑定 `onClick={this.add}`  的时候 实际上在触发了之后 实际上执行的是 button.onClick.call(null,event) 所以null 在浏览器中被置换为window 那么有什么办法去修改呢？

**箭头函数**不存在这个问题 因为箭头函数只会从自己的**作用域链的上一层**继承this 换句话说箭头函数的this会指向当前最近的费箭头函数的this,所以此时指向的就是render里的this 就是组件实例 而之后不论怎样都不会改变

或者直接用 `onClick={this.add.bind(this)}` 把这个回调函数的this 绑定到实例上

因为箭头函数在class 里面声明 不像普通函数是默认在原型里面 它是作为**实例的一个属性**

所以事件绑定的最终写法：

```javascript
class Son extends React.Component{
  addN = () => this.setState({n:this.state.n+1})
  construrctor{
    this.addN = () => this.setState({n:this.state.n+1})
  }  // 👆🏻两种写法完全等价 
   
  // 👇两种写法完全等价
  addN(){
    this.setState({n:this.state.n+1})
  }
  addN():function(){
    this.setState({n:this.state.n+1})
  }
}
```

**上面在对象上 下面在原型上**



3. 函数是组件  一个set 一个get

只关心初始值 怎么读 怎么写 `const [n, setN] = useState(0);`

读 `{n}`  写 `{()=>{setN(n+1)}}`  setN 会产生一个新的 n  



#### 用props 传外部数据

[代码示例](https://codesandbox.io/s/wild-currying-y6t17?file=/src/index.js:0-640)

```javascript
import React from "react";
import ReactDOM from "react-dom";

import "./styles.css";

function App() {
  return (
    <div className="App">
      爸爸
      <Son messageForSon="儿子你好" />
    </div>
  );
}

class Son extends React.Component {
  render() {
    return (
      <div className="Son">
        我是儿子，我爸对我说「{this.props.messageForSon}」
        <Grandson messageForGrandson="孙贼你好" />
      </div>
    );
  }
}

const Grandson = props => {
  return (
    <div className="Grandson">
      我是孙子，我爸对我说「{props.messageForGrandson}」
    </div>
  );
};

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```





复杂的state怎么办

[代码示例](https://codesandbox.io/s/vibrant-pond-4quli?file=/src/index.js)

class 组件没变化 重复一遍代码即可 函数组键有两种写法

```javascript
function App() {
  return (
    <div className="App">
      爸爸
      <Son />
    </div>
  );
}

class Son extends React.Component {
  constructor() {
    super();
    this.state = {
      n: 0,
      m: 0
    };
  }
  addN() {
    this.setState({ n: this.state.n + 1 });
    // 1. m 会被覆盖为 undefined 吗？
  }
  addM() {
    this.setState({ m: this.state.m + 1 });
    // 1. n 会被覆盖为 undefined 吗？
  }
  render() {
    return (
      <div className="Son">
        儿子 n: {this.state.n}
        <button onClick={() => this.addN()}>n+1</button>
        m: {this.state.m}
        <button onClick={() => this.addM()}>m+1</button>
        <Grandson />
      </div>
    );
  }
}

const Grandson = () => {
  // 2. 还有其他写法吗
  const [n, setN] = React.useState(0);
  const [m,setM] = React.useState(0);
  return (
    <div className="Grandson">
      孙子 n:{n}
      <button onClick={() => setN(n + 1)}>n+1</button>
  		m:{m}
      <button onClick={() => setN(m + 1)}>m+1</button>
    </div>
  );
};

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

1. setState 时候 只传了m/n，另一个会被覆盖成undefined吗 

不会因为 setState 会自动帮你合并对象的**第一层**属性（**第二层是不行的！！**）

第二层不会合并的[例子](https://codesandbox.io/s/winter-snowflake-o57z4)  记住加 `...` 

类似于 `    this.setState({...this.state, n: this.state.n + 1 });`

2. 还有一种写法 但是不推荐

```javascript
const Grandson = () => {
  // 2. 还有其他写法吗
  const [state, setState] = React.useState({
    n:0,m:0
  })
  // 一定要写 ...state 一定要写...state 一定要写 ...state 
  return (
    <div className="Grandson">
      孙子 n:{state:n}
      <button onClick={() => setState(...state,n:state.n + 1)}>n+1</button>
  		m:{state.m}
      <button onClick={() => setState(...state,m:state.m + 1)}>m+1</button>
    </div>
  );
};
```

一定要写 `... state` 因为 函数组件setState **不会帮你合并其他属性** 如果只传一个 其他会被覆盖成undefined

 

#### 事件绑定

最终写法

```javascript
class Son extends React.Component{
  constructor() {
    super();
    this.state = {
      n: 0,
    };
  }
  addN = () => this.setState({n:this.state.n+1})
  return (
      <div>
        <button onClick={addN}>n+1</button>
      </div>
   );
}
```



#### props 

传外部数据

初始化 

把props传给constructor 这会把外部props 放到this上

```javascript
class B extends React.Component{
  constructor(props){
    super(props);
  }
  render(){}
}
```

不能改写props！

改props 的值 

`this.props = {/* 另一个对象*/}` 没有意义 只是指向了一个新对象而已

`this.props.xxx = "hello"` 没有意义 外部数据 不应该内部改值



相关的钩子

`componentWillReceiveProps`钩子

当组件接受新的props时会触发此钩子

现在已经弃用了 改成`UNSAFE_componentWillReceiveProps`

```javascript
class App extends React.Component {
  constructor(props) {
    super();
    this.state = {
      x: 1
    };
  }
  onClick = () => this.setState({ x: this.state.x + 1 });
  render() {
    return (
      <div className="App">
        App <button onClick={this.onClick}>+1</button>
        <B name={this.state.x} />
      </div>
    );
  }
}

class B extends React.Component {
  UNSAFE_componentWillReceiveProps(nextProps, nextContent) {
    console.log("旧的props");
    console.log(this.props);  // 旧的props
    console.log("props变化了");
    console.log("新的props");
    console.log(nextProps); // nextProps是 变化后的props
  }

  render() {
    return <div>{this.props.name}</div>;
  }
}
// 按加一后会显示
// 旧的props 
// {name: 1}
// props变化了 
// 新的props 
// {name: 2}
```



#### state

读

`this.state.xxx.yy.zz`

写

`this.setState(newState,fn)`

setState 不会立即改变this.state，会在当前代码运行完后，再去更新state，从而触发UI更新

fn 是set成功后的回调

`this.setState((state)=>{newState},fn)`

那加一来说 如果传函数 运行多次就是实际效果 因为引用的当前的state

```javascript
// n初始值为1
onClick = ()=> {
  this.setState({n:this.state.n+1})  //  2  因为引用的都是旧state
  this.setState({n:this.state.n+1})  // 还是2 
}
 
onClick2 = () => {
  this.setState((state)=>({n:state.n+1}))  // 2  引用的当前的state
  this.setState((state)=>({n:state.n+1}))  // 3
}
// 用onClick2 这种方法时 componentWillReceiveProps 钩子仅触发一次 因为React会把两步合并成一步
```

`this.state.n+=1 this.setState(this.state)`

最好不这么写 最好生成一个新的对象