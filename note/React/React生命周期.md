### React生命周期

![react-2](/assets/react-2.png)

类比如下代码

```javascript
let div = document.createElement('div')
// 这是div 的 create/ constructor 过程
div.textContent = 'hi'
// 这是初始化state
document.body.appendChild(div)
// 这是div的mount 过程
div.textContent = 'hi2'
// 这是div的update过程
div.remove()
// 这是div的unmount过程
```

React组件也有这些过程 我们诚挚为生命周期

函数列表

`constructor()`

`static getDerivedStateFromProps()`

`shouldComponentUpdate()`

`render()`

`getSnapshotBeforeUpdate()`

`componentDidMount()`

`componentDidUpdate()`

`componentWillUnmount()`

`static getDerivedStateFromError()`

`componentDidCatch()`



#### constructor 

初始化props

初始化state 但此时不能调用setState

用来bind this 在constructor 里写

`this.onClick = this.onClick.bind(this)`

可用新语法代替



#### shouldComponentUpdate

返回**true** 表示**不阻止**UI 更新

返回**false** 表示**阻止**UI更新

shouldComponentUpdate有什么用？

它允许我们收到判断是否进行组件更新 我们可以根据应用场景灵活的设置返回值 以避免不必要的更新

同样以加一为例

```javascript
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      n: 1
    };
  }
  onClick = () => {
    this.setState((state) => ({
      n: state.n + 1
    }));
    this.setState((state) => ({
      n: state.n - 1
    }));
    // 虽然都是 {n:1} 和 {n:1} 但是实际上是两个对象
  };
  shouldComponentUpdate(nextProps, nextState) {
    if (nextState.n === this.state.n) {  // 这里判断如果对象n属性没变那么没必要去触发render
      return false;
    } else {
      return true;
    }
  }
  render() {
    console.log("render了1次");
    return (
      <div>
        App
        <div>
          {this.state.n}
          <button onClick={this.onClick}>+1</button>
        </div>
      </div>
    );
  }
}
```

`React.PureComponent` 也会做同样的事情 即会对比新 state 和旧 state 的每一个 key 以及新 props 和旧 props 的每一个 key 一样就不render 如果换成这个 就不需要写 `shouldComponentUpdate`了

它会自动对新旧state 进行对比 **注意**：只对比第一层



#### render

展示视图 `return (<div>...</div>)`

小tips:

* 如果想返回两个div 而不是用一个div包住的两个 可以用<React.Fragment>包起 这种可以缩写成<></>
* 可以写if else
* 可以写for
  * `return this.state.array.map(n=><span key={n}>{n}</span>)`



#### componentDidMount

1.在元素插入页面后执行 这些代码**依赖于DOM** 🌰 展示一个元素的高度

2.此处可以发起**AJAX请求**  用于**加载数据**

3.**首次渲染**会执行此钩子

```javascript
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      n: 1,
      width: undefined
    };
  }

  componentDidMount() {
    const div = document.getElementsById("xxx");
    const { width } = div.getBoundingClientRect();
    this.setState({ width });
  }

  render() {
    return <div id= 'xxx'>Hello World,{this.state.width}</div>;
  }
}

// or 用ref 去拿这个node
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      n: 1,
      width: undefined
    };
    this.divRef = React.createRef(); // 实例化
  }
  componentDidMount() {
    const div = this.divRef.current;  // 取时别忘了加 .current 
    console.log(div)  // 就是一个 node 
    const { width } = div.getBoundingClientRect();
    this.setState({ width });
  }

  render() {
    return <div ref={this.divRef}>Hello World,{this.state.width}</div>;  // 赋给ref属性
  }
}
```





#### componentDidUpdate

componentDidUpdate(prevProps,prevState,snapshot)

在视图更新后执行代码

此处也可以发起AJAX请求，用于**更新数据 ** 比如说userId 变了 这时候就需要发起新请求 更新视图

**首次渲染不会**执行此钩子

在此处setState 可能会引起无线循环 除非放if里

若 shouldComponentUpdate返回false 则不触发此钩子



#### componentWillUnmount

用途

组件将要被移出页面然后被销毁是执行

unmount过的组件不会再次mount



如果在componentDidMount里面监听了window scroll

