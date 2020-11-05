# BOM

## 目录

[offsetWidth/offsetHeight,clientWidth/clientHeight 与 scrollWidth/scrollHeight 的区别](#jump1)

[描述浏览器的渲染过程，DOM 树和渲染树的区别](#jump2)

[重绘和回流（重排）的区别和关系](#jump3)

[如何最小化重绘(repaint)和回流(reflow)](#jump4)

[](#jump)

[](#jump)

[](#jump)

[](#jump)

[](#jump)

---

<span id="jump1"></span>

## 

---

<span id="jump2"></span>

## 描述浏览器的渲染过程，DOM 树和渲染树的区别

### 浏览器的渲染过程

- 解析 HTML， 构建 DOM(DOM 树)，并行请求 css/image/js

- CSS 文件下载完成，开始构建 CSSOM(CSS 树)

- CSSOM 构建结束后，和 DOM 一起生成 Render Tree(渲染树)

- 布局(Layout)：计算出每个节点在屏幕中的位置

- 显示(Painting)：通过显卡把页面画到屏幕上

### DOM 树 和 渲染树 的区别

- DOM 树与 HTML 标签一一对应，包括 head 和隐藏元素

- 渲染树不包括 head 和隐藏元素

---

<span id="jump3"></span>

## 重绘和回流（重排）的区别和关系

- 重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘

- 回流：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生回流

- 回流必将引起重绘，而重绘不一定会引起回流

-JS 获取 Layout 属性值（如：offsetLeft、scrollTop、getComputedStyle 等）也会引起回流。因为浏览器需要通过回流计算最新值

---

<span id="jump4"></span>

## 如何最小化重绘(repaint)和回流(reflow)

- 需要要对元素进行复杂的操作时，可以先隐藏(display:"none")，操作完成后再显示

- 需要创建多个 DOM 节点时，使用 DocumentFragment 创建完后一次性的加入 document

- 缓存 Layout 属性值，如：var left = elem.offsetLeft; 这样，多次使用 left 只产生一次回流

- 尽量避免用 table 布局（table 元素一旦触发回流就会导致 table 里所有的其它元素回流）

- 避免使用 css 表达式(expression)，因为每次调用都会重新计算值（包括加载页面）

- 批量修改元素样式：elem.className 和 elem.style.cssText 代替 elem.style.xxx

---

<span id="jump"></span>

## 