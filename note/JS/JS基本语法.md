## JS语法

### 表达式与语句

#### 表达式

- 1+2 表达式的值为 3
- add (1,2) 表达式的值为函数的返回值(只有函数有返回值)
- console.log 表达式的值为函数本身

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1704878/1595156158337-8fcb9db6-495a-49d4-a93d-4fdd5fa63d81.png)

- console.log (3) 表达式的值为多少？

打印的是3，值是undefined

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1704878/1595156124356-b53426ad-9d3b-46a9-bdd3-c67976d0c33d.png)

#### 语句

var a = 1 是一个语句

#### 二者的区别

- 表达式一般都有值，语句可能有也可能没有
- 语句一般会改变环境（声明、赋值）

上面两句话并不是绝对的

#### 大小写敏感（不要写错）

var a 和 var A 是不同的

object 和 Object 是不同的

function 和 Function 是不同的

#### 空格

大部分空格没有实际意义

`var a = 1` 和 `var a=1` 没有区别

加回车大部分时候也不影响

只有一个地方不能加回车，那就是 return 后面（面试喜欢考）



### 标识符

#### 规则

第一个字符，可以是 Unicode 字母或$或_或中文

后面的字符，除了上面所说，还可以有数字

`var $1 = 'xxxx';`

后面的字符，除了上面所说，还可以有数字

#### 变量名是标识符

```javascript
var _ = 1
var $ = 2
var ____ = 6
var 你好 = 'hi'
```





### if语句

#### 语法

- If（表达式）{语句 1} else {语句 2}
- {}在语句只有一句的时候可以省略，**不建议这样做**



#### 推荐写法

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1704878/1595170297636-df23c356-4382-405e-bfc0-1a9b70210171.png)

#### switch语句（不推荐使用）

#### 语法

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1704878/1595170336172-3469c911-d23f-47f2-a8bf-998beba12016.png)

#### break的坑

- **大部分时候，省略 break 你就完了**
- 少部分时候，可以利用 break



### 问好冒号表达式/三元表达式

#### 语法

表达式1？表达式2:表达式3

#### &&

A && B && C && D

取第一个假值或 D

并不会取 true / false

#### ||

A || B || C || D

取第一个真值或 D

并不会取 true / false



### while循环

#### 语法

- While（表达式）{语句}
- 判断表达式的真假
- 当表达式为真，执行语句，执行完再判断表达式的真假
- 执行完再次判断表达式的真假





### for循环

for 是 while 循环的方便写法



#### 语法

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1704878/1595172098961-012162f7-740e-4b8f-b7e2-e12d7ea37cee.png)

1. 先执行语句 1
2. 然后判断表达式 2
3. 如果为真，执行循环体，然后执行语句 3
4. 如果为假，直接退出循环，执行后面的语句





### break 和 continue

break：退出所有循环（最近的循环）

continue：退出一次循环



### label 语句

#### 语法

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1704878/1595174615710-b16387ef-09bb-4c17-b611-4e0a8648027a.png)



用的很少，面试问到的概率5%

```
{
    foo: 1
}
```

问：上面的东西是什么？

答：foo是一个label，语句是1