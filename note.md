# why老师箴言

**TS对于很多类型的检测报不报错，取决于它的内部规则，而并不是说逻辑或者思想上的问题**

举个例子：

~~~typescript
interface IPerson {
  name: string
  age: number
}
const info: IPerson = {
  name: "why",
  age: 18,
  height: 1.88, // 从这里就已经开始报错了，因为IPerson接口没有指定height属性 
  address: "sd"
}
~~~

以下代码不报错：

~~~typescript
interface IPerson {
  name: string
  age: number
}
const p = {
  name: "why",
  age: 18,
  height: 1.88,
  address: "sd"
}
const info: IPerson = p
~~~

这个看似离谱的代码没报错，所以说，报不报错都是ts的规则而已

## TS学习的等级划分

1. 知道ts，但是没有用过
2. AnyScript，万物皆可any
3. 大多数使用any，但是普通的很多类型用法也是可以把握的
4. 大多数类型都是使用正确的，极少数使用any（业务开发）
5. 可以使用TS封装一些高级类型，包括框架当中某些特殊的类型（pinia/vuex）
6. 真正的TS融会贯通，阅读TS源码（TS开发者）

# plus：mac常用快捷键

## 工具类

* `shift + control + command + 4`：截图一个区域到粘贴板

## 编程类

* `commond + 方向键`：光标快速移动至（文件最上方/文件最下方/行末/行首）——（上/下/左/右）
* `option + ⬅️`：光标移动至单词的开头
* `option + ➡️`：光标移动至单词的末尾

* `shift + option + ➡️`：向右选中至当前行（词语）末尾
* `shift + option + ⬅️`：向右选中至当前行（词语）首

# TS设计背景

开发中的共识：**错误出现的越早越好(编写时>>编译时>>运行时)**，js不能进行类型检测（类型缺失），导致在运行时才能发现类型错误——>TypeScript：开发时进行类型检测

* ts不光为js带来类型检测，增加开发安全性；
* ts增加大型项目的可读性，可维护性

* 同时支持所有js语法，一些js新特性更是领先js的，所以是js的完全加强版

# 配置ts运行环境

`npm i typescript -g`

`tsc --version`

# Ts基本使用  

~~~typescript
let message: string = "hello"
/*
	1. 变量:类型
	2. string类型是typescript为我们提供的字符串类型，而String是js中的一个构造函数（类），所以一般使用string
*/
~~~

`tsc xxx.ts`：编译ts代码为js代码

`npm install tslib @types/node -g` 安装完这两个包之后就可以使用`ts-node xxx.ts`执行ts文件了

# 变量的声明

`var/let/const 标识符: 数据类型 = 赋值;`：变量后的数据类型又称之为“类型注解”（Type Annotation）

ts中不推荐使用`var`进行变量声明

## 类型推导

如果在变量声明时进行赋值，那么会自动进行类型推导：

~~~typescript
let name = "why"; // 自动推导出name应为string类型

name = 123; // 报错
~~~

* 如果`let`声明变量，推导出来的类型是通用类型
* 如果用`const`声明变量，推导出来的是字面量类型

~~~typescript
const height = 1.88; // height就是1.88类型
~~~

# js与ts的数据类型

ts是js的超集：

![image-20230109003154965](./image/ts是js的超集.png)

* `number`
* `boolean`
* `string`

## 数组类型

注意事项：真实的开发中，数组一般存放相同的数据类型（利于对数据进行统一处理）

~~~typescript
let name: string[] = ["abc", "cba", "nba"]; // 数组类型，且数组中存放字符串
/*
	等价写法
*/
let nums: Array<number> = [123, 321, 111]; // 数组类型，且数组中存放数字number
~~~

## Object类型

~~~typescript
let info: {
  name: string,
  age: number,
  height: number
} = {
  name: "why",
  age: 18,
  height: 1.88
}
~~~

当然也可以用`type`或者`interface`限制对象类型，后面再学

不要写：`let info: object = ...`，这样代表info是一个空对象（后面我们使用info时既不能访问内部属性，也不能设置属性）

## null && undefined

也是基本类型：

~~~typescript
let n: null = null;
let u: undefined = undefined;
~~~

## 函数的类型

### 函数参数

ts中定义一个函数时，需要明确的制定参数的类型：

~~~typescript
function sum(num1: number, num2: number) {
  return num1 + num2;
}
~~~

### 函数返回值

返回值类型可以明确制定，也可以自动进行类型推导

~~~typescript
function sum(num1: number, num2: number): number {
  return num1 + num2;
}
~~~

### 匿名函数的参数类型

一般来说**匿名函数的参数类型都会被上下文自动确定**，我们不要刻意去加类型注解：

~~~typescript
const name: string[] = ["avb", "sfdf", "hhh"];

// 不要写成：function(item: string, index: number, arr: string[])
names.forEach(function(item, index, arr) {
  ...
})
~~~

## 对象类型

~~~typescript
function printCoordinate(point: {x: number, y: number}) {
  console.log("x坐标：",point.x);
  console.log("y坐标：",point.y);
}
// 嫌point对象的类型注解太长了，可以用type起个别名，完全等价于：
type PointType = {x: number, y: number};
function printCoordinate(point: PointType) {
  console.log("x坐标：",point.x);
  console.log("y坐标：",point.y);
}
// 对象类型的注解中，属性之间也可以使用;进行分隔，例如：{x: number; y: number}，所以如果换行的话甚至可以不写分隔符（js特性：换行时自动补全分号）
~~~

### 可选属性

~~~typescript
function printCoordinate(point: {x: number, y: number, z?: number}) { ... } 
~~~

## any数据类型

any类型表示不限制变量的类型，并且**可以对该变量进行任意的操作（与unknown的区别）**，例如访问属性`.length`...（用any注解一个变量相当于ts中回到了js）

## unknown类型

表示不知道变量的类型（不对变量的类型进行限制），但是默认情况下不能对变量进行任何的操作（对其任何的操作都是非法的）：

~~~typescript
let foo: unknown = "aaa";
foo = 123; // 合法
console.log(foo.length) // 访问属性，非法
~~~

如果想操作（访问属性或者方法）unknown类型的变量，需要对其进行**类型缩小**，然后根据缩小后的类型进行对应（合法）的操作：
~~~typescript
let foo: unknown = "aaa";
if(typeof foo === "string") { // 使用typeof运算符进行类型缩小
  console.log(foo.length);
}
~~~

