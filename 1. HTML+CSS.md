# 1. HTML + CSS

## 1.1. 什么是盒子模型？

- 所有 HTML 元素都可以看作是一个盒子，包括 margin（外边距）、padding（内边距）、border（边框）、content（内容）
- IE 盒子模型的 content 部分包含了 border 和 padding，标准盒子模型不包括。
- W3C：box-sizing:content-box;（浏览器默认）
  IE：box-sizing:border-box;

## 1.2. 定位有几种方式？每种方式定位的基准是什么吗？那种方式会脱标？他们之间会有层级关系吗，谁的层级会高些？

| 定位方式                     | 定位基准                       | 是否脱标 |
| ---------------------------- | ------------------------------ | -------- |
| 静态定位`position:static;`   | 所有的标准流都是静态定位       | 否       |
| 相对定位`position:relative;` | 自己的标准流                   | 否       |
| 绝对定位`position:absolute;` | 绝对元素往上找的第一个定位元素 | 是       |
| 固定定位`position: fixed;`   | 浏览器窗口                     | 是       |

## 1.3. 浮动会带来哪些影响？哪些方式可以清除浮动？

影响：

1. 脱标

清除浮动：

1. 强行给父盒子添加高度 （不推荐，不利于后期维护）
2. 创建一个新的块级盒子放在浮动元素的最后面，给这个盒子添加一个 css 属性：clear:both;（不推荐，会产生一个多余的盒子）
3. 伪元素清除浮动 （推荐，书写一个公共类就可以使用）

   ```css
   .clearfix::after {
     content: '';
     display: block;
     clear: both;
     height: 0;
     visibility: hidden;
   }
   ```

4. 给父元素添加 overflow:hidden; (在某些特定场景下使用不了)

## 1.4. 如何让一个不定宽高的 div 垂直水平居中？

1. flex 布局

   ```css
   .box{
      display: flex;   <!-- 弹性布局 -->
      justify-content: space-around;  <!-- 水平居中 -->
      align-items: center;  <!-- 垂直居中 -->
   }
   ```

2. 绝对定位和 transfrom

   ```css
   .box {
     position: absolute;
     top: 50%;
     left: 50%;
     transform: translate(-50%, -50%);
   }
   ```

3. 定位和 `margin:auto;`

4. `text-align:center`实现元素的水平居中，以及通过设置元素的 height 和 line-height 相同来实现子元素的垂直居中

5. `diplay：table-cell`
   把其变成表格样式，再利用表格的样式来进行居中

## 1.5. H5/C3 增加了哪些新属性，分别列举一下？

1. 离线缓存。可以在关闭浏览器后再次打开时恢复数据，以减少网络流量。
2. 音频视频自由嵌入，多媒体形式更为灵活。
3. 地理定位。地理位置定位，让定位和导航不再专属导航软件，地图也不用下载非常大的地图包，可以通过缓存来解决，到哪儿下哪儿，更灵活
4. Canvas 绘图，提升移动平台的绘图能力。使用 Canvas API 可以简单绘制热点图收集用户体验资料，支持图片的移动、旋转、缩放等常规编. 。
5. 丰富的交互方式。提升互动能力：拖拽、撤销历史操作、文本选择等。
6. 开发及维护成本低，这个相对于原生 APP 开发来说。更低的开发及维护成本；使页面变得更小，减少了用户不必要的支出;而且，性能更好使. 电量更低。
7. CSS3 视觉设计师的辅助利器的支持。CSS3 支持了字体的嵌入、版面的排版，以及最令人印象深刻的动画功能。
8. html5 调用手机摄像头和手机相册、通讯录等功能。

## 1.6. H5/C3 新属性如何做兼容处理？（重要）

## 1.7. Css 如何画一个三角形？

- css 画三角形代码原理采用的是均分原理；
- 在矩形的直角，两条边的样式要均分，把 div 的宽高设为零，四条边 top right bottom left 设置一个宽度
- 把需要显示的一边设置有色，其他的设置透明，就这样一个三角形就出来了

```css
.box {
  　　width: 0;
  　　height: 0;
  　　border-top: 50px solid transparent;
  　　border-right: 50px solid red;
  　　border-bottom: 50px solid transparent;
  　　border-left: 50px solid transparent;
}
```

## 1.8. HTML5 应用程序缓存和浏览器缓存有什么区别？

HTML5 引入了应用程序缓存，这意味着 web 应用可进行缓存，并可在没有因特网连接时进行访问。
应用程序缓存为应用带来三个优势：

- 离线浏览 - 用户可在应用离线时使用它们
- 速度 - 已缓存资源加载得更快
- 减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源。

实现借助于 manifest 文件
`<html manifest="demo.appcache">`

## 1.9. 页面之间的参数传递有哪些方式？

## 1.10. 浏览器缓存和离线缓存的区别是什么吗？

## 1.11. 为什么会有缓存？如果不想缓存如何避免掉？

## 1.12. H5 的 wbsocket 是什么？应用场景有哪些？

## 1.13. http 以及 https 的区别是什么？

http 和 https 使用的是完全不同的连接方式,用的端口也不一样,前者是 80,后者是 443。
HTTPS 是以安全为目标的 HTTP 通道，简单讲是 HTTP 的安全版。
由于 https 要还密钥和确认加密算法的过程，所以更安全。

## 1.14. em 和 rem 的区别？

## 1.15. 移动端页面如何来做适配？

## 1.16. JS 如何设置获取盒模型对应的宽和高

- dom.style.width/height
     这种方式只能取到 dom 元素内联样式所设置的宽高，也就是说如果该节点的样式是在 style 标签中或外联的 CSS 文件中设置的话，通过这种方法是获取不到 dom 的宽高的。

- dom.currentStyle.width/height
     这种方式获取的是在页面渲染完成后的结果，就是说不管是哪种方式设置的样式，都能获取到。但这种方式只有 IE 浏览器支持。

- window.getComputedStyle(dom).width/height
     这种方式的原理和 2 是一样的，这个可以兼容更多的浏览器，通用性好一些。

- dom.getBoundingClientRect().width/height
     这种方式是根据元素在视窗中的绝对位置来获取宽高的。

- dom.offsetWidth/offsetHeight
     这个就没什么好说的了，最常用的，也是兼容最好的。

## 1.17. css reset 和 normalize.css 有什么区别

- 两者都是通过重置样式，保持浏览器样式的一致性；
- 前者几乎为所有标签添加了样式，后者保持了许多浏览器样式，保持尽可能的一致；
- 后者修复了常见的桌面端和移动端浏览器的 bug：包含了 HTML5 元素的显示设置、预格式化文字的 font-size 问题、在 IE9 中 SVG 的溢出、许多出现在各浏览器和操作系统中的与表单相关的 bug。
- 前者中含有大段的继承链；
- 后者模块化，文档较前者来说丰富；

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

## 1.19. window.onload 和 document ready 的区别？

- `window.onload` 是在 dom 文档树加载完和所有文件加载完之后执行一个函数， `Document.ready` 原生没有这个方法，jquery 中有 `$().ready(function)`,在 dom 文档树加载完之后执行一个函数（注意，这里面的文档树加载完不代表全部文件加载完）
- `$(document).ready` 要比 `window.onload` 先执行
- `window.onload` 只能出来一次，`$(document).ready` 可以出现多次
