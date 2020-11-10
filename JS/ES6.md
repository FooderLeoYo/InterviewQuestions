# ES6

## 目录

[var、let 及 const 区别](#jump1)

[Proxy](#jump2)

[map](#jump3)

[filter](#jump4)

[reduce](#jump5)

[箭头函数与普通函数的区别](#jump6)

[Promise](#jump7)

[async 和 await](#jump8)

[私有方法和私有属性](#jump9)

[Generator ](#jump10)

[ES6对几种基本数据类型做的常用升级优化](#jump11)

[Symbol是什么，有什么作用？](#jump12)

[Set是什么，有什么作用？](#jump13)

[Map是什么，有什么作用？](#jump14)

[Proxy是什么，有什么作用？](#jump15)

[Reflect是什么，有什么作用？](#jump16)

[Iterator是什么，有什么作用？(重要)](#jump17)

[for...in 和for...of有什么区别？](#jump18)

[Object.is() 与原来的比较操作符 ===、== 的区别？](#jump19)

[](#jump)

[](#jump)

---	

<span id="jump1"></span>

## var、let 及 const 区别

- 全局申明的 var 变量会挂载在 window 上，而 let 和 const 不会

- var 声明变量存在变量提升，let 和 const 不会

- let、const 的作用范围是块级作用域，而 var 的作用范围是函数作用域

- 同一作用域下 let 和 const 不能声明同名变量，而 var 可以

- 同一作用域下在 let 和 const 声明前使用会存在暂时性死区

- const

	- 一旦声明必须赋值，不能使用 null 占位

	- 声明后不能再修改

	- 如果声明的是复合类型数据，可以修改其属性

---

<span id="jump2"></span>

## Proxy

### 概念

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写

```javascript
var proxy = new Proxy(target, handler);
```

new Proxy()表示生成一个Proxy实例

target参数表示所要拦截的目标对象

handler参数是一个配置对象，需要提供一个对应的处理函数，且必须是Proxy支持的拦截类型

注意，要使得Proxy起作用，必须针对Proxy实例proxy进行操作，而不是针对目标对象target进行操作

例如：

```javascript
var proxy = new Proxy({}, {
  get: function(target, propKey) {
    return 35;
  }
});

proxy.time // 35
proxy.name // 35
proxy.title // 35
```

### Proxy支持的拦截类型

- get(target, propKey, receiver)
	- 拦截对象属性的读取，比如 proxy.foo 和 proxy['foo']。

- set(target, propKey, value, receiver)
	- 拦截对象属性的设置，比如 proxy.foo = v 或 proxy['foo'] = v，返回一个布尔值。

- has(target, propKey)
	- 拦截 propKey in proxy 的操作，返回一个布尔值。

- deleteProperty(target, propKey)
	- 拦截 delete proxy[propKey]的操作，返回一个布尔值。

- ownKeys(target)
	- 拦截 Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)、for...in 循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而 Object.keys()的返回结果仅包括目标对象自身的可遍历属性。

- getOwnPropertyDescriptor(target, propKey)
	- 拦截 Object.getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象。

- defineProperty(target, propKey, propDesc)
	- 拦截 Object.defineProperty(proxy, propKey, propDesc）、Object.defineProperties(proxy, propDescs)，返回一个布尔值。

- preventExtensions(target)
	- 拦截 Object.preventExtensions(proxy)，返回一个布尔值。

- getPrototypeOf(target)
	- 拦截 Object.getPrototypeOf(proxy)，返回一个对象。

- isExtensible(target)
	- 拦截 Object.isExtensible(proxy)，返回一个布尔值。

- setPrototypeOf(target, proto)
	- 拦截 Object.setPrototypeOf(proxy, proto)，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。

- apply(target, object, args)
	- 拦截 Proxy 实例作为函数调用的操作，比如 proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。

- construct(target, args)
	- 拦截 Proxy 实例作为构造函数调用的操作，比如 new proxy(...args)。

### vue中的应用

Vue3.0 中将会通过 Proxy 来替换原本的 Object.defineProperty 来实现数据响应式

原因在于 Proxy 无需一层层递归为每个属性添加代理，一次即可完成以上操作，性能上更好

并且原本的实现有一些数据更新不能监听到，但是 Proxy 可以完美监听到任何方式的数据改变，唯一缺陷可能就是浏览器的兼容性不好了

---

<span id="jump3"></span>

## map

map的作用是历原数组，将每个元素拿出来做一些变换然后返回一个新数组，原数组不发生改变

```javascript
array.map(function(currentValue,index,arr), thisValue)
```

[参数说明](https://www.runoob.com/jsref/jsref-map.html)

---

<span id="jump4"></span>

## filter

filter的作用是在遍历数组的时候将返回值为 true 的元素放入新数组，我们可以利用这个函数删除一些不需要的元素

```javascript
array.filter(function(currentValue,index,arr), thisValue)
```

[参数说明](https://www.runoob.com/jsref/jsref-filter.html)

---

<span id="jump5"></span>

## reduce

reduce可以将数组中的元素通过回调函数最终转换为一个值

```javascript
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```

[参数说明](https://www.runoob.com/jsref/jsref-reduce.html)

---

<span id="jump"></span>

## 箭头函数与普通函数的区别

- 普通 function 的声明在变量提升中是最高的，箭头函数没有函数提升

- 箭头函数没有属于自己的this，arguments

- 箭头函数不能作为构造函数，不能被 new，因为：

	- 没有自己的 this，无法调用 call，apply

	- 没有 prototype 属性

- 不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数

---

<span id="jump7"></span>

## Promise

---

<span id="jump8"></span>

## async 和 await

async 就是将函数返回值使用 Promise.resolve() 包裹了下

await 就是 generator 加上 Promise 的语法糖，且内部实现了自动执行 generator

相比直接使用 Promise 来说，优势在于处理 then 的调用链，能够更清晰准确的写出代码

当然也存在一些缺点，如果多个异步代码没有依赖性却使用了 await 会导致性能上的降低，这时完全可以使用 Promise.all 的方式

---

<span id="jump9"></span>

## 私有方法和私有属性

### 命名上加以区别

即在函数名或属性名前加_

但这并不安全，只是一种团队规范

### Symbol

利用Symbol 值的唯一性

利用Symbol 值的唯一性，将私有方法的名字命名为一个 Symbol 值

```javascript
const bar = Symbol('bar');
const snaf = Symbol('snaf');

export default class myClass {
  // 公有方法
  foo(baz) {
    this[bar](baz);
  }

  // 私有方法
  [bar](baz) {
    return (this[snaf] = baz);
  }

  // ...
}
```

上面代码中，bar 和 snaf 都是 Symbol 值，一般情况下无法获取到它们

但是也不是绝对不行，Reflect.ownKeys()依然可以拿到它们

```javascript
const inst = new myClass();

Reflect.ownKeys(myClass.prototype);
// [ 'constructor', 'foo', Symbol(bar) ]
```

### 模块

将私有方法移出类，放到模块里

```javascript
class Widget {
  foo(baz) {
    bar.call(this, baz);
  }

  // ...
}

function bar(baz) {
  return (this.snaf = baz);
}
```

上面代码中，foo 是公开方法，内部调用了 bar.call(this, baz)

这使得 bar 实际上成为了当前模块的私有方法

---

<span id="jump10"></span>

## Generator

### 定义

function关键字与函数名之间有一个星号，代表该函数是Generator

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}
```
### 调用

调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针

```javascript
var hw = helloWorldGenerator(); // hw是一个指针
```

### next

以后，每次调用遍历器对象的next方法，就会返回一个有着value和done两个属性的对象

```javascript
hw.next();
// { value: 'hello', done: false }

hw.next();
// { value: 'world', done: false }

hw.next();
// { value: 'ending', done: true }

hw.next();
// { value: undefined, done: true }
```

其中：

- value是紧跟在本次yield后面的表达式的计算结果

- done表示是否遍历结束

遍历器对象的next方法的运行逻辑如下：

- 遇到yield表达式就暂停，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值

- 下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式

- 直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值

- 如果该函数没有return语句，则返回的对象的value属性值为undefined

### next 方法的参数

yield表达式本身没有返回值，或者说总是返回undefined。value和done是next返回的，这点要注意

next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值，即相当于将yield及其左边的表达式一起替换成了这个参数

```javascript
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}; 相当于把```yield (x + 1)```换成```null```，因此y = 2 * (null)
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }; 相当于把```yield (x + 1)```换成```12```，因此y = 2 * (12)
b.next(13) // { value:42, done:true }; 相当于把```yield (y / 3)```换成```13```，因此z = 13
```

---

<span id="jump11"></span>

## ES6对几种基本数据类型做的常用升级优化

### String

#### 优化部分

- 字符串模板

#### 升级部分

- 在String原型上新增了includes()方法，用于取代传统的只能用indexOf查找包含字符的方法(

- 还新增了startsWith(), endsWith(), padStart(),padEnd(),repeat()等方法

### Array

#### 优化部分

- 数组解构赋值

- 扩展运算符

#### 升级部分

- 在rray原型上新增了find()方法，用于取代传统的只能用indexOf查找包含数组项目的方法

- 还新增了copyWithin(), includes(), fill(),flat()等方法

### Number

#### 优化部分

- 在Number原型上新增了isFinite(), isNaN()方法，用来取代ES5的会先将非数值类型的参数转化为Number类型再做判断的isFinite(), isNaN()

#### 升级部分

- 在Math对象上新增了Math.cbrt()，trunc()，hypot()等等较多的科学计数法运算方法

### Object（重要）

#### 优化部分

- 对象的解构赋值

- 对象的扩展运算符(...)

- super 关键字

#### 升级部分

- 在Object原型上新增了is()方法，做两个目标对象的相等比较，用来完善'==='方法

- 在Object原型上新增了assign()方法，用于对象新增属性或者多个对象合并

- 在Object原型上新增了getOwnPropertyDescriptors()方法，结合defineProperties()方法，可以完美复制对象

- 在Object原型上新增了getPrototypeOf()和setPrototypeOf()方法，用来获取或设置当前对象的prototype对象

- 在Object原型上还新增了Object.keys()，Object.values()，Object.entries()方法

### Function

#### 优化部分

- 箭头函数

#### 升级部分

- 双冒号运算符，用来取代以往的bind，call,和apply(浏览器暂不支持，Babel已经支持转码)

```javascript
foo::bar;
// 等同于
bar.bind(foo);

foo::bar(...arguments);
// 等同于
bar.apply(foo, arguments);
```

---

<span id="jump12"></span>

## Symbol是什么，有什么作用

- 是ES6新引入的原始数据类型

- 所有Symbol()生成的值都是独一无二的，可以从根本上解决对象属性太多导致属性名冲突覆盖的问题

---

<span id="jump13"></span>

## Set是什么，有什么作用？

- Set是ES6引入的一种类似Array的新的数据结构，Set实例的成员类似于数组item成员

- 区别是Set实例的成员都是唯一，不重复的，这个特性可以轻松地实现数组去重

---

<span id="jump14"></span>

## Map是什么，有什么作用？

- 是ES6引入的一种类似Object的新的数据结构，Map可以理解为是Object的超集

- 打破了以传统键值对形式定义对象，对象的key不再局限于字符串，也可以是Object，可以更加全面的描述对象的属性

---

<span id="jump15"></span>

## Proxy是什么，有什么作用？

- 是ES6新增的一个构造函数，可以理解为JS语言的一个代理

- 用来改变JS默认的一些语言行为，包括拦截默认的get/set等底层方法，使得JS的使用自由度更高

---

<span id="jump16"></span>

## Reflect是什么，有什么作用？

- 将原生的一些零散分布在Object、Function或者全局函数里的方法(如apply、delete、get、set等等)，统一整合到Reflect上，这样可以更加方便更加统一的管理一些原生API

- 因为Proxy可以改写默认的原生API，如果一旦原生API别改写可能就找不到了，所以Reflect也可以起到备份原生API的作用

---

<span id="jump17"></span>

## Iterator是什么，有什么作用？(重要)

- 因为ES6新增了Set、Map类型，他们和Array、Object类型很像，但是都不能用for循环遍历。ES6也就顺其自然的需要一种设计标准，来统一所有可遍历类型的遍历方式，Iterator正是这样一种标准

Iterator标准规定，所有部署了key值为[Symbol.iterator]，且[Symbol.iterator]的value是标准的Iterator接口函数的对象，都称之为可遍历对象

---

<span id="jump18"></span>

## for...in 和for...of有什么区别？

ES6统一了遍历标准，制定了可遍历对象Iterator，而遍历则采用for...of去遍历

所有部署了Iterator接口的对象(可遍历对象)都可以通过for...of去遍历，而for..in仅仅可以遍历对象

---

<span id="jump19"></span>

## Object.is() 与原来的比较操作符 ===、== 的区别？

- == 相等运算符，比较时会自动进行数据类型转换

- === 严格相等运算符，比较时不进行隐式类型转换

- Object.is 同值相等算法，在 === 基础上对 0 和 NaN 特别处理

