# ES6小结
>2018.10.05
## 前言
    ES6没有什么好说的，重点知识。。。
## 正文
### 一、let和const
    let和const命令可以说是ES6为了完善js的变量作用环境提出的，主要的特点有以下
- 不存在变量提升，不会像var存在变量提升的毛病
- 暂时性死区，如果在作用域内提前使用了let命令声明的变量，不管其外部作用域是否已经声明了该变量，都会报ReferenceError（引用错误）
- 不允许重复声明，在同一作用域下let命令不允许重复声明变量，否则会报错
- 块级作用域: let和const命令都为使用的地方启用了块级作用域这个特性（只有在两个大括号内），支持多级嵌套和只允许内层作用域访问外层作用域。
- const的本质：const的本质是保存的内存的地址，这个地址是无法改变的，当时内存的数据就不一定了，如果保存的是对象，那么对象内的数据变更是检测不到的，所以const还是有问题的，使用时需要谨慎。
- 其他: for循环的循环声明部分是一个父级作用域，循环体一个子作用域；函数的参数默认声明的，不能再在函数体内使用let和const重复声明。
### 二、解构赋值
    ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
#### 1.数组的解构赋值
##### 1.1基本用法
如下代码，在数组内([])匹配号好对应位置的数组元素即可，支持不完全匹配、数组嵌套匹配、`...`操作符匹配剩余元素；注意：如果解构不成功会返回unfettered
```javascript
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
##### 1.2默认值
如下代码，使用默认值的时候，只有当匹配的元素严格等于（`===`）undefined的时候，默认值才会生效。
```javascript
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```
#### 2.对象的解构赋值
##### 2.1基本用法
如下代码，对象的解构赋值逻辑是通过属性名匹配，然后将属性值赋值，支持不完全匹配、对象嵌套匹配、对象属性简写模式；注意：如果解构不成功会返回undefined，且注意属性名是匹配模式，属性值才是需要赋值的对象。
```javascript
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```
##### 2.2默认值
```javascript
如下代码，默认值生效的逻辑与数组的默认值相同
var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x: y = 3} = {};
y // 3

var {x: y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
```
####  3.其他注意事项
除了上面提到，还有字符串、数值、bool、函数参数都支持解构赋值
### 三、函数的扩展
#### 1.函数参数默认值
ES6后函数可以设置默认值，具体用法看如下代码，函数参数的默认值只有实参严格对于undefined的时候才会生效，而默认值设置的位置一般会放在参数列表的末尾，如果不这样就必须手动加入undefined。
```javascript
// 例一
function f(x = 1, y) {
  return [x, y];
}

f() // [1, undefined]
f(2) // [2, undefined])
f(, 1) // 报错
f(undefined, 1) // [1, 1]

// 例二
function f(x, y = 5, z) {
  return [x, y, z];
}

f() // [undefined, 5, undefined]
f(1) // [1, 5, undefined]
f(1, ,2) // 报错
f(1, undefined, 2) // [1, 5, 2]
```
#### 2.rest参数
rest代表的是一个真正的数组，用于获取函数多于的参数，这样就能传入不定数量的参数了，具体使用如下代码
```javascript
function add(...values) {
  let sum = 0;
  for (var val of values) {
    sum += val;
  }
  return sum;
}

add(2, 5, 3) // 10
```
#### 3.箭头函数
箭头函数，一种函数的缩写形式，一定程度上减少了编写函数时的代码量，使用方式见如下代码，使用时需要注意一下几点:
- 箭头函数不能当做构造函数使用
- 箭头函数中的this，就是定义时所在的对象，而不是使用时所在的对象（可以理解为绑定上下文）
- 箭头上中不能使用arguments这个对象，可以通过rest对象代替
- 不可以使用yield命令，故不能用作Generator函数 
### 四、对象的扩展
- 1.属性简洁表示，ES6支持在对象中直接写入变量和函数来作为对象的属性和方法，这样就是属性名为变量名，属性值为变量值。
    ```javascript
    const foo = 'bar';
    const baz = {foo};
    baz // {foo: "bar"}
    // 等同于
    const baz = {foo: foo};
    ```
- 2.属性名表达式，es6支持访问或者定义对象的属性通过类似数组的方式，且支持填入表达式。
    ```javascript
    // 方法一
    obj.foo = true;
    // 方法二
    obj['a' + 'bc'] = 123;
    ```
- 3.Object.is()，由于ES5以前通过全等来比较两个变量的值是否相等，但是其中有很多bug，比如：+0 !== -0、NaN !== NaN，Object.is()则是ES6提供的一个值比较的方法，算是填补了以前的bug。
    ```javascript
    +0 === -0 //true
    NaN === NaN // false

    Object.is(+0, -0) // false
    Object.is(NaN, NaN) // true
    ```
### 五、其他
- Array.form: Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。

### 六、Iterator和一些循环
    Iterator是ES6为了统一数据结构可遍历的提出的，具体时间数据结构只要定义了Iterator则可视为该数据类型是可遍历的，具体是定义Symbol.iterator这个表达式属性，通过属性表达式进行定义，该属性返回一个函数，该函数返回一个具有next()方法的对象，通过调用next()方法会逐步遍历该数据结构类型的数据。
#### 1.对象的遍历
- for...in循环：只遍历对象自身的和继承的可枚举的属性,其中提供的参数是属性名。
- Object.keys()：返回对象自身的所有可枚举的属性的键名的一个数组。
- Object.values(): 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。
- Object.entries: 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。
- Object.getOwnPropertyNames(obj)：返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。
#### 数组的遍历
- for...of，可以遍历部署了Symbol.iterator属性的数据结构，包括:数组，Set、Map等（遍历数组是是直接遍历值）
- for...in，也可以遍历数组，但是是遍历的数组下标
- forEach，向其提供一个函数，函数中提供两个参数，第一个参数是数组值，第二个是数组下标
- map，通过map也可以遍历数组，但是会返回一个新的数组
- for循环，最老套的方法
## 参考
- [ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/let)