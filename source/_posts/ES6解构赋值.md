---
title: ES6 解构赋值 
date: 2016年8月10日
categories: 
     - JS   
toc: true  
---

#### 1.数组的解构赋值 ####

``` js
	//ES5的写法
	var a = 1;
	var b = 2;
	var c = 3;
	//ES6的写法
	var [a,b,c] = [1,2,3];
```

上面代码表示，可以从数组中提取值，按照对应位置，对变量赋值。
<!-- more -->
本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。下面是一些使用嵌套数组进行解构的例子。
```js
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```
如果解构不成功，变量的值就等于undefined。
```js
var [foo] = [];
var [bar, foo] = [1];
```
以上两种情况都属于解构不成功，foo的值都会等于undefined。

另一种情况是不完全解构，即等号左边的模式，只匹配一部分的等号右边的数组。这种情况下，解构依然可以成功。

```js
let [x, y] = [1, 2, 3];
x // 1
y // 2

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```
上面两个例子，都属于不完全解构，但是可以成功。

如果等号的右边不是数组,那么将会报错.
```js
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```
上面的表达式都会报错，因为等号右边的值，要么转为对象以后不具备Iterator接口（前五个表达式），要么本身就不具备Iterator接口（最后一个表达式）。

`构赋值不仅适用于var命令，也适用于let和const命令。`
```js
var [v1, v2, ..., vN ] = array;
let [v1, v2, ..., vN ] = array;
const [v1, v2, ..., vN ] = array;
```
`对于Set结构，也可以使用数组的解构赋值。`
```js
let [x, y, z] = new Set(["a", "b", "c"]);
x // "a"
```
`事实上，只要某种数据结构具有Iterator接口，都可以采用数组形式的解构赋值。`
```js
function* fibs() {
  var a = 0;
  var b = 1;
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

var [first, second, third, fourth, fifth, sixth] = fibs();
sixth // 5
```
上面代码中，fibs是一个Generator函数，原生具有Iterator接口。解构赋值会依次从这个接口获取值。

-----
#### 默认值
解构赋值允许指定默认值。
```js
var [foo = true] = [];
foo // true

[x, y = 'b'] = ['a']; // x='a', y='b'
[x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```
#### 2.对象的解构赋值 ####
解构不仅可以用于数组，还可以用于对象。

```js
var { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
```
对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

```js
var { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

var { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```
上面代码的第一个例子，等号左边的两个变量的次序，与等号右边两个同名属性的次序不一致，但是对取值完全没有影响。第二个例子的变量没有对应的同名属性，导致取不到值，最后等于undefined。

如果变量名与属性名不一致，必须写成下面这样。

```js
var { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```
上说明，对象的解构赋值是下面形式的简写。

