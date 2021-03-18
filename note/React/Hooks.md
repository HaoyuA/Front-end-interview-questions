## React Hooks

![react-1](/assets/react-1.png)

### useState

#### 模拟useState

首先假设我们写了一个App 组件代码如下：

```javascript
import React from "react";
import ReactDOM from "react-dom";
const rootElement = document.getElementById("root");

function App() {
  const [n, setN] = React.useState(0);
  return (
    <div className="App">
      <p>{n}</p>
      <p>
        <button onClick={() => setN(n + 1)}>+1</button>
      </p>
    </div>
  );
}

ReactDOM.render(<App />, rootElement);
```

那么按下了 button会发生什么呢？

首先 首次渲染会调用 `App()` 这个函数 render `<App/>`

这个函数会返回一个虚拟`div`  即为**React 元素 ** 然后React 会根据它变成真实的`div` node

当用户点击button  调用 `setN(n+1)`  会再次render `<App/>`

即还是会调用`App()`  得到新的 虚拟`div` 根据 DOM diff 去更新真div

每次调用`App()` 都会调用`useState(0)`

然而 这个n 每次都是不一样的 `console.log(n)` 第一次是0 按下按钮后是1 



useState做了什么 

先看setN 做了什么

setN 一定会修改数据 将某个值x 将n+1 存入x

setN 一定会触发 `<App/>` 重新渲染



useState 

useState 肯定会从x 读取n的最新值



X 

每个组件有自己的数据x , 我们将其命名为state



尝试实现React.useState

```javascript
let _state;
const myUseState = (initialValue) => {
  _state = _state === undefined ? initialValue : _state;
  const setState = (newValue) => {
    _state = newValue;
    render();
  };
  return [_state, setState];
};

const render = () => {
  ReactDOM.render(<App />, rootElement);
};

function App() {
  const [n, setN] = myUseState(0);
  return (
    <div className="App">
      <p>{n}</p>
      <p>
        <button onClick={() => setN(n + 1)}>+1</button>
      </p>
    </div>
  );
}

ReactDOM.render(<App />, rootElement);
```



什面这种情况有一个问题 若有两个变量都用了useState 怎么办 

答案 用数组去存 state 但是这个的**问题** 是**很依赖调用顺序**  若每次render调用顺序错了  就会bug

```javascript
let _state = [];
let index = 0;
const myUseState = (initialValue) => {
  const currentIndex = index;
  _state[currentIndex] =
    _state[currentIndex] === undefined ? initialValue : _state[currentIndex];
  const setState = (newValue) => {
    _state[currentIndex] = newValue;
    render();
  };
  index += 1; // 若有多个useState 就写入数组的第 index 个
  return [_state[currentIndex], setState];
};

const render = () => {
  index = 0;   // 每次render重置index 防止数组写入过多的数据
  ReactDOM.render(<App />, rootElement);
};
```

Hooks 如果写在if里面

```javascript
let m,setM
if(n%2===1){
  [m,setM] = React.useState(0)
}
// 这样写会报错 hooks must be called in the exact same order
```



每一个组件都有一个虚拟的state 和index 

React的节点应该是FiberNode

_state 的真名称为 memorizedState index的实现用到了**链表**



由于函数组件每次出行渲染 函数组件就会执行

对应state都会出现分身  如果不想出现分身 可以用 useRef/useContext 等

[具体引用](https://juejin.im/post/5bdfc1c4e51d4539f4178e1f)



#### useState注意事项

 `const [n, setN] = React.useState (0)`

 `const [user, setUser]= React.usestate ({name: Fr})`

**注意事项 1:** 不可局部更新

如果 state 是一个对象，能否部分 setState

答案是不行，请看[示例代码](https://codesandbox.io/s/snowy-pine-7gdw2) 因为 setstate 不会帮我们**合并属性**



**注意事项 2**: 地址要变

下面这种写法 点了按钮 user会变吗？ 并不会

```javascript
function App() {
  const [user, setUser] = useState({ name: "Frank", age: 18 });
  const onClick = () => {
    user.name = "Jack";
    setUser(user);
  };
  return (
    <div className="App">
      <h1>{user.name}</h1>
      <h2>{user.age}</h2>
      <button onClick={onClick}>Click</button>
    </div>
  );
}
```

 setstate (obj）如果obj **地址不変**，那么 React 就认为**数据没有变化**

**注意事项3** useState接受函数(很少用)

```javascript
const [state,setState] = useState(()=>{
  return initialState
})
```

该函数返回初始state 且只执行一次

setState 接收函数

```javascript
setN(n=>n+1) 
setN(n=>n+1) 
setN(n=>n+1)  // 写几次就会加几次

setN(n+1)
setN(n+2)  // 只有最后一次有效 即n+2
```



### useReducer

1.创建初始值initialState

2.创建所有操作reducer(state,action)

3.传给useReducer 得到读和写API

4.调用写({type:'操作类型'})

总的来说是useState的复杂版

```javascript
const initialState = {
  n:0
}

const reducer = (state,action) =>{
  if(action.type === 'add'){
    return {n:state.n+action.number}
  }else if(action.type === 'mult'){
    return {n:state.n+action.number}
  }else{
    throw new Error('unkonwn type')
  }
}

function App(){
  const [state,dispatch] = useReducer(reducer,initialState)
  const {n} =state
  const onClick = ()=>{
    dispatch({type:add,number:1})
	}
  const onClick2 = ()=>{
    dispatch({type:add,number:2})
  }
  return (
  	<div className = 'App'>
    	<h1>n:{n}</h1>
			<button onClick = {onClick}> +1</button>
 			<button onClick = {onClick2}> +2</button>
		</div>
  )
}
```

一个同useReducer的表单[例子](https://codesandbox.io/s/clever-chatterjee-391o9?file=/src/index.js)





### useRef

 useRef 不仅可以用于div 还可以用于任意数据

```javascript
function App() {
  const nRef = React.useRef(0);  // 就是 {current:0}
  const log = () => setTimeout(() => console.log(`n: ${nRef.current}`), 1000);
  return (
    <div className="App">
      <p>{nRef.current} 这里并不能实时更新</p>
      <p>
        <button onClick={() => (nRef.current += 1)}>+1</button>
        <button onClick={log}>log</button>
      </p>
    </div>
  );
}
```





### useContext

useContext 就好比是全局对象 不过是你指定的上下文中

```javascript
const themeContext = React.createContext(null);

function App() {
  const [theme, setTheme] = React.useState("red");
  return (
    <themeContext.Provider value={{ theme, setTheme }}>
      <div className={`App ${theme}`}>
        <p>{theme}</p>
        <div>
          <ChildA />
        </div>
        <div>
          <ChildB />
        </div>
      </div>
    </themeContext.Provider>
  );
}

function ChildA() {
  const { setTheme } = React.useContext(themeContext);
  return (
    <div>
      <button onClick={() => setTheme("red")}>red</button>
    </div>
  );
}

function ChildB() {
  const { setTheme } = React.useContext(themeContext);
  return (
    <div>
      <button onClick={() => setTheme("blue")}>blue</button>
    </div>
  );
}
```



