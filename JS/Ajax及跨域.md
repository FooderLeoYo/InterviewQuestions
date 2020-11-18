# Ajax及跨域

## 目录

[Ajax 是什么? 如何创建一个 Ajax？](#jump1)

[ajax事件触发条件](#jump4)

[ajax事件执行顺序](#jump5)

[什么是浏览器的同源政策？](#jump2)

[如何解决跨域问题？](#jump3)

[](#jump6)

[](#jump)

---	

<span id="jump1"></span>

## Ajax 是什么? 如何创建一个 Ajax？

它是一种异步通信的方法，向服务器发送请求的时候，我们不必等待结果，而是可以同时做其他的事情，等到有了结果它自己会根据设定进行后续操作，与此同时，页面是不会发生整页刷新的，提高了用户体验

创建一个 ajax 有这样几个步骤：

1. 创建 XMLHttpRequest 对象，也就是创建一个异步调用对象

2. 使用open方法设置请求的参数。`open(method, url, 是否异步)``

3. 设置响应 HTTP 请求状态变化的函数

4. 发送 HTTP 请求

5. 注册事件。 注册onreadystatechange事件，状态改变时就会调用

6. 获取返回的数据，更新UI

---

<span id="jump4"></span>

## ajax事件触发条件

[ajax事件触发条件](https://raw.githubusercontent.com/FooderLeoYo/InterviewQuestions/master/assets/imgs/ajax%E4%BA%8B%E4%BB%B6%E8%A7%A6%E5%8F%91%E6%9D%A1%E4%BB%B6.png)

---

<span id="jump5"></span>

## ajax事件执行顺序

[ajax事件执行顺序](https://raw.githubusercontent.com/FooderLeoYo/InterviewQuestions/master/assets/imgs/ajax%E4%BA%8B%E4%BB%B6%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F.png)

---

<span id="jump2"></span>

## 什么是浏览器的同源政策？

一个域下的 js 脚本在未经允许的情况下，不能够访问另一个域的内容，同源政策的目的主要是为了保证用户的信息安全

里的同源的指的是两个域的协议、域名、端口号必须相同，否则则不属于同一个域

同源政策主要限制了三个方面：

- 当前域下的 js 脚本不能够访问其他域下的 cookie、localStorage 和 indexDB

- 当前域下的 js 脚本不能够操作访问操作其他域下的 DOM

- 当前域下 ajax 无法发送跨域请求

---

<span id="jump3"></span>

## 如何解决跨域问题？

解决跨域的方法我们可以根据我们想要实现的目的来划分

### 实现主域名下的不同子域名的跨域操作

- 两个页面都通过js强制设置document.domain为基础主域

### ajax 无法提交跨域请求

- 使用 jsonp 来实现跨域请求，它的主要原理是通过动态构建script标签来实现跨域请求，因为浏览器对 script 标签的引入没有跨域的访问限制，这种方式只能用于 get 请求。

- 使用 CORS 的方式，CORS请求分成两类：简单请求和非简单请求。

	- 对于简单请求，只服务端设置Access-Control-Allow-Origin即可，前端无须设置，若要带cookie请求：前后端都需要设置

	- 非简单请求，浏览器会先发出一次预检请求，来判断该域名是否在服务器的白名单中，如果收到肯定回复后才会发起请求

- 使用 websocket 协议，这个协议没有同源限制

- 使用服务器来代理跨域的访问请求，就是有跨域的请求操作时发送请求给后端，让后端代为请求。如： nginx反向代理、Nodejs中间件代理

### 解决不同跨域窗口间的通信问题

- 使用location.hash + iframe的方法：a欲与b跨域相互通信，通过中间页c来实现。三个页面，不同域之间利用iframe的location.hash传值，相同域之间直接js访问来通信

- 使用 window.name + iframe 的方法：window.name属性的独特之处：name值在不同的页面（甚至不同域名）加载后依旧存在，并且可以支持非常长的 name 值（2MB）

- 使用 postMessage  