>var { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
也就是说，对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

```js
var { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined
```
真正被赋值的是变量baz，而不是模式foo。

注意，采用这种写法时，变量的声明和赋值是一体的。对于let和const来说，变量不能重新声明，所以一旦赋值的变量以前声明过，就会报错。

```js
let foo;
let {foo} = {foo: 1}; // SyntaxError: Duplicate declaration "foo"

let baz;
let {bar: baz} = {bar: 1}; // SyntaxError: Duplicate declaration "baz"
```
码中，解构赋值的变量都会重新声明，所以报错了。不过，因为var命令允许重新声明，所以这个错误只会在使用let和const命令时出现。如果没有第二个let命令，上面的代码就不会报错。

```js
let foo;
({foo} = {foo: 1}); // 成功

let baz;
({bar: baz} = {bar: 1}); // 成功
```
码中，let命令下面一行的圆括号是必须的，否则会报错。因为解析器会将起首的大括号，理解成一个代码块，而不是赋值语句。

和数组一样，解构也可以用于嵌套结构的对象。

```js
var obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

var { p: [x, { y }] } = obj;
x // "Hello"
y // "World"
```
这时p是模式，不是变量，因此不会被赋值。

```js
var node = {
  loc: {
    start: {
      line: 1,
      column: 5
    }
  }
};

var { loc: { start: { line }} } = node;
line // 1
loc  // error: loc is undefined
start // error: start is undefined
```
码中，只有line是变量，loc和start都是模式，不会被赋值。

下面是嵌套赋值的例子。

```js
let obj = {};
let arr = [];

({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });

obj // {prop:123}
arr // [true]
```
解构也可以指定默认值。

```js
var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x:y = 3} = {};
y // 3

var {x:y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
```
生效的条件是，对象的属性值严格等于undefined。

```js
var {x = 3} = {x: undefined};
x // 3

var {x = 3} = {x: null};
x // null
```
代码x属性等于null，就不严格相等于undefined，导致默认值不会生效。

如果解构失败，变量的值等于undefined。

```
var {foo} = {bar: 'baz'};
foo // undefined
```
如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错。

// 报错
>var {foo: {bar}} = {baz: 'baz'};
上面代码中，等号左边对象的foo属性，对应一个子对象。该子对象的bar属性，解构时会报错。原因很简单，因为foo这时等于undefined，再取子属性就会报错，请看下面的代码。

>var _tmp = {baz: 'baz'};
>_tmp.foo.bar // 报错
如果要将一个已经声明的变量用于解构赋值，必须非常小心。

// 错误的写法
```js
var x;
{x} = {x: 1};
// SyntaxError: syntax error
``` 
写法会报错，因为JavaScript引擎会将{x}理解成一个代码块，从而发生语法错误。只有不将大括号写在行首，避免JavaScript将其解释为代码块，才能解决这个问题。

// 正确的写法
>({x} = {x: 1});
上面代码将整个解构赋值语句，放在一个圆括号里面，就可以正确执行。关于圆括号与解构赋值的关系，参见下文。

解构赋值允许，等号左边的模式之中，不放置任何变量名。因此，可以写出非常古怪的赋值表达式。

```js
({} = [true, false]);
({} = 'abc');
({} = []);
```
表达式虽然毫无意义，但是语法是合法的，可以执行。

对象的解构赋值，可以很方便地将现有对象的方法，赋值到某个变量。

>let { log, sin, cos } = Math;
上面代码将Math对象的对数、正弦、余弦三个方法，赋值到对应的变量上，使用起来就会方便很多。

由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。

```js
var arr = [1, 2, 3];
var {0 : first, [arr.length - 1] : last} = arr;
first // 1
last // 3
```
数组进行对象结构。数组arr的0键对应的值是1，[arr.length - 1]就是2键，对应的值是3。

----
#### 3.字符串的解构赋值 ####

字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。

```js
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```
组的对象都有一个length属性，因此还可以对这个属性解构赋值。

>let {length : len} = 'hello';
>len // 5

#### 4.数组和布尔值的解构赋值 ####
解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。
```js
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```
上面代码中，数值和布尔值的包装对象都有toString属性，因此变量s都能取到值。

解构赋值的规则是，只要等号右边的值不是对象，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。

-----

#### 5.函数参数的解构赋值 ####
```js
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```
上面代码中，函数add的参数表面上是一个数组，但在传入参数的那一刻，数组参数就被解构成变量x和y。对于函数内部的代码来说，它们能感受到的参数就是x和y。
```js
[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
```
函数参数的解构也可以使用默认值。
```js
function move({x = 0, y = 0} = {}) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
```
上面代码中，函数move的参数是一个对象，通过对这个对象进行解构，得到变量x和y的值。如果解构失败，x和y等于默认值。
注意，下面的写法会得到不一样的结果。
```js
function move({x, y} = { x: 0, y: 0 }) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, undefined]
move({}); // [undefined, undefined]
move(); // [0, 0]
```
上面代码是为函数move的参数指定默认值，而不是为变量x和y指定默认值，所以会得到与前一种写法不同的结果。

undefined就会触发函数参数的默认值。
```js
[1, undefined, 3].map((x = 'yes') => x);
// [ 1, 'yes', 3 ]
```