## void类型

出现场景：

TS中如果一个函数没有任何返回值，那么这个函数的返回值类型的类型注解就是`void`。（如果返回值是void类型，那么函数体中我们也可以显式`return undefined`，ts编译器允许这样做而已）

应用场景：

指定函数类型的返回值是void：

~~~typescript
type FooType = () => void;
const foo: FooType = () => {};
// 其实像foo函数，函数没有返回值，其实也没有必要去刻意指定其返回值为void，毕竟类型推导会指出其返回值为void

// 实际中的使用场景一般出现在：
type ExecFnType = (...args: any[]) => void;
function delayExecFn(fn: ExecFnType) {
  setTimeout(() => {
    fn("why", 18);
  }, 1000)
}
delayExecFn((name, age) => {
  console.log(name, age);
})
/*
	一个函数的参数为函数类型，我们一般会规定函数参数的类型，这时候会用到void
*/
~~~

注意（了解即可）：当基于上下文类型推导推导出返回类型为void的时候，并不会强制函数一定不能返回内容，就是说我们写`arr.forEach()`时，提示函数参数应为`(item: xxx, index: number, this: xxx) => void`，这里的void就是推导出来的，我们可以在函数体中`return`。

## never类型（极不常见）

应用场景：

1. 开发中很少实际去定义never类型，某些情况下会自动进行类型推导出never（死循环函数、throw Error的函数...）

2. 开发框架（工具）的时候可能会用到never

~~~typescript
function handleMessage(message: string | number) {
  switch(typeof message) {
      case: "string":
      	console.log(message.length);
      	break;
      case: "number":
      	console.log(message);
      	break;
      /*
      	handleMessage是我们封装的一个工具函数，其正常使用过程永远也不会进入到default逻辑中，但是如果哪一天想要开发这个函数本身，我们拓展了message参数的类型，比如message: string | number ｜ boolean，但是我们函数体中并没有对应处理布尔类型的逻辑，这样就会走到default中，never类型的变量被赋值，就会报错，提示开发人员添加相关处理逻辑
      */
    	default:
      	const check: never = message;	
  }
} 
~~~

## tuple（元组）类型

数组中存放的数据类型不同，且其每一项都有明确的类型限制：

~~~typescript
const info: [string, number, number] = ["why", 18, 1.88];
~~~

使用场景：

作为函数的返回值类型（某些函数返回一个数组，比如数组的第一项为字符串，第二项为一个函数）：

~~~typescript
function example(params: xxx): [str: string, (newValue: number) => void] {
  ...
  let str = "..."
  function setValue(newValue: number) {
    ...
  }
  return [str, setValue];
}
~~~

# 联合类型 && 交叉类型

## 联合类型

用`|`来连接类型从而构造出的新类型，因为变量可能是其中的任何一种类型，所以一般需要搭配类型缩小推断出更加具体的类型

~~~typescript
function printId(id: number | string) {
  if(typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id);
  }
}
~~~

## 交叉类型

`&`连接多个类型，表示要同时满足多个类型

应用场景：用`&`来组合对象类型：

~~~typescript
interface IKun {
  name: string
  age: number
}
interface ICoder {
  name: string
  coding: () => void
}

const jrd: IKun & ICoder = {
  name: "jrd",
  age: 18,
  coding: function() {
    console.log("coding");
  }
}
~~~

# type && interface

## 类型别名type

~~~typescript
// 赋值的方式定义一个type
type PointType = { x: number, y: number, z?: number };
function printCoordinate(point: PointType) {
  console.log(point.x, point.y, point.z);
}
~~~

type也可以给基本类型起别名：`type MyNumber = number`

## 接口声明

~~~typescript
// 直接声明一个接口
interface PointType {
  x: number
  y: number
  z?: number
}
function printCoordinate(point: PointType) {
  console.log(point.x, point.y, point.z);
}
~~~

**两者在定义对象类型的变量时，基本没有区别**

## type与interface的区别

* type类型使用范围更广，type可以声明任何类型；但是接口类型只能用来声明对象

* type不允许相同名称的别名同时存在；interface可以多次声明同一个接口名称，相当于对接口内容的叠加

~~~typescript
type PointType1 = {
  x: number
  y: number
}
type PointType1 = { // 重复定义type PointType1，报错
  z?: number
}

interface PointType2 {
  x: number
  y: number
}
interface PointType2 {
  z: number
}
const point: PointType2 = { // 报错：缺少了z属性
  x: 100,
  y: 200,
}
~~~

* interface支持继承

~~~typescript
interface IPerson {
  name: string
  age: number
}
interface IKun extends IPerson {
  kouhao: string
}
const ikun1: Ikun = {
  kouhao: "你干嘛，哎呦",
  name: "jrd",
  age: 20
}
~~~

* interface可以被类实现（后期ts面向对象时再学）

~~~typescript
class Person implements IPerson {
  ...
}
~~~

### 总结：

**如果是非对象类型的定义用type**，毕竟使用范围更广，可以定义的类型更多；**如果是对象类型的声明那么使用interface**，因为可以重复声明，还有一些其他特性等，可拓展性更强

但是，其实即使对于对象类型的定义，两者也是随便使用的，毕竟不要忘了ts的核心作用就是在开发阶段，编译阶段给我们做类型限制。两者都能无差别的达成这一目的。

# 类型断言（强制指定类型）

例子：

（其实下面这个例子就是类型断言的核心所在：**通过断言方便我们去使用变量**）

~~~typescript
const imgEl = document.querySelector(".img") as HTMLImageElement; // 如果我们不把imgEl断言为HTMLImageElement，那么默认它的类型就是Element ｜ null——我们操作其属性时（比如imgEl.src="xxx"）就需要进行类型缩小等操作
imgEl.src = "xxx";
imgEl.alt = "yyy";
~~~

断言规则：断言为更加具体的类型或者不太具体的类型（any/unknown）类型：

~~~typescript
const age: number = 18;
const age2 = age as string; // 把number断言成string，报错

// 以下代码从ts类型检测的角度来说是正确的，但是代码本身逻辑有问题（迷迷糊糊，好像没啥实际用）——TSbug
const age3 = age as any; // 把具体类型断言为不太具体的类型any
const age4 = age3 as string; // 把any断言为具体的类型string
~~~

# 非空断言

## plus知识：`?.`——可选链运算符



---

https://cloud.tencent.com/developer/article/2073555

