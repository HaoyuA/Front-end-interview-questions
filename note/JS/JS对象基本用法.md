### 对象：

#### 定义：

√ 无序的数据集合 

√  键值对的集合



#### 如何声明对象 

```javascript
let obj = { 'name': 'frank', 'age': 18 }
let obj = new Object({'name': 'frank'})
console.log({ 'name': 'frank, 'age': 18 })
```



#### 细节

**键名是字符串 键名是字符串 键名是字符串** 不是标识符 重要的事情说三遍

可以用 Object.keys(object) 来拿对象的键名集合

所以键名可以是任何字符

```javascript
let obj = {
  "":1,
  '中文':2,
  "👍":3,
  "1":2,
}
```

引号可以省略 省略就只能写标识符;  键名还是字符串 比如数字开头的就不能写

```javascript
let obj = {
  "1":2,
  1attr:3,
}
//Uncaught SyntaxError: Invalid or unexpected toke

let obj = {
  "1":2,
  '1attr':3,
}
```

因为上面的原因 会有一些奇怪的属性名

```javascript
let obj = {
  1: 'a',
  3.2: 'b',
  1e2: true,
  1e-2: true,
  .234: true,
  0xFF: true
};
Object.keys(obj) // values entries
=> ["1", "100", "255", "3.2", "0.01", "0.234"]
```

如何用变量做属性名

```javascript
let p1 = 'name'
let obj = { p1 : 'frank'} //这样写，属性名为 'p1'
let obj = { [p1] : 'frank' } //这样写，属性名为 'name'
obj[p1] = 'frank';
```

#### 删

- `delete obj.xxx`或 `delete obj ['xxx']`
- 即可删除 obj 的 xxx 属性
- 请区分「属性值为 undefined」和「不含属性名」

**不含属性名：**`'xxx' in obj === false`

**含有属性名，但是值为 undefined：**`'xxx' in obj && obj.xxx === undefined`



#### 查

**查看自身所有属性**： ` Object.keys (obj)`

**查看自身+共有属性：**`console.dir(obj)`

**查看所有值：**`Object.values(obj)`

**查看所有键值对：**`Object.entries(obj)`

判断一个属性是自身的还是共有的`obj.hasOwnProperty('toString')`

**查看属性**

两种方法查看属性

1. 中括号语法：`obj['key']`
2. 点语法：`obj.key`

坑新人语法：`obj [key] // 变量 key 值一般不为'key'`

而是key所指的值



#### 增 &&改

**直接赋值**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1704878/1595256798895-0868ebff-4121-4b6e-a42d-faf6cc263948.png?x-oss-process=image%2Fresize%2Cw_1500)

批量赋值(es6)

```
Object.assign(obj, {age: 18，gender: 'man' })
```

**修改或增加共有属性**

无法通过自身修改或增加共有属性（读的时候走原型，写的时候走自身）

- `let obj = {}, obj2 = {}//共有 toString `
- obj.toString = 'xxx' 只会在改 **obj 自身属性**
- obj2.toString 还是在原型上

**如果偏要修改或增加原型上的属性**

- `obj.__proto__.toString= 'xxx'//不推荐用__proto__`
- `0bject.prototype.toString = 'xxx'`

一般来说，不要修改原型，会引起很多问题



#### 改某对象的原型

**推荐使用 Object.create**

**![image.png](https://cdn.nlark.com/yuque/0/2020/png/1704878/1595258228384-b9d0e60a-e5d5-49e7-8b99-8624ccef6acc.png)**



#### 原型链

**每个对象都有原型**

- 原型里存着对象的共有属性
- 比如 obj 的原型就是一个对象
- obj. __ proto__存着这个对象的地址
- 这个对象里有 toString / constructor / valueOf 等属性

**对象的原型也是对象**

- 所以对象的原型也有原型
- obj= {}的原型即为所有对象的原型
- 这个原型包含所有对象的共有属性，是对象的根
- 这个原型也有原型，是 null



new X() 一共做了4件事：

1. 自动创建一个新对象
2. 把 这个新对象关联到原型 原型地址为X.prototype
3. 自动将空对象作为this关键字运行构造函数
4. 自动return this