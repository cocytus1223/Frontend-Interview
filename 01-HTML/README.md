## 1.18. HTML 全局属性(global attribute)有哪些

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

## 1.5. doctype 是什么？

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

## 1.5. H5 新增了哪些新属性？

1. 离线缓存。可以在关闭浏览器后再次打开时恢复数据，以减少网络流量。
2. 音频视频自由嵌入，多媒体形式更为灵活。
3. 地理定位。地理位置定位，让定位和导航不再专属导航软件，地图也不用下载非常大的地图包，可以通过缓存来解决，到哪儿下哪儿，更灵活
4. Canvas 绘图，提升移动平台的绘图能力。使用 Canvas API 可以简单绘制热点图收集用户体验资料，支持图片的移动、旋转、缩放等常规编. 。
5. 丰富的交互方式。提升互动能力：拖拽、撤销历史操作、文本选择等。
6. 开发及维护成本低，这个相对于原生 APP 开发来说。更低的开发及维护成本；使页面变得更小，减少了用户不必要的支出;而且，性能更好使. 电量更低。
7. CSS3 视觉设计师的辅助利器的支持。CSS3 支持了字体的嵌入、版面的排版，以及最令人印象深刻的动画功能。
8. html5 调用手机摄像头和手机相册、通讯录等功能。