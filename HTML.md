# HTML

## 目录

[doctype是什么](#jump1)

[Semantics](#jump2)

[inline, block and void elements](#jump3)

[Difference between link and @import](#jump4)

[浏览器内核](#jump5)

[h5新特性](#jump6)

[H5 Application Cache](#jump6)

[canvas & svg](#jump7)

[script 的位置是否会影响首屏显示时间？](#jump8)

[](#jump)

---	

<span id="jump1"></span>

## doctype是什么

### What's it for

Tell the browser's parser what document standard to parse this document

The absence of DOCTYPE or incorrect format will cause the document to be rendered in qurik mode

### Standar mode and quirk mode

Standar mode: Page elements arrangement and JS operating modes are both run at the highest standards supported by the browser

Quirk mode: The page is displayed in a loosely backward compatible way, simulating the behavior of old browsers to prevent the site from not working

---

<span id="jump2"></span>

## Semantics 

### Semantics in CSS

Adding semantic class, id to html lables to make the purpose of these lables more easy to understand

### Semantics in HTML

Making the html document structure more clear by using semantic lables like h1-h6

#### benefits

Search engines will consider its contents as important keywords

Finding blocks of meaningful code is significantly easier than searching though endless divs

Screen readers can use it as a signpost to help visually impaired users navigate a page

---

<span id="jump3"></span>

## inline, block and void elements

inline:

```
a b span img input select strong
```

block:

```
div ul ol li dl dt dd h p
```

void:

```
<br> <hr> <img> <input> <link> <meta>
```

---

<span id="jump4"></span>

## Difference between link and @import

- link is a XHTML lable, except CSS, it could also defines RSS, rel atributes; @import only for CSS

- When page is loading, link will be loaded at the same time; @import after page loaded

- @import CSS2.1, only for IE5 above; link doesn't have compatibility problem

---

<span id="jump5"></span>

## 浏览器内核

### Rendering Engine and JS Engine

2 main parts: Rendering Engine and JS Engine

- rendering engine: layout and style rendering, then output to the monitor or printer

- JS engine: parsing and executing JS scripts, implements  dynamic effects and user interaction

At first, the rendering engine and the JS engine were not clearly distinguishedAt first, the rendering engine and the JS engine were not clearly distinguished

Later, the JS engine became more and more independent, and the kernel tended to refer to the rendering engine

### Common browser kernel

Blink内核：新版 Chrome、新版 Opera

Webkit内核：Safari、原Chrome

Gecko内核：FireFox、Netscape6及以上版本

Trident内核（又称MSHTML内核）：IE、国产浏览器

Presto内核：原Opera7及以上

---

<span id="jump"></span>

## h5新特性

```
新增选择器 document.querySelector、document.querySelectorAll
拖拽释放(Drag and drop) API
媒体播放的 video 和 audio
本地存储 localStorage 和 sessionStorage
离线应用 manifest
桌面通知 Notifications
语意化标签 article、footer、header、nav、section
增强表单控件 calendar、date、time、email、url、search
地理位置 Geolocation
多任务 webworker
全双工通信协议 websocket
历史管理 history
跨域资源共享(CORS) Access-Control-Allow-Origin
页面可见性改变事件 visibilitychange
跨窗口通信 PostMessage
Form Data 对象
绘画 canvas
```

---

<span id="jump6"></span>

## H5 Application Cache

### When online

the browser finds manifest attribute, then request the manifest file

If it is the first time to access the app, the browser will download resources according to manifest file and store them offline

If the app has been accessed and the resource has been stored offline, the browser will compare the new manifest file with the old

If the file is changed, the resources in the file will be downloaded again and stored offline

### When offline

The browser directly uses the resources stored offline

---

<span id="jump7"></span>

## canvas & svg

### 区别1

canvas is a h5 new lable

svg is not a html5 exclusive tag, initially svg uses xml

### 区别2

canvas draws bitmap, bitmap is good at display multible color and color layers, so it's used for games or graphics

svg draws Vector Graphic, Vector Graphic can be infinitely amplified without distortion, so it's used for icons and map

### 区别3

Graphics drawn in canvas cannot be captured by the search engine

svg could

### 区别4

canvas doesn't support event binding

svg supports

---

<span id="jump8"></span>

## script 的位置是否会影响首屏显示时间？

- 在解析 HTML 生成 DOM 过程中，js 文件的下载是并行的，不需要 DOM 处理到 script 节点。因此，script 的位置不影响首屏显示的开始时间

- 浏览器解析 HTML 是自上而下的线性过程，因此script 会延迟 DomContentLoad，只显示其上部分首屏内容，从而影响首屏显示的完成时间

