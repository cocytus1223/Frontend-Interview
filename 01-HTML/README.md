# HTML 类

## HTML 全局属性(global attribute)有哪些

- accesskey:设置快捷键，提供快速访问元素如 aaa 在 windows 下的 firefox 中按 alt + shift + a 可激活元素
- class:为元素设置类标识，多个类名用空格分开，CSS 和 javascript 可通过 class 属性获取元素
- contenteditable: 指定元素内容是否可编辑
- contextmenu: 自定义鼠标右键弹出菜单内容
- data-\*: 为元素增加自定义属性
- dir: 设置元素文本方向
- draggable: 设置元素是否可拖拽
- dropzone: 设置元素拖放类型： copy, move, link
- hidden: 表示一个元素是否与文档。样式上会导致元素不显示，但是不能用这个属性实现样式效果
- id: 元素 id，文档内唯一
- lang: 元素内容的的语言
- spellcheck: 是否启动拼写和语法检查
- style: 行内 css 样式
- tabindex: 设置元素可以获得焦点，通过 tab 可以导航
- title: 元素相关的建议信息
- translate: 元素和子孙节点内容是否需要本地化

## doctype 是什么？

- DTD（document type definition，文档类型定义）是一系列的语法规则，用来定义 XML 或(X)HTML 的文件类型。浏览器会使用它来判断文档类型，决定使用何种协议来解析，以及切换浏览器模式。
  解读：DTD 就是告诉浏览器我是什么文档类型，那么浏览器根据这个来判断用什么引擎来解析渲染他。

- DOCTYPE 是用来声明文档类型和 DTD 规范的，一个主要的用途便是文件的合法性验证。如果文件代码不合法，那么浏览器解析时便会出一些差错。
  解读：DOCTYPE 通知浏览器当前文档的 DTD。

- 常见的 DOCTYPE

1. HTML 5
   `!DOCTYPE html`
2. HTML 4.01 Strict，严格模式，该 DTD 包含所有 HTML 元素和属性，但不包括展示型的和弃用的元素（比如 font）
   `<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">`
3. HTML 4.01 Transitional，传统模式（宽松模式），该 DTD 包含所有 HTML 元素和属性，包括展示型的和弃用的元素（比如 font）
   `<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">`

## H5 新增了哪些新属性？

1. 离线缓存。可以在关闭浏览器后再次打开时恢复数据，以减少网络流量。
2. 音频视频自由嵌入，多媒体形式更为灵活。
3. 地理定位。地理位置定位，让定位和导航不再专属导航软件，地图也不用下载非常大的地图包，可以通过缓存来解决，到哪儿下哪儿，更灵活。
4. Canvas 绘图，提升移动平台的绘图能力。使用 Canvas API 可以简单绘制热点图收集用户体验资料，支持图片的移动、旋转、缩放等常规编。
5. 丰富的交互方式。提升互动能力：拖拽、撤销历史操作、文本选择等。
6. 开发及维护成本低，这个相对于原生 APP 开发来说。更低的开发及维护成本；使页面变得更小，减少了用户不必要的支出;而且，性能更好使. 电量更低。
7. html5 调用手机摄像头和手机相册、通讯录等功能。

## 请描述`<script>`、`<script async>`和`<script defer>`的区别。

- `<script>` - HTML 解析中断，脚本被提取并立即执行。执行结束后，HTML 解析继续。
- `<script async>` - 脚本的提取、执行的过程与 HTML 解析过程并行，脚本执行完毕可能在 HTML 解析完毕之前。当脚本与页面上其他脚本独立时，可以使用 async，比如用作页面统计分析。
- `<script defer>` - 脚本仅提取过程与 HTML 解析过程并行，脚本的执行将在 HTML 解析完毕后进行。如果有多个含 defer 的脚本，脚本的执行顺序将按照在 document 中出现的位置，从上到下顺序执行。

**注意**：没有 src 属性的脚本，async 和 defer 属性会被忽略。

## 什么是渐进式渲染（progressive rendering）？

渐进式渲染是用于提高网页性能（尤其是提高用户感知的加载速度），以尽快呈现页面的技术。

在以前互联网带宽较小的时期，这种技术更为普遍。如今，移动终端的盛行，而移动网络往往不稳定，渐进式渲染在现代前端开发中仍然有用武之地。

一些举例：

- 图片懒加载——页面上的图片不会一次性全部加载。当用户滚动页面到图片部分时，JavaScript 将加载并显示图像。
- 确定显示内容的优先级（分层次渲染）——为了尽快将页面呈现给用户，页面只包含基本的最少量的 CSS、脚本和内容，然后可以使用延迟加载脚本或监听 DOMContentLoaded/load 事件加载其他资源和内容。
- 异步加载 HTML 片段——当页面通过后台渲染时，把 HTML 拆分，通过异步请求，分块发送给浏览器。

## 为什么在`<img>`标签中使用 srcset 属性？请描述浏览器遇到该属性后的处理过程。

因为需要设计响应式图片。我们可以使用两个新的属性——`srcset` 和 `sizes`——来提供更多额外的资源图像和提示，帮助浏览器选择正确的一个资源。

`srcset` 定义了我们允许浏览器选择的图像集，以及每个图像的大小。

`sizes` 定义了一组媒体条件（例如屏幕宽度）并且指明当某些媒体条件为真时，什么样的图片尺寸是最佳选择。

所以，有了这些属性，浏览器会：

- 查看设备宽度
- 检查 sizes 列表中哪个媒体条件是第一个为真
- 查看给予该媒体查询的槽大小
- 加载 srcset 列表中引用的最接近所选的槽大小的图像

## 常用的 meta 标签有哪些

```html
//声明文档使用的字符编码 <meta charset="utf-8" />

//优先使用 IE 最新版本和 Chrome
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />

//页面描述 <meta name="description" content="网页描述" />

//页面关键词 <meta name="keywords" content="" />

//网页作者 <meta name="author" content="name, email@gmail.com" />

//搜索引擎抓取 <meta name="robots" content="index,follow" />

//为移动设备添加 viewport
<meta
  name="viewport"
  content="initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no"
/>

//添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）
<meta
  name="apple-itunes-app"
  content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL"
/>

//设置苹果工具栏颜色
<meta name="apple-mobile-web-app-status-bar-style" content="black" />

//启用360浏览器的极速模式(webkit) <meta name="renderer" content="webkit" />

//避免IE使用兼容模式 <meta http-equiv="X-UA-Compatible" content="IE=edge" />

//不让百度转码 <meta http-equiv="Cache-Control" content="no-siteapp" />

//针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓
<meta name="HandheldFriendly" content="true" />

//微软的老式浏览器 <meta name="MobileOptimized" content="320″> //uc强制竖屏
<meta name="screen-orientation" content="portrait" />

//QQ强制竖屏 <meta name="x5-orientation" content="portrait" />

//UC强制全屏 <meta name="full-screen" content="yes" />

//QQ强制全屏 <meta name="x5-fullscreen" content="true" />

//UC应用模式 <meta name="browsermode" content="application" />

//QQ应用模式 <meta name="x5-page-mode" content="app" />

//windows phone 点击无高光
<meta name="msapplication-tap-highlight" content="no" />

//设置页面不缓存 <meta http-equiv="pragma" content="no-cache" />
<meta http-equiv="cache-control" content="no-cache" />
<meta http-equiv="expires" content="0" />
```
