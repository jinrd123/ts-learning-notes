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

