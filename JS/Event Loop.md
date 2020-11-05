# Event Loop

## 目录

[为什么JavaScript是单线程](#jump1)

[JavaScript 实现异步编程的方法](#jump2)

[setTimeOut、setImmediate、process.nextTick()的比较](#jump3)

[js 延迟加载的方式有哪些](#jump4)

[script的defer和async](#jump5)

[](#jump)

[](#jump)

[](#jump)

---	

<span id="jump1"></span>

## 为什么JavaScript是单线程

### 同步问题

作为浏览器脚本语言，JavaScript 的主要用途是与用户互动，以及操作 DOM

这决定了它只能是单线程，否则会带来很复杂的同步问题

比如，假定 JavaScript 同时有两个线程，一个线程在某个 DOM 节点上添加内容，另一个线程删除了这个节点，这时浏览器就会不知道应该以哪个线程为准

### Web Worker

为了利用多核 CPU 的计算能力，HTML5 提出 Web Worker 标准，允许 JavaScript 脚本创建多个线程

但是子线程完全受主线程控制，且不得操作 DOM

所以，这个新标准并没有改变 JavaScript 单线程的本质

---

<span id="jump2"></span>

## JavaScript 实现异步编程的方法

- 回调函数

- 事件监听

- 发布/订阅

- Promises 对象

- Async 函数

---

<span id="jump3"></span>

## setTimeOut、setImmediate、process.nextTick()的比较

### setTimeout()

将事件插入到了事件队列，必须等到当前代码（执行栈）执行完，主线程才会去执行它指定的回调函数

当主线程时间执行过长，无法保证回调会在事件指定的时间执行

浏览器端每次 setTimeout 会有 4ms 的延迟，当连续执行多个 setTimeout，有可能会阻塞进程，造成性能问题

### setImmediate()

事件插入到事件队列尾部，主线程和事件队列的函数执行完成之后立即执行，和 setTimeout(fn,0)的效果差不多

服务端 node 提供的方法。浏览器端最新的 api 也有类似实现:window.setImmediate,但支持的浏览器很少

### process.nextTick()

插入到事件队列尾部，但在下次事件队列之前会执行

也就是说，它指定的任务总是发生在所有异步任务之前，当前主线程的末尾

服务器端 node 提供的办法，用此方法可以用于处于异步延迟的问题

---

<span id="jump4"></span>

##  js 延迟加载的方式有哪些

js 的加载、解析和执行会阻塞页面的渲染过程，因此我们希望 js 脚本能够尽可能的延迟加载，提高页面的渲染速度

- 给 js 脚本添加 defer 属性

- 给 js 脚本添加 async 属性

- 对文档的加载事件进行监听，当文档加载完成后再动态的创建 script 标签来引入 js 脚本

- XmlHttpRequest 脚本注入

- 异步加载库 LABjs、模块加载器 Sea.js

---

<span id="jump5"></span>

## script的defer和async

两者与未添加该属性的script的共同区别是，script文件下载时不会阻塞html解析

defer：并行加载 js 文件，等待所有文件均下载完后，才按照页面上 script 标签的顺序执行

async：并行加载 js 文件，下载完成立即执行，不会按照页面上 script 标签的顺序执行