**可选链**操作符( **`?.`** )允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。`?.` 操作符的功能类似于 `.` 链式操作符，不同之处在于，在引用为空([nullish](https://developer.mozilla.org/zh-CN/docs/Glossary/Nullish) ) (`[null](<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null>)` 或者 `[undefined](<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined>)`) 的情况下不会引起错误，该表达式短路返回值是 `undefined`。与函数调用一起使用时，如果给定的函数不存在，则返回 `undefined`。

语法：

~~~js
obj?.prop
obj?.[expr]
arr?.[index]
func?.(args)
~~~

---

可选链只能用来访问属性（或者调用方法），但是不能用在赋值表达式的左边：

~~~typescript
type IPerson = {
  name: string,
  age: number,
  friend?: {
    name: string
  }
}

const info: IPerson = {
  name: "why",
  age: 18
}

console.log(info.friend?.name); // undefined

info.friend?.name = "jrd"; // 报错，可选链.?不能用在赋值表达式左侧

/*
	解决方案：
		1. 类型缩小(类型更精细)
*/
if(info.friend) {
  info.friend.name = "kobe";
}
~~~

解决方案2——非空断言`!`（确保某个变量一定是有值的）：

~~~typescript
info.friend!.name = "james"; // 有点危险，只有确保friend一定有值的情况下才能使用
~~~

# 字面量类型

字面量类型就是某个基本类型的具体值作为类型，比如一个具体的字符串或者一个具体的数字，例如：

~~~typescript
const name: "why" = "why";
let age: 18 = 18;
// 单个字面量没啥意义，一般将多个字面量联合起来
type Direction = "left" | "right" | "up" | "down";
const d1: Direction = "left"; // Direction类型的变量只能取四个值中的一个
~~~

例子：

~~~typescript
// 封装请求方法：
type MethodType = "get" | "post";
function request(url: string, method: MethodType) {
  ...
}
request("http://localhost", "post");

const info = {
  url: "xxx",
  method: "post",
}
request(info.url, info.method); // 报错：info.method是一个string类型，而request的第二个参数是一个字符串类型的字面量，说白了就是一个字面量类型，自然类型不匹配

// 解决方案1:类型断言——将string类型断言为更具体的字符串字面量
request(info.url, info.method as "post");
  
// 解决方案2:直接让我们的info对象类型是一个字面量类型
const info2: { url: string, method: "post" } = {
  url: "xxx",
  method: "post"
}
request(info2.url, info2.method);
~~~

## as const

让一个对象的所有属性都变成字面量类型

~~~typescript
// 续上：解决方案3:as const
const info3 = {
  url: "xxx",
  method: "post",
} as const; // 此时info3的类型：{ url: "xxx", method: "post" }

request(info3.url, info3.method);  // 这里info3.url是一个字符串字面量类型，request的第一个参数是string类型，这样把一个更精细的值赋值给宽泛的值是ok的
~~~

# 类型缩小

定义：

Type Narrowing，也译作类型收窄

我们可以通过类似于`typeof padding === "number"`的判断语句，来改变ts的执行路径

在给定的执行路径中，我们可以缩小比声明时更小的类型，这个过程称之为缩小（Narrowing）

而我们编写的`typeof padding === "number"`可以称之为**类型保护（类型缩小的逻辑判断语句）**

常见的类型保护：

* `typeof`
* 平等缩小：`===`/`!==`
* `instanceof`
* `in`
* ...

~~~typescript
// 1. typeof：使用最多
function printID(id: number | string) {
  if(typeof id === "string") {
    console.log(id.length, id.split(""));
  }else {
    console.log(id);
  }
}

// 2. ===平等缩小：方向的类型判断
type Direction = "left" | "right" | "up" | "down";
function switchDirection(direction: Direction) {
  if(direction === "left") {
    console.log("left相关逻辑");
  }else if(direction === "right") {
    console.log("right相关逻辑");
  }
  ...
}
  
// 3. instanceof：日期类型的判断
function printDate(data: string | Date) {
  if(date instanceof Date) {
    console.log(date.getTime());
  } else {
    console.log(date);
  }
}

// 4. in——判断对象身上有无某个属性方法
interface ISwim {
  swim: () => void
}
interface IRun {
  run: () => void
}
function move(animal: ISwim | IRun) {
  if("swim" in animal) {
    animal.swim();
  }else if("run" in animal) {
    animal.run();
  }
}
const fish: ISwim = {
  swim: function() {},
}
const dog: IRun = {
  run: function() {},
}
move(fish);
move(dog);
~~~

move方法的错误写法：

~~~typescript
function move(animal: ISwim | IRun) {
  if(animal.swim) { // 报错，因为animal为联合类型，类型不确定，不可以直接访问属性（访问属性这个行为从ts语法上出错了）
    animal.swim();
  }
  ...
}
~~~

换句话说，`in`运算符可以避开`.`运算符访问对象属性的行为，这个行为是会被ts进行检查的；ts没有针对in的特殊检查，所以可以判断对象是否有某个属性

上面的类型保护会自动进行类型缩小，但有些情况下需要我们用**类型谓词`is`**手动进行类型缩小，场景：

```ts
// 判断参数是否为string类型, 返回布尔值
function isString(s:unknown):boolean{
  return typeof s === 'string'
}

// 参数转为大写函数
// 直接使用转大写方法报错, str有可能是其他类型
function upperCase(str:unknown){
  str.toUpperCase()
  // 类型“unknown”上不存在属性“toUpperCase”。
}

// 判断参数是否为字符串,是在调用转大写方法
function ifUpperCase(str:unknown){

  if(isString(str)){
    str.toUpperCase()
    // (parameter) str: unknown
    // 报错:类型“unknown”上不存在属性“toUpperCase”
  }
}
```

也就是说虽然我们的isString方法内部对于参数str进行了类型判断，并且给出了一个布尔类型的返回参数，但是在`ifUpperCase`函数中并没有对str的类型进行缩小，str依然是原本的unknow类型，**对于这种类型判断函数，我们希望除了给出一个布尔类型的判断结果之外还能对传入的参数进行类型缩小，这就需要用到类型谓词is：**

```ts
function isString(s:unknown):s is string{ // s is string表示：函数返回类型为布尔类型，如果为true，那么参数s自动类型缩小为string
  return typeof s === 'string'
}

function ifUpperCase(str:unknown){

  if(isString(str)){
    str.toUpperCase()
    // (parameter) str: string
  }
}
```

# 函数类型

## 函数类型表达式

函数本身也是一个变量（标识符），所以本身也可以有类型限制（函数类型表达式）：

~~~typescript
type BarType = (num1: number) => number; // 函数类型表达式
const bar: BarType = (arg: number): number => {
  return 123;
}
~~~

**函数类型表达式中，参数列表中，参数的名称不能省略（如上的num1）**，某些语言中可以只写变量类型，但是TypeScript不可以！！！省略之后表达的意思就变了：

~~~typescript
type BarType = (number) => number; // 这样省略参数名其实意思是：(number: any) => number
~~~

### 函数类型参数的个数不进行校验

ts对于传入的函数类型的参数个数不进行检测：

~~~typescript
type CalcType = (num1: number, num2: number) => number;
function calc(calcFn: CalcType) { // calc接收的函数从函数类型表达式CalcType来看，应该接受2个参数
  calcFn(10, 20);
}
calc(function(){ // 这里传给calc的函数一个参数都没有，但是不报错，原因如上⬆️
  return 123;
})
~~~

其实很好理解：

`forEach`接收的函数也提供了三个参数：item、index以及this，但是我们也经常只使用item一个参数呀，所以说ts对函数的参数个数进行检测本来就是不合理的，那将会让ts非常难用

## 调用签名(参数列表后是冒号)

### 调用签名

从对象的角度来看一个函数，函数变量本身也是一个对象，也可以有其他属性（**axios本身是一个函数，但是他身上挂载了axios.get等很多属性、方法**），这样我们定义一个接口让函数这个对象去实现，这样就给函数对象本身增加了一些属性，但是为了保证函数的可运行性，我们需要给接口中添加一个调用签名：

~~~typescript
// 1. 函数类型表达式
type BarType = (num1: number) => number
// 2. 函数的调用签名
interface IBar {
  name: string
  age: number
  (num1: number): number. // 函数的调用签名格式——>（参数名：参数类型, ...): 函数返回值
}

const bar: IBar = (num1: number): number => {
  return 123;
}

bar.name = "aaa";
bar.age = 18;
~~~

### 构造签名（了解）

~~~typescript
class Person { // Person是一个类，自然也是一个构造函数
}

interface IContructorPerson { // 实现了这个接口的函数，可以用new来进行调用（是一个构造函数），并且函数体返回一个Person类实例
  new (): Person
}

function factory(fn: IContructorPerson) {
  const f = new fn();
  return f;
}

factory(Person); // Person构造函数肯定可以用new进行调用，并且返回一个Person实例（完全契合IContructorPerson接口）
~~~

## 可选参数

见上面：函数类型—函数类型表达式—函数类型参数的个数不进行校验 其实是**一个函数如果只有函数类型表达式进行限制，那么他的参数个数是不进行限制的**。但是如果在函数声明时定义的参数列表，那么每个参数默认都是必选参数：

~~~typescript
function foo(x: number, y: number) {
  
}

foo(10); // 报错：函数声明时的两个参数都是必须的
~~~

可选参数：

~~~typescript
// 这样y就是可选的，但是如果函数调用时不传y，那么其就是undefined类型，所以可选参数？符就代表参数的定义的类型与undefined的联合类型
// 等价于 y: unmber | undefined
function foo(x: number, y?: number) {
  if(y !== undefined) {
    console.log(y + 10);
  }
}
foo(10);
~~~

## 默认参数

1. 函数的参数可以有默认值，在有默认值的情况下参数的类型注解可以省略
2. 有默认值的参数，可以接收一个undefined值（ts特点，没啥实用性知道就行）

~~~typescript
function foo(x: number, y = 100) {
  console.log(y + 10);
}
foo(10);
foo(10, undefined); // 没问题
~~~

## 剩余参数

说白了就是js中函数的参数列表写一个数组的解构，然后调用时所有的参数都放入这个数组中：

~~~typescript
function sum(...nums: (number | string)[]) {
  
}
foo("anv", 123, "cba");
~~~

## 函数的重载

举例：我们希望实现add函数可以把两个数字或者两个字符串进行相加：

~~~typescript
function sum(a1: number, a2: number): number; // 函数重载签名
function sum(a1: string, a2: string): string;
function sum(a1: any, a2: any): any { // 有实现体的通用函数
  return a1 + a2;
}
console.log(sum(10, 20));
console.log(sum("aaa", "bbb"));
~~~

ts函数重载的语法构成：

若干个函数重载签名和一个有实现体的通用函数，当我们调用函数时，ts根据我们传入的参数类型来决定执行函数体时，到底执行哪一个函数重载签名（有实现体的函数，是不能被直接调用的）

平时业务开发基本用不到，一般出现在封装一些通用工具的场景中

## 联合类型 && 函数重载的选择

需求：定义一个函数，传入字符串或者数组，返回他们的长度：

~~~typescript
// 1. 使用联合类型来实现
function getLength(a: string | any[]) {
  return a.length;
}
// 2. 使用函数重载来实现
function getLength(a: string): number;
function getLength(a: any[]): number;
function getLength(a: any) {
  return a.length;
}
~~~

**选择原则：如果需求能使用联合类型来实现，尽量选择联合类型来实现**

# TS中的this

在默认情况下（没有设置ts的配置文件this相关），`this`默认是`any`类型

项目目录下执行`tsc --init`初始化ts配置文件（ts语法检测的一些规则）——`tsconfig.json`

发现生成`tsconfig.json`之后，this就出问题了：函数中的this会报错：

~~~typescript
function foo() {
  console.log(this); // 报错，因为this指向谁不明确
}
~~~

`tsconfig.json`中有一个this相关的配置项：`"noImplicitThis": true`（implicit：模糊的，隐含的），也就是说不允许ts中的this指向不明，所以上面的代码就报错了。

解决方案：

1. 设置`tsconfig.json`——`"noImplicitThis": false`
2. 在普通函数中（如上报错的函数），我们需要在函数体的参数列表中，**第一个参数明确指定this及其类型**，然后在调用方法时使用`call`函数，call的第一个参数明确指定this：

~~~typescript
function foo(this: { name: string }, info: { name: string }) {
  console.log(this, info); // 这样this就有明确的类型了，ts不报错
}
foo.call({ name: "why" }, { name: "kobe" }); // 输出：{ name: "why" }, { name: "kobe" }
~~~

# this相关的ts内置工具

## ThisParameterType< T >

获取T类型中的this的类型：

~~~typescript
function foo(this: { name: string }, info: { name: string }) {
  console.log(this, info);
}
type FooType = typeof foo; // type FooType = (this: {name:string}, info: {name: string}) => void
type FooThisType = ThisParameterType<>; // type FooThisType = { name: string }
~~~

## OmitThisParameter< T >

去除T类型中的this之后的函数类型：

~~~typescript
function foo(this: { name: string }, info: { name: string }) {
  console.log(this, info);
}
type FooType = typeof foo; // type FooType = (this: {name:string}, info: {name: string}) => void
type pureFooType = OmitThisParameter<FooType>; // type pureFooType = (info: {name:string}) => void
~~~

## ThisType< T >

一个对象a（A类型），a里面除了各种属性外，有很多的方法，这些方法的函数体中都用到了this变量，这时（基于配置了`"noImplicitThis": true`，this需要明确指明的情况）我们定义这些函数时第一个参数都需要用来手动指定this的类型，为了简化这种重复的操作，这时候我们可以让对象a的类型为`A & ThisType<希望绑定给方法的this的类型X>`，这样就相当于给a对象的所有方法里的this都指定了`X`类型：

~~~typescript
interface IState {
  name: string
  age: number
}
interface IStore {
  state: IState
  eating: () => void
  running: () => void
}
const store: IStore & ThisType<IState> = {
  state: {
    name: "jrd",
    age: 18,
  },
  eating: function() {
    console.log(this.name);
  },
  running: function() {
    console.log(this.age);
  }
}
~~~

等价于：

~~~typescript
interface IState {
  name: string
  age: number
}
interface IStore {
  state: IState
  eating: () => void
  running: () => void
}
const store: IStore = {
  state: {
    name: "jrd",
    age: 18,
  },
  eating: function(this: IState) {
    console.log(this.name);
  },
  running: function(this: IState) {
    console.log(this.age);
  }
}
~~~

解析：毕竟在`"noImplicitThis": true`的情况下，函数中使用this都是需要明确指定this的类型的，eating函数和running函数我们希望IState类型的变量去调用，所以我们第一个参数需要手动指定`this: IState`，但是`store: IStore & ThisType<IState>`之后，即store变量的类型添加上`& ThisType<IState>`之后，store的属性方法的this就都指定为`IState`类型了

Pinia的源码里就是这么操作的

# TS面向对象

其实当下的前端开发中，面向对象的编程其实用的越来越少了，vue3以及react函数式组件结合hook的开发模式，大家都更倾向于函数式编程（更加灵活）。

## 成员属性必须先在class中声明，不然直接constructor中this访问属性会报错：

~~~typescript
class Person {
  name: string; // 声明成员属性
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}
export {}; // 不写会报错Person命名冲突（具体和谁冲突不清楚，可能是tsc生成的js文件中的Person？）
~~~

 

---

plus：export暴露变量之后ts文件就会变成一个模块，变量方法等都属于这个模块（形成一个独立的命名空间）

~~~typescript
/**
 * 变量名冲突问题
 */
let name = 'tom'; // 默认情况下，无法使用变量名name等，与全局对象window的name属性出现了重名

export {}; // 解决：使用export将文件声明为一个模块module，变量被限制在当前模块作用域下，不会再产生冲突
~~~

---



成员属性必须进行初始化，constructor中初始化可以，也可以属性声明时进行初始化（这样其实就不用进行类型注解了）：

~~~typescript
class Person {
  name = "";
  age = 0;
}
export {};
~~~

或者不想初始化，用非空断言(逃避ts语法检查罢了hhh)：

~~~typescript
class Person {
  name!: string;
  age!: number;
}
export {};
~~~



## 成员修饰符

`public`、`private`、`protected`写在成员属性声明或者成员方法的定义之前，给类成员添加访问权限：

* `public`：默认属性，可任意访问
* `private`：私有属性，只有在类本身的内部（class定义体内部）才能访问
* `protected`：保护类型：只有在类本身以及子类的内部才能访问

* 针对成员属性`readonly`修饰符：只读属性

~~~typescript
class Person {
  readonly name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}
const p = new Person("why", 18);
p.name = 20; // 报错：只读属性不能修改
export {}; 
~~~



## 私有属性的getter和setter

~~~typescript
class Person {
  private _name: string // 私有属性，属性前面使用 "_"
	constructor(name: string) {
    this._name = name;
  }
  running() {
    console.log("running:", this._name);
  }
  // 对私有属性的访问与修改进行拦截
  set name(newValue: string) {
    this._name = newValue;
  }
  get name(): string {
    return this._name;
  }
}
~~~



## 参数属性（语法糖：属性声明 + 构造函数赋值）

构造函数的参数列表里写修饰符 等价于 成员属性声明 + 构造函数内部进行赋值

~~~typescript
class Person {
  constructor(public name: string, private age: number, readonly height: number) {}
}
const p = new Person("why", 18, 1.88);
~~~



## 抽象类abstract

抽象类中可以定义抽象方法（没有函数实现），抽象类供其他类继承，子类要实现抽象类中的抽象方法：

~~~typescript
abstract class Shape {
  abstract getArea();
}

class Rectangle extends Shape {
  constructor(public width: number, public height: number) {
    super();
  }
  getArea() {
    return this.width * this.height;
  }
}
~~~



## TS类型检测——鸭子类型

TypeScript对于类型检测的时候使用鸭子类型

鸭子类型：如果一只鸟，走起来像鸭子，游起来像鸭子，看起来像鸭子，那么我们可以认为他就是一只鸭子

ts鸭子类型：只关心属性和行为，而不关心具体是不是对应的类型：

~~~typescript
class Person {
  constructor(public name: string, public age: number) {}
  running() {}
}
class Dog {
  constructor(public name: string, public age: number) {}
  running() {}
}
function printPerson(p: Person) {
  console.log(p.name, p.age);
}

printPerson(new Person("jrd", 18));
printPerson({name: "kobe", age: 30, running: function() {}}); // 通过（鸭子）类型检测
printPerson(new Dog("旺财", 3)); // 通过类型检测
~~~



## 类具备的类型特性

类的作用：

1. 可以创建对应的实例对象
2. 类本身可以作为其实例对象的类型
3. 类也可以当作一个有构造签名的函数

~~~typescript
class Person {}
// 类作为其实例的类型
const p: Person = new Person();
function printPerson(p: Person) {}

// 当作一个有构造签名的函数
function factory(ctor: new () => void) {}
factory(Person)
~~~



# 对象类型的属性修饰符

* 可选属性？
* 只读属性readonly

~~~typescript
interface IPerson {
  name: string
  age?: number
  readonly height: number
}
~~~





# 索引签名

限制一个对象的属性访问方式（索引的类型）以及属性值的类型

~~~typescript
interface ICollection {
  [index: string]: number // 表示此接口类型的对象必须满足：通过string类型的索引访问属性，访问到的属性为number类型
  length: number // 有一个length属性，其属性值必须是number类型（也受到上面索引签名的约束，所以lenght也必须是number类型）
}

function iteratorCollection(collection: ICollection) {
}

iteratorCollection({ name: 111, age: 18, legtn: 10 });
~~~

解释索引签名中的奇怪现象：

~~~typescript
interface IIndexType {
    // 索引签名
    [index: string]: string
}

// 索引签名：[index: number]: string
// const names: IIndexType = ["abc", "cba", "nba"]; // 不报错

// [index: string]: any
// const names: IIndexType = ["abc", "cba", "nba"]; // 不报错

// [index: string]: string
const names: IIndexType = ["abc", "cba", "nba"]; // 报错：应该是因为["abc", "cba", "nba"]为新鲜变量进行严格类型检测的原因，毕竟["abc", "cba", "nba"]中还有很多方法，比如forEach等，他们的返回值并不是string，这也就是上面any为什么不报错的原因，因为概括了各种方法的返回值
~~~

plus：索引签名的index类型只能是string或者number，两者的联合类型也不行

plus：还有一些奇奇怪怪的规则，不整理了——day3上午3：28左右的部分



# 接口的继承特性

`extends`可以从其他接口中继承过来属性

作用：

1. 减少了相同代码的重复编写
2. 自定义接口时，希望自定义的接口拥有某个类型（第三方库提供的接口）的所有属性，使用继承

~~~typescript
interface IPerson {
  name: string
  age: number
}

interface IKun extends IPerson {
  slogan: string
}

const ikun: IKun = {
  name: "why",
  age: 18,
  slogan: "niganma,aiyou"
}
~~~





# 接口可以被类实现

~~~typescript
interface IKun {
  name: string
  age: number
  slogan: string
  playBasketball: () => void
}

interface IRun {
  running: () => void
}

class Person implements IKun, IRun {
  name: string
  age: number
  slogan: string
  playBasketball() {}
  running() {}
}
~~~





# TS中严格字面量赋值检测

当一个变量第一次被定义时，那么ts认为它是fresh新鲜的，ts将对新鲜的变量进行严格字面量类型检测：变量必须完全满足类型的要求（该有的属性必须要有，**而且不能有多余的属性**）：

~~~typescript
interface IKun {
    name: string,
    age: number,
}

let str = "";
let obj = {
    name: "jrd",
    age: 20,
    height: 1.75,
}

let ikun: IKun = obj; // 不报错：obj已经不是新鲜变量了，它满足了IKun的所有属性要求，多了一个height属性没事
let ikun2: IKun = { // 不报错：通过严格类型检测
    name: "why",
    age: 18,
}
let ikun3: IKun = { // 报错：ikun3首次出现，进行严格类型检测，比IKun接口多了一个height属性
    name: "why",
    age: 18,
    height: 1.88,
}
let ikun4: IKun = str; // str不是新鲜变量，但不代表就不进行类型检测了，只是说此时类型检测的标准是只要满足拥有接口该有的属性即可，str不满足IKun接口，报错
~~~





# 抽象类与接口的区别

相关辨析js中其实不太重要，其他面向对象语言中比较重视

从理解上来说：

​	抽象类是对一类事物的抽象，表达的是is a的关系，如：猫is a动物（抽象类）

​	接口是对某一种行为的抽象，表达的是has a的关系，如：猫has a爬树的技能（接口）、

从语法层面上来讲：

​	抽象类只能被单一继承，接口可以被多重实现

​	抽象类中可以有方法的实现体，接口中只能有函数的声明



# ts枚举类型

ts增加的新特性而已，用一些常量或者联合类型也能实现效果，当然也可以使用枚举类型：

~~~typescript
enum Direction { // 定义了枚举类型Direction
  LEFT, // 默认值为0
  RIGHT // 默认值为1 
}

const d1: Direction = Direction.LEFT; 
function turnDirection(direction: Direction) {
  switch(direction) {
    case Direction.LEFT:
    case Direction.RIGHT:
      ...
  }
}
~~~

一些源码中喜欢给枚举类型赋值时使用位运算（方便功能相 ｜ 吧）：

~~~typescript
enum Operation {
  Read = 1 << 0, // 1左移0位，值为1
  Write = 1 << 1, // 1左移1位，值为2
  foo = 1 << 2 // 1左移2位，值为4
}
~~~

## 应用实例



**总结：enum对象完全可以理解为一个普通js对象，但是我们可以省略写属性值，它自动从0依次递增赋值，我们也可以不省略，即给枚举属性赋值，那么就完全和给对象的属性赋值一样了（不同的是对象`key: value`而枚举`key = value`）**

~~~typescript
enum Color {
  Red, // 0
  Green, // 1
  Blue // 2
}

enum Color {
  DarkRed = 3, // 3
  DarkGreen, // 4
  DarkBlue // 5
}

enum MyEnum {
  First = "First String",
  Second = "Second String",
  Third
}
console.log(MyEnum.Third); // 输出 2
~~~



# 泛型编程（ts中的真正难点）

泛型——类型的参数化

基本使用：

~~~typescript
function bar<Type>(arg: Type): Type {
  return arg;
}

// 完整的写法：
const res1 = bar<number>(123);

// 省略的写法：
const res2 = bar("aaaaa"); // res2能正确推导出来为string类型 
~~~

支持多个泛型参数：

~~~typescript
function foo<T, E>(a1: T, a2: E) {}
~~~

开发中常用的泛型参数名称：

* T：Type的缩写，类型
* K，V：key与value
* E：Element的缩写，元素
* O：Object的缩写，对象

## 泛型接口

~~~typescript
interface IKun<T = string> { // 定义接口时使用泛型，并且指定默认值（给函数参数的默认值一样，这里指定类型的默认值）
  name: T
  age: number
  slogan: T
}
~~~

## 泛型类

~~~typescript
class Point<Type = number> {
  x: Type
  y: Type
  constructor(x: Type, y: Type) {
    this.x = x;
    this.y = y;
  }
}
~~~

## 泛型约束

基本语法：

<T extends InterfaceB>表示传入的类型必须满足B的要求，也可以有其他属性，但是至少要满足B

需求我们想实现一个函数getInfo，获取传入的内容，内容必须有length属性：

~~~typescript
interface ILength {
  length: number
}

function getInfo<Type extends Ilength>(args: Type): Type { // Type相当于类型变量，用于记录本次函数调用的参数的类型，所以在整个函数的执行周期中，一直保留着参数的类型
  return args;
}

const info1 = getInfo("aaaa"); // 最后返回的结果info1就是"aaaa"的类型string，同时也满足了extends指定的约束
const info2 = getInfo(["aaa", "bbb", "ccc"]); 
const info3 = getInfo({length: 100});
~~~



泛型参数搭配keyof添加约束实例：

~~~typescript
// plus——keyof的使用：keyof O：keyof后跟一个对象类型，返回结果为对象类型所有属性的类型的联合类型
interface IKun {
  name: string
  age: number
}
type IkunKeys = keyof IKun // type IKunKeys = string | number

// 获取对象的某个key的值
function getObjectProperty<O, K extends keyof O>(obj: O, key: K) {
  return obj[key];
}

const info = {
  name: "why",
  age: 10,
  height: 1.88
}

const name = getObjectProperty(info, "name");
~~~



# 映射类型（Mapped Types）

业务开发用不到，应用于源码以及通用工具开发中

暂时跳过：day3，下午，2:27

## 基本使用



# 内置类型工具和类型体操

ts的目的是为js添加一套类型校验系统，因为js本身的灵活性，所以ts不得不增加更多的功能来适应js的灵活性——ts是一种支持类型编程的类型系统

类型体操地址：https://github.com/type-challenges/type-challenges

类型体操解析地址：https://ghaiklor.github.io/type-challenges-solutions/en/

（不要舍本逐末，ts只是给js添加类型约束的工具）



# TS知识拓展



## ts模块化

### 基本使用与注意事项

TypeScript认为什么是一个模块：

* **js规范中声明任何没有export的js文件都应该是一个js脚本，而非一个模块**
* 在一个js脚本中，变量和类型会被声明在共享作用域（直观上会造成命名冲突等问题）

如果我们有一个文件，没有任何的import或者export，但是我们希望他被当作一个模块进行处理，添加代码：

~~~typescript
export {}
~~~

这会把文件改成一个没有导出任何内容的模块（生成js/ts模块的操作）。



ts模块导入类型注意事项：

`type.ts`：

~~~typescript
export interface IPerson {
  name: string
  age: number
}

export type IDType = number | string
~~~

`index.ts`:

~~~typescript
import { type IDType, type IPerson } from "./type.ts"; // 在一个模块中导入类型（接口），推荐在类型前面加上type关键字；如果{}中全是类型或者接口，可以把type关键字提取到{}之前即：import type { IDType, IPerson } ...,这样做的目的是方便非ts编译器，比如babel等可以快速移除不相关的导入，提高编译性能

const id1: IDType = 111;
const p: IPerson = { name: "why", age: 18 };
~~~



### 命名空间（了解）

ts自己的一种模块模式，也就是namespaces（命名空间），他是在es模块标准之前出现的

ts官方文档（对命名空间的态度）：

虽然命名空间没有被废弃，但是由于es模块已经拥有了命名空间的大部分特性，因此**更推荐使用es模块**，这样才能与js的发展方向保持一致

`format.ts`：

~~~typescript
namespace price {
  function format(price) {
    return "¥" + price;
  }
  const name = "price";
}

namespace date {
  function format(dateString) {
    return "2022-10-10";
  }
  const name = "date";
}
~~~

这样的话因为没有export，format.ts中的两个命名空间被当作全局变量，其他文件中自然可以直接访问price以及date，但是一般我们这样写：

把命名空间以及命名空间中想提供的变量都进行export：

~~~typescript
export namespace price {
  export function format(price) {
    return "¥" + price;
  }
  export const name = "price";
}

export namespace date {
  export function format(dateString) {
    return "2022-10-10";
  }
  export const name = "date";
}
~~~

`index.ts`：

~~~typescript
import { price, date } from "./format";

price.format("1111");
date.format("2222");
~~~



## 类型查询

现象引入：

ts中我们可以直接执行如下代码，并且document对象、getElementById方法等都有明确的类型限制：

`const imageEl = document.getElementById("image") as HTMLImageElement;`

### `.d.ts`文件

ts的类型声明文件，这类文件中不写逻辑代码，只用来定义接口以及type

ts在执行时会在如下.d.ts文件中查找我们的类型声明：

* 内置类型声明：我们npm安装ts时就被下载下来了，上面的document等就属于这类文件定义的
* 外部定义的类型声明：第三方库，比如axios等等
* 自定义的类型声明



利用webpack搭建一个自动编译的ts运行环境

`./webpack环境/ts-run-envirenment`：

`npm init -y`

`npm i webpack webpack-cli -D`

`./webpack环境/ts-run-envirenment/webpack.config.js`：

~~~js
module.exports
~~~

webpack相关插件：

`npm install html-webpack-plugin -D`&&创建`/index.html`(根目录下)

`npm i webpack-dev-server -D`（开启本地服务）

`npm i ts-loader`

配置package.json:

~~~js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
    mode: "development",
    entry: "./src/index.ts",
    output: {
        path: path.resolve(__dirname, "./dist"),
        filename: "bundle.js"
    },
    resolve: {
        extensions: [".ts", ".js", ".cjs", ".json"]
    },
    module: {
        rules: [
            {
                test: /\.ts$/,
                loader: "ts-loader"
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: "./index.html"
        })
    ],
    devServer: {},
}
~~~

生成tsconfig.json（ts-loader需要）：

`tsc --init `

`npm run serve`



### 内置声明文件的配置：

其实我们ts文件能正常运行就是因为上面ts-loader的处理，ts-loader对ts文件的处理是基于tsconfig.json文件的。所以我们可以通过配置tsconfig.json来修改ts的各种运行环境，包括一些语言特性（支不支持es6...），随之就会影响内置的ts声明文件。

（项目中我们可能不用ts-loader进行ts文件处理，而是babel

### 外部定义的类型声明文件

在我们ts环境的项目中，一些三方库，比如axios，我们安装axios的时候，这个包里就包含了它的`.d.ts`类型声明文件，我们在项目里就可以直接使用axios了。

还有一种情况，比如react，我们npm i react之后import发现报错，因为react这个包本身缺少类型声明文件，**不是说react里的代码缺少类型逻辑（人家react本身就使用js写的），而是说我们的ts项目不认识这个第三方模块**，我们这时候一般还要单独安装他的类型声明文件相关的包，比如：`npm i @type/react --save-dev`，这样才能`import React from "react"`不报错。

### 自定义类型声明文件

有一些三方包，网络上也没有它的类型声明文件（让ts识别他的配置文件），这时候我们安装了这玩意之后，我们在我们的ts项目中直接import是会报错的，所以我们需要自定义一个`.d.ts`类型声明文件，让ts项目认识这个包——知道这个包是一个模块：

`jrd.d.ts`

~~~typescript
declare module "jrd" {
  export function join(...args: any[]): any
}
~~~

`index.ts`

~~~typescript
import JRD from "jrd"; // 有了上面的配置，ts就能识别jrd模块了，并且我们在调用JRD.join方法时也会给出相应的类型提示
~~~

#### 使用场景：

1. 引入的三方库没有类型声明文件，ts不识别相关模块，我们自己在`.d.ts`文件中`declare module`声明ap

2. 我们确定在代码运行起来的时候某些地方一定有某个变量，而且确定它的类型，但是我们ts项目中暂时并没有这个变量，这时候我们就可以在`.d.ts`文件中声明它的类型。声明之后ts就不会报错了：

   比如我们在`index.html`中的一个<script>标签中定义了三个变量，我们ts项目中并没有这三个变量，但是打包完之后，ts项目会被引入到`index.html`中，这样的话我们就能在`.d.ts`中先声明出来这三个变量，这样ts就不会报错了（我们自己保证逻辑上它运行时不会出错）

3. 文件模块的模块声明写在`.d.ts`里

## 模块声明

一些文件模块，比如一张`.png`图片，我们在ts项目中引入时ts是不识别这个文件作为一个模块的，所以我们会在`.d.ts`中声明文件模块：

~~~typescript
// 这种文件模块声明就不需要指明模块内容了
declare module "*.png"
declare module "*.jpg"
declare module "*.jpeg"
declare module "*.svg"
// 一般的模块声明
declare module "moduleName" {
  // 模块里面的类型定义
}
~~~

vue+ts的项目中，`.vue`的文件ts默认也是不识别的，需要我们在`.d.ts`文件中声明`.vue`文件模块：

~~~typescript
// 度成长项目中翻到的
declare module '*.vue' {
  import type {DefineComponent} from 'vue';
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  const component: DefineComponent<{}, {}, any>;
  export default component;
}
~~~

## .d.ts文件中声明命名空间(很少见)

我们在`index.html`中通过cdn的方式引入了一个包，比如jq，理论上来说我们的项目中完全可以使用jq的东西的，但是ts项目不识别，这时候我们按照上面在`.d.ts`文件中声明一个模块是不行的，因为我们根本不需要import引入这个模块了，所以我们这时候声明一个命名空间才是最合适的，因为命名空间是不要像模块一样引入就生效的：

~~~typescript
declare namespace $ {
  export function ajax(...): ...
}
~~~

# axios封装（空）

# 条件类型

## 条件类型的基本使用

~~~typescript
function sum<T extends number | string>(arg1: T, arg2: T): T extends string ? string : number; // 函数签名
function sum(arg1: any, arg2: any) {
  return arg1 + arg2
}
~~~

因为ts默认会进行一些类型判断给我们的变量一个类型，但是这个类型可能不是我们想要的。**条件类型说白了就是根据ts自动判断出来的类型的一些特性（"abc" extends string，ts默认给我们一个字面量类型"abc"，他满足extends string，所以返回修改成string类型），然后我们修改成我们想要的类型**，有点类似于三元运算符。

## 内置工具ReturnType

`ReturnType<myTypeOfMyFn>`返回传入的函数类型的返回值的类型：

~~~typescript
type CalcFnType = (num1: number, num2: number) => number

function foo() {
  return "abc";
}

type CalcReturnType = ReturnType<CalcFnType>;
type FooReturnType = ReturnType<typeof foo>
~~~

## 条件类型中推断（infer）

https://www.jianshu.com/p/707a304d7752

`infer`语法的限制如下：

1. `infer`只能在条件类型的 extends 子句中使用
2. `infer`得到的类型只能在`true`语句中使用, 即`X`中使用

自己实现ReturnType（取到函数的返回值）

~~~typescript
type MyReturnType<T extends (...args: any[]) => any> = T extends (...args: any[]) => infer R ? R : never;
~~~

**解析：MyReturnType<T extends (...args: any[]) => any>这里的<T extends (...args: any[]) => any>是限制传入MyReturnType的类型为一个函数类型；因为我们只想要这个函数类型的一个部分（返回值）的类型，所以我们想用infer，infer放在函数返回值类型变量的前面就好了，但是infer只能在extends子句中出现，所以T extends (...args: any[]) => infer R ? R : never这里再写一遍extends是为了使用infer**

**infer出现在extends子句的任意一个类型变量的前面，说白了就是让ts去拿到（推断）这个变量的类型，然后我们就可以去使用这个类型**

### 举例：

推断数组(或者元组)的类型

```rust
type InferArray<T> = T extends (infer U)[] ? U : never;
```

```tsx
type I0 = InferArray<[number, string]>; // string | number
type I1 = InferArray<string[]>; // string
type I2 = InferArray<number[]>; // number
```

推断某个函数类型的参数的类型：

~~~typescript
type MyParamters<T extends (...args: any[]) => any> = T extends (...args: infer P) => any ? P : never;
~~~

## 分发条件类型

![image-20230203110752836](./image/:Users:jinrongda:Library:Application Support:typora-user-images:image-20230203110752836.png)

白话：给条件类型传类型参数的时候（toArray为条件类型，因为用了extends，toArray<number | string>就是给toArray传递了一个number ｜ string类型），如果传入的是联合类型，相当于联合类型的各部分分开传给条件类型，最后条件类型的结果再联合起来。

# ts内置工具

3.44（前置映射类型）