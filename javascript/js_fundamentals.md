## 学习JavaScript基础知识，做一个更好的开发者
### Index
1. [原始类型](#原始类型)
1. [数字](#数字)
1. [字符串](#字符串)
1. [布尔值](#布尔值)
1. [对象](#对象)
1. [原始类型 VS 对象](原始类型-VS-对象)
1. [变量](#变量)
1. [数组](#数组)
1. [函数](#函数)
1. [函数声明](#函数声明)
1. [函数表达式](#函数表达式)
1. [箭头函数](#箭头函数)
1. [函数调用](#函数调用)
1. [this](#this)
1. [参数](#参数)
1. [return 语句](#return-语句)
1. [动态类型](#动态类型)
1. [单线程](#单线程)
1. [异常](#异常)
1. [原型模式](#原型模式)
1. [原型链](#原型链)
1. [函数模式](#函数模式)
1. [结论](#结论)
1. [原文链接](#原文链接)

JavaScript 有原始类型 primitives、对象和函数。它们都是值，所有这些都被视为对象，甚至原始的数据类型。

### 原始类型
数字、布尔值、字符串、undefined 和 null 是原始类型。

### 数字
JavaScript中只有一种数字类型，即双精度浮点数。十进制数的算术不精确。

你可能已经知道，0.1 + 0.2 并不等于 0.3。但对于整数，算术是准确的，所以 1+2 === 3。

数字从 `Number.prototype` 对象继承方法。方法可以在数字上被调用：
```js
(123).toString();  //"123"
(1.23).toFixed(1); //"1.2"
```

有用于转换为数字的全局函数：`parseInt(), parseFloat() 和 Number()`
```js
parseInt("1")       //1
parseInt("text")    //NaN
parseFloat("1.234") //1.234
Number("1")         //1
Number("1.234")     //1.234
```

无效的算术运算或无效的转换不会抛出异常，但会产生 NaN “not -a- number” 值。isNaN() 方法可用于检测 NaN。

The `+` 运算符可以用于加法或字符串拼接。
```js
1 + 1      //2
"1" + "1"  //"11"
1 + "1"    //"11"
```

### 字符串
字符串存储一系列 Unicode 字符。文本内容可以放在双引号 `""` 或单引号 `''` 内。

字符串有如下方法: substring()、indexOf() 和 concat()。
```js
"text".substring(1,3) //ex
"text".indexOf('x')   //2
"text".concat(" end") //text end
```

字符串和所有原始类型一样是不可变的。例如：concat() 不修改现有字符串，而是创建一个新的字符串。

### 布尔值
布尔值有两个值: 真和假。
JavaScript 语言有为真的值和假的值。
false, null, undefined，`''`(空字符串)，0 和 NaN 都是假值。所有其他值，包括所有对象，都是真值。

当在布尔上下文中运算时，真值被视为 true，假值被视为false。请看下一个显示了 false 分支的示例。
```js
let text = '';
if(text) {
  console.log("This is true");
} else {
  console.log("This is false");
}
// This is false
```

### 对象
对象是一个动态的键-值对集合。键 key 是字符串。值 value 可以是原始类型、对象或函数。

创建对象最简单的方法是使用对象字面量:
```js
let obj = {
  message : "A message",
  doSomething : function() {}
}
```

我们可以在任何时候读取、添加、编辑和删除对象的属性。

- 读取: `object.name, object[expression]`
- 添加或修改: `object.name = value, object[expression] = value`
- 删除: `delete object.name, delete object[expression]`

```js
let obj = {}; //create empty object
obj.message = "A message"; //add property
obj.message = "A new message"; //edit property
delete object.message; //delete property
```

对象被实作为散列映射。可以使用 `objec.create(null)` 创建一个简单的散列映射:
```js
let french = Object.create(null);
french["yes"] = "oui";
french["no"]  = "non";
french["yes"];//"oui"
```

如果希望使对象不可变，使用 `object.freeze()` 方法。

可以使用 `Object.keys()` 方法遍历对象的所有属性。
```js
function logProperty(name){
  console.log(name); //property name
  console.log(obj[name]); //property value
}
Object.keys(obj).forEach(logProperty);
```

### 原始类型 VS 对象
原语数据类型被视为对象，因为它们有方法，但它们不是对象。

原始类型是不可变的，对象是可变的。

### 变量
可以使用 var、let 和 const 定义变量。

`var` 声明并且可选择性地初始化一个变量。未初始化的变量的值为 undefined。
用 var 声明的变量有一个函数作用域。

`let` 声明的变量具有块作用域。

`const` 声明的变量不能重新被赋值。const 冻结变量 variable，`object.freeze()` 方法冻结对象 object。

在任何函数之外声明的变量，它的作用域都是全局的 global。

### 数组
JavaScript 有类似数组的对象 -- 数组是使用对象来实现的，使用索引访问元素，索引被转换为字符串并用作检索出来的值的名称。在下一个示例中，`arr[1]` 获取的值与 `arr['1']` 是相同的: 即 `arr[1] === arr['1']`。

一个简单的数组，比如：`let arr = ['A'， 'B'， 'C']` 通过如下的对象来模拟:

```js
{
  '0': 'A',
  '1': 'B',
  '2': 'C'
}
```

用 `delete` 从数组中删除值会留下空洞 holes。`splice()` 可以用来避免这个问题，但是它在移动所有元素（将位于被删除元素后面的元素向左移动一个位置）的时候是很慢的。

```js
let arr = ['A', 'B', 'C'];
delete arr[1];
console.log(arr); // ['A', empty, 'C']
console.log(arr.length); // 3
```

### 函数
函数是对象。函数可以赋值给变量，存储在对象或数组中，作为参数传递给其他函数
和从函数中返回。

定义函数有三种方法:
- 函数声明
- 函数表达式(又称函数字面量)
- 箭头函数

### 函数声明
- `function` 是在一行上的第一个关键字
- 它一定有名字
- 可在定义前使用。函数声明被移动或“提升”到其作用域的顶部。

```js
function doSomething(){}
```

### 函数表达式
- `function` 不是在一行上的第一个关键字
- 函数名是可选的--可以是一个匿名函数表达式或一个命名函数表达式。
- 它需要被定义，然后才能执行
- 定义后可以自动执行(称为 `“IIFE” Immediately Invoked Function Expression` 立即调用函数表达式)

```js
let doSomething = function() {}
```

### 箭头函数
箭头函数是用于创建匿名函数表达式的糖语法。

```js
let doSomething = () => {};
```

箭头函数没有自己的`this`和参数 `arguments`

### 函数调用
函数可以以不同的方式调用:

- 函数形式
```js
doSomething(arguments)
```

- 方法形式
```js
theObject.doSomething(arguments)
theObject["doSomething"](arguments)
```

- 构造函数形式
```js
new doSomething(arguments)
```

- `apply` 形式
```js
doSomething.apply(theObject, [arguments])
doSomething.call(theObject, arguments)
```

- `bind` 形式
```js
let doSomethingWithObject = doSomething.bind(theObject);
doSomethingWithObject();
```
调用函数时传入的参数可以多于也可以少于函数在定义时声明的变量数。多余的参数将被忽略，缺少的参数将被设置为 `undefined。`

函数有两个伪参数: `this` 和 `arguments`。

### this
`this` 表示函数的上下文。它的值取决于函数是如何被调用的。

```js
+------------+------------------+
| Form       | this             |
+------------+-------------------
| Function   | window/undefined |
| Method     | theObject        |
| Constructor| the new object   |
| apply      | theObject        |  
| bind       | theObject        |    
+------------+------------------+
```

### 参数
`arguments` 伪参数（`pseudo-parameter`）提供了调用函数时使用的所有参数。它是一个类数组的对象，但不是数组。它缺少数组所具有的方法。

```js
function reduceToSum(total, value){
  return total + value;
}

function sum(){
  let args = Array.prototype.slice.call(arguments);
  return args.reduce(reduceToSum, 0);
}

sum(1,2,3);
```

另一种替代选择是新的其余参数（rest parameters）语法。这次 args 是一个数组对象。
```js
function sum(...args){
  return args.reduce(reduceToSum, 0);
}
```

### return 语句
没有 `return` 语句的函数返回 `undefined`。使用 `return` 时要注意自动插入分号 `;`。流函数(The flowing function)不会返回一个空对象，而是一个未定义的对象。

```js
function getObject(){
  return
  {
  }
}
getObject()
// undefined
```

为了避免这个问题，在 `return` 的相同行上使用 `{`
```js
function getObject(){
  return {
  }
}
getObject()
// {}
```

### 动态类型
JavaScript 有动态类型。值有类型，变量没有。类型可以在运行时更改。
```js
function log(value){
  console.log(value);
}
log(1);
log("text");
log({message : "text"});
```

`typeof()` 运算符可以检查变量的类型。
```js
let n = 1;
typeof(n);   //number
let s = "text";
typeof(s);   //string
let fn = function() {};
typeof(fn);  //function
```

### 单线程
主要的 JavaScript 运行时 runtime 是单线程的。两个函数不能同时运行。运行时包含一个事件队列 queue，该队列存储要处理的消息列表。没有死锁 deadlock，所以不需要使用锁 lock。但是，事件队列中的代码需要快速运行,否则浏览器会变得没有响应，并要求终止 kill 任务。

### 异常
JavaScript 有一个异常处理机制。通过使用 `try/catch` 语句包装代码，它可以像您期望的那样工作。`try/catch` 语句有单一的 `catch` 代码块，用于缓存所有异常。

同样有趣的是，JavaScript 有时更喜欢沉默的错误(silent errors)。这与 JavaScript 在 EcmaScript 3 之前没有抛出错误有关。

当我试图修改一个冻结的 frozen 对象时，下面的代码不会抛出异常:
```js
let obj = Object.freeze({});
obj.message = "text";
```

严格模式消除了一些 JavaScript 无声的 silent 错误。`"use strict";` 语句会激活严格模式。

### 原型模式
`Object.create()`、构造函数和类 class 在原型系统上构建对象。

考虑下面的例子:
```js
let service = {
 doSomething : function() {}
}
let specializedService = Object.create(service);
console.log(specializedService.__proto__ === service); //true
```

我使用 `Object.create()` 构建了一个新的对象 `specializedService`，该对象有 `service` 对象作为它的原型 prototype。这意味着 `doSomething()`方法在 `specializedService` 对象上是可用的。这也意味着 `specializedService` 对象的 `__proto__` 属性指向 `service` 对象。

现在让我们使用类 class 构建一个类似的对象。
```js
class Service {
  doSomething(){}
}
class SpecializedService extends Service {
}
let specializedService = new SpecializedService();
console.log(specializedService.__proto__ === SpecializedService.prototype);
```

`Service` 类中定义的所有方法都将被添加到 `Service.prototype` 原型对象中。`Service` 类的实例将具有相同的原型对象 (`Service.prototype`)。所有实例都将委托方法调用到 `Service.prototype` 原型对象。方法 method 一旦在 `Service.prototype` 对象上定义，那么 `Service` 类的所有的实例都将继承这些方法。

### 原型链
对象继承于其他对象。每个对象都有一个原型，并从中继承其属性。

当您请求一个对象不包含的属性时，JavaScript 将查找原型链，直到它找到所请求的属性，或者直到它到达链的末端。

### 函数模式
JavaScript 有一级函数和闭包。这些概念为 JavaScript 中的函数式编程打开了大门。因此，高阶函数是可能的。

闭包是一个内部函数，它可以访问父函数的变量，即使在父函数执行之后也是如此。

高阶函数是将另一个函数作为输入、返回一个函数或两者都做的函数。

有关JavaScript函数方面的更多信息，请参阅:

- [Discover the power of first class functions](https://medium.freecodecamp.org/discover-the-power-of-first-class-functions-fd0d7b599b69)
- [How point-free composition will make you a better functional programmer](https://medium.freecodecamp.org/how-point-free-composition-will-make-you-a-better-functional-programmer-33dcb910303a)
- [Here are a few function decorators you can write from scratch](https://medium.freecodecamp.org/here-are-a-few-function-decorators-you-can-write-from-scratch-488549fe8f86)
- [Why you should give the Closure function another chance](https://medium.freecodecamp.org/why-you-should-give-the-closure-function-another-chance-31253e44cfa0)
- [Make your code easier to read with Functional Programming](https://medium.freecodecamp.org/make-your-code-easier-to-read-with-functional-programming-94fb8cc69f9d)

### 结论
JavaScript 的强大之处在于它的简单性。

了解 JavaScript 的基本原理可以使我们更好地理解和使用该语言。

### 原文链接
- [Learn these JavaScript fundamentals and become a better developer](https://medium.freecodecamp.org/learn-these-javascript-fundamentals-and-become-a-better-developer-2a031a0dc9cf)
