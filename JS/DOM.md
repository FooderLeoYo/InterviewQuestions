# DOM

## 目录

[documen.write 和 innerHTML 的区别](#jump1)

[介绍 DOM 的发展](#jump2)

[介绍 DOM0，DOM2，DOM3 事件处理方式区别](#jump3)

[IE 的事件处理和 W3C 的事件处理有哪些区别？](#jump4)

[W3C 事件的 target 与 currentTarget 的区别？](#jump5)

[JS 获取 dom 的 CSS 样式](#jump6)

[在一个 DOM 上同时绑定两个点击事件：一个用捕获，一个用冒泡。事件会执行几次，先执行冒泡还是捕获？](#jump7)

[如何派发事件(dispatchEvent)？（如何进行事件广播？）](#jump8)

[移动端的点击事件的有延迟，时间是多久，为什么会有？ 怎么解决这个延时](#jump9)

[区分什么是“客户区坐标”、“页面坐标”、“屏幕坐标”](#jump10)

[focus/blur 与 focusin/focusout 的区别与联系](#jump11)

[mouseover/mouseout 与 mouseenter/mouseleave 的区别与联系](#jump12)

[offsetWidth/offsetHeight,clientWidth/clientHeight 与 scrollWidth/scrollHeight 的区别](#jump13)

[区分什么是“客户区坐标”、“页面坐标”、“屏幕坐标”](#jump14)

[attribute 和 property 的区别是什么？](#jump15)

[](#jump)

[](#jump)

---

<span id="jump1"></span>

## documen.write 和 innerHTML 的区别

document.write 的内容会代替整个文档内容，会重写整个页面

innerHTML 的内容只是替代指定元素的内容，只会重写页面中的部分内容

---

<span id="jump2"></span>

## 介绍 DOM 的发展

- DOM：文档对象模型（Document Object Model），定义了访问 HTML 和 XML 文档的标准，与编程语言及平台无关

- DOM0：提供了查询和操作 Web 文档的内容 API，未形成标准

- DOM1：W3C 提出标准化的 DOM，简化了对文档中任意部分的访问和操作

- DOM2：原来 DOM 基础上扩充了鼠标事件等细分模块，增加了对 CSS 的支持

- DOM3：增加了 XPath 模块和加载与保存（Load and Save）模块

---

<span id="jump3"></span>

## 介绍 DOM0，DOM2，DOM3 事件处理方式区别

### dom0 事件的特点

1. dom0 事件就是直接通过 onclick 绑定到 html上的事件

	```javascript
	<input onclick="xx"/>
	```

	或：

	```javascript
	input.onclick = function(){  ...  }
	```

2. 清理dom0 事件时，只需给该事件赋值为 null

	```javascript
	input.onclick = null
	```

3. 同一个元素的同一种事件只能绑定一个函数，否则后面的函数会覆盖之前的函数

4. 不存在兼容性问题

### dom2 事件的特点

1. 事件是通过 addEventListener 绑定的事件
	
	```javascript
	btn.addEventListener('click', func, false);
	btn.attachEvent("onclick", func); // IE
	```

2. 同一个元素的同种事件可以绑定多个函数，按照绑定顺序执行

3. 清除事件时，使用 removeEventListener

	```javascript
	btn.removeEventListener('click', func, false);
	btn.detachEvent("onclick", func); // IE
	```

### DOM3 

1. DOM3级事件在DOM2级事件的基础上添加了更多的事件类型

2. 同时DOM3级事件也允许使用者自定义一些事件

绑定事件：

```javascript
eventUtil.addListener(input, "textInput", func);
```

---

<span id="jump4"></span>

## IE 的事件处理和 W3C 的事件处理有哪些区别？

### 绑定事件

W3C: targetEl.addEventListener('click', handler, false);

IE: targetEl.attachEvent('onclick', handler);

### 删除事件

W3C: targetEl.removeEventListener('click', handler, false);

IE: targetEl.detachEvent(event, handler);

### 事件目标

W3C: e.target

IE: window.event.srcElement

### 阻止事件默认行为

W3C: e.preventDefault()

IE: window.event.returnValue = 'false'

### 阻止事件传播

W3C: e.stopPropagation()

IE: window.event.cancelBubble = true

---

<span id="jump5"></span>

## W3C 事件的 target 与 currentTarget 的区别？

target 只会出现在事件流的目标阶段

currentTarget 可能出现在事件流的任何阶段

当事件流处在目标阶段时，二者的指向相同

当事件流处于捕获或冒泡阶段时：currentTarget 指向当前事件活动的对象(一般为父级)

---

<span id="jump6"></span>

## JS 获取 dom 的 CSS 样式

```javascript
function getStyle(obj, attr) {
  if (obj.currentStyle) {
    return obj.currentStyle[attr];
  } else {
    return window.getComputedStyle(obj, false)[attr];
  }
}
```

---

<span id="jump7"></span>

## 在一个 DOM 上同时绑定两个点击事件：一个用捕获，一个用冒泡。事件会执行几次，先执行冒泡还是捕获？

- 该 DOM 上的事件如果被触发，会执行两次（执行次数等于绑定次数）

- 如果该 DOM 是目标元素，则按事件绑定顺序执行，不区分冒泡/捕获

- 如果该 DOM 是处于事件流中的非目标元素，则先执行捕获，后执行冒泡

---

<span id="jump8"></span>

## 如何派发事件(dispatchEvent)？（如何进行事件广播？）

W3C: 使用 dispatchEvent 方法

IE: 使用 fireEvent 方法

---

<span id="jump9"></span>

## 移动端的点击事件的有延迟，时间是多久，为什么会有？ 怎么解决这个延时

### 原因

移动端的click事件可以拆解为：touchstart -> touchmove -> touchend -> click

浏览器在 touchend 之后会等待约 300ms ，如果没有 tap 行为，则触发 click 事件， click 事件触发代表一轮触摸事件的结束

浏览器等待约 300ms 的原因是：浏览器在 click 之后要等待 300ms，看用户有没有下一次点击，来判断这次操作是不是双击

点击穿透：在这 300ms 以内，如果上层元素隐藏或消失了，由于 click 事件的滞后性，300ms后将会触发隐藏元素之下的元素的click事件

### 解决

1.通过 meta 标签禁用网页的缩放

2.通过 meta 标签将网页的 viewport 设置为 ideal viewport

3.调用一些 js 库，比如 FastClick

---

<span id="jump10"></span>

## 区分什么是“客户区坐标”、“页面坐标”、“屏幕坐标”

### 客户区坐标

鼠标指针在可视区中的水平坐标(clientX)和垂直坐标(clientY)

### 页面坐标

鼠标指针在页面布局中的水平坐标(pageX)和垂直坐标

### 屏幕坐标

设备物理屏幕的水平坐标(screenX)和垂直坐标(screenY)

---

<span id="jump11"></span>

## focus/blur 与 focusin/focusout 的区别与联系

- focus/blur 不冒泡，focusin/focusout 冒泡

- focus/blur 兼容性好，focusin/focusout 在除 FireFox 外的浏览器下都保持良好兼容性

---

<span id="jump12"></span>

## mouseover/mouseout 与 mouseenter/mouseleave 的区别与联系

- mouseover/mouseout 是标准事件，所有浏览器都支持；mouseenter/mouseleave 是 IE5.5 引入的特有事件后来被 DOM3 标准采纳，现代标准浏览器也支持

- mouseover/mouseout 是冒泡事件；mouseenter/mouseleave不冒泡。需要为多个元素监听鼠标移入/出事件时，推荐 mouseover/mouseout 托管，提高性能

---

<span id="jump13"></span>

## offsetWidth/offsetHeight,clientWidth/clientHeight 与 scrollWidth/scrollHeight 的区别

- offsetWidth/offsetHeight 返回值包含 content + padding + border，效果与 e.getBoundingClientRect()相同

- clientWidth/clientHeight 返回值只包含 content + padding，如果有滚动条，也不包含滚动条

- scrollWidth/scrollHeight 返回值包含 content + padding + 溢出内容的尺寸

---

<span id="jump14"></span>

## 区分什么是“客户区坐标”、“页面坐标”、“屏幕坐标”

- 客户区坐标：鼠标指针在可视区中的水平坐标(clientX)和垂直坐标(clientY)相对于浏览器的左上定点为原点

- 页面坐标：鼠标指针在页面布局中的水平坐标(pageX)和垂直坐标(pageY)相对于Document对象即文档窗口的左上顶点为坐标原点

- 屏幕坐标：设备物理屏幕的水平坐标(screenX)和垂直坐标(screenY)

---

<span id="jump15"></span>

## attribute 和 property 的区别是什么？

attribute 是 dom 元素在文档中作为 html 标签拥有的属性；

property 就是 dom 元素在 js 中作为对象拥有的属性。

对于 html 的标准属性来说，attribute 和 property 是同步的，是会自动更新的

但是对于自定义的属性来说，他们是不同步的