那么就要在componentWillUnmount里面取消监听

如果在componentDidMount里面创建了Timer

那么就要在componentWillUnmount里面clear掉

如果在componentDidMount创建了AJAX请求

那么就要在componentWillUnmount取消AJAX



### 函数组件模拟生命周期

创建方式

```javascript
const hello = (props) =>{
  return <div>{props.message}</div>
}

const hello = props => <div>{props.message}</div>

function hello(){
  return <div>{props.message}</div>
}
```

如果要用函数组件代替class组件

有两个问题

1.函数组件没有state

React v16.8.0推出hooks API

其中一个API叫useState可以解决问题

2.函数组件没有生命周期

React v16..8.0推出Hooks API

其中一个API叫做useEffect可以解决问题

下面看代码

#### 模拟componentDidMount

```javascript
// 这么写每次state变化都会触发
const App = (props) => {
  const [n, setN] = useState(0);
  useEffect(() => {
    console.log("useEffect");
  });
  return (
    <div>
      {n}
      <button onClick={() => setN(n + 1)}>+1</button>
    </div>
  );
};

// 将useEffect 第二个参数改成 [] 表明只在第一次渲染调用 模拟componentDidMount
useEffect(() => {
    console.log("useEffect");
  }, []);

// constructor  函数组件执行的时候就相当于constructor
```



#### 模拟组件的componentDidUpdate

```java
// 如果监听具体看某个state中的值是否变化 可以再数组中加上那个值 模拟componentDidUpdate
const App = (props) => {
  const [n, setN] = useState(0);
  const [m, setM] = useState(0);
  useEffect(() => {
    console.log("n变了");
  },[n]);  // 这样就指监听n
  useEffect(() => {
    console.log("n或者m变了");
  },[n,m]); // 监听n和m
  return (
    <div>
      {n}
      <button onClick={() => setN(n + 1)}>n+1</button>
    </div>
    </hr>
 		<div>
      {m}
      <button onClick={() => setM(m + 1)}>m+1</button>
    </div>
  );
};
```

上面代码首次渲染也会触发 怎么让它第二次加载才触发呢

**自定义hook** 使得可以模拟组件的componentDidUpdate  即首次加载不触发 state更新才触发 

实现原理是 再加一个state 这个state由于计算n的变化次数 但这个state大于1时 说明n 更新

```javascript
// 上面第一次加载也会触发 怎么让它第二次加载才触发呢 用自定义hooks
const App = (props) => {
  const [n, setN] = useState(0);
  const useUpdate = (fn,array) =>{
    const [count, setCount] = useState(0)
    useEffect(()=>{
      setCount(x=>x+1)
    },array)
    useEffect(()=>{
      if(count>1){
        fn()
      }
    },[count])
  }
  useUpdate(()=>{
    console.log('模拟componentDidUpdate')
  },[n])
  return (
    <div>
      {n}
      <button onClick={() => setN(n + 1)}>+1</button>
    </div>
  );
};
```



#### 模拟componentWillUmmount

```javascript

// 模拟componentWillUmmount  useEffect 去return一个函数
const App = (props) => {
  const [childVisible, setChildVisible] = useState(true);
  const hide = () => {
    setChildVisible(false);
  };
  const show = () => {
    setChildVisible(true);
  };
  return (
    <div>
      {childVisible ? (
        <button onClick={hide}>hide</button>
      ) : (
        <button onClick={show}>show</button>
      )}
      {childVisible ? <Child /> : null}
    </div>
  );
};

const Child = (props) => {
  useEffect(() => {
    console.log('第一次渲染')
    return () => {
      console.log("Child 销毁了");
    };
  });
  return <div>Child</div>;
};
```



#### render

```javascript
// render  就是函数组件return 里面的东西  那么有多个结构且有不同逻辑怎么办
const X = ()=>{
	const n = 1 +1
  return (
  	<div>
    	{n}
    </div>
  )
}
const Y = ()=>{
	const n = 1 +1
  return (
  	<div>
    	{n}
    </div>
  )
}
const App = ()=>{
	return (
  	<>    // React Fragment 语法
    <X/> <Y/>
    </>
  )
}
```



#### shouldComponentUpdate

// 后面的React.memo 或者 useMemo