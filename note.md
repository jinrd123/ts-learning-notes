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

