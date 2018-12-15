# CSS 类

## 什么是盒子模型？

- 所有 HTML 元素都可以看作是一个盒子，包括 margin（外边距）、padding（内边距）、border（边框）、content（内容）
- IE 盒子模型的 content 部分包含了 border 和 padding，标准盒子模型不包括。
- W3C：box-sizing:content-box;（浏览器默认）
  IE：box-sizing:border-box;

## 定位有几种方式？

经过定位的元素，其 `position` 属性值必然是 `relative`、`absolute`、`fixed` 或 `sticky`

- `static`：默认定位属性值。该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 top, right, bottom, left 和 z-index 属性无效。
- `relative`：该关键字下，元素先放置在未添加定位时的位置，再在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）。
- `absolute`：不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素位置。绝对定位的元素可以设置外边距（margins），且不会与其他边距合并。
- `fixed`：不为元素预留空间，而是通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。打印时，元素会出现在的每页的固定位置。fixed 属性会创建新的层叠上下文。当元素祖先的 transform 属性非 none 时，容器由视口改为该祖先。
- `sticky`：在屏幕范围（viewport）时该元素的位置并不受到定位影响（设置是 top、left 等属性无效），当该元素的位置将要移出偏移范围时，定位又会变成 fixed，根据设置的 left、top 等属性成固定位置的效果。

## 浮动会带来哪些影响？哪些方式可以清除浮动？

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

## 如何让一个不定宽高的 div 垂直水平居中？

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

## Css 如何画一个三角形？

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

## 重置（resetting）CSS 和 标准化（normalizing）CSS 的区别是什么？你会选择哪种方式，为什么？

- 重置（Resetting）： 重置意味着除去所有的浏览器默认样式。对于页面所有的元素，像 margin、padding、font-size 这些样式全部置成一样。几乎为所有标签添加了样式，你将必须重新定义各种元素的样式。
- 标准化（Normalizing）： 标准化没有去掉所有的默认样式，而是保留了有用的一部分，同时还修复了常见的桌面端和移动端浏览器的 bug：包含了 HTML5 元素的显示设置、预格式化文字的 font-size 问题、在 IE9 中 SVG 的溢出、许多出现在各浏览器和操作系统中的与表单相关的 bug。

- 两者都是通过重置样式，保持浏览器样式的一致性；

## CSS 选择器的优先级是如何计算的？

> 继承 < 通配符 < 标签选择器 < 类选择器 < ID 选择器 < 行内样式 <　!important

关于 CSS 选择器的优先级，我们需要一套计算公式来去计算，这个就是 CSS Specificity，我们称为 CSS 特性或称非凡性，它是一个衡量 CSS 值优先级的一个标准 具体规范入如下：
specificity 用一个四位的数 字串(CSS2 是三位)来表示，更像四个级别，值从左到右，左面的最大，一级大于一级，数位之间没有进制，级别之间不可超越。

| 样式                     | specificity |
| ------------------------ | ----------- |
| 继承或者\* 的贡献值      | 0,0,0,0     |
| 每个元素（标签）贡献值为 | 0,0,0,1     |
| 每个类，伪类贡献值为     | 0,0,1,0     |
| 每个 ID 贡献值为         | 0,1,0,0     |
| 每个行内样式贡献值       | 1,0,0,0     |
| 每个!important 贡献值    | 正无穷      |

## 请阐述 `z-index` 属性，并说明如何形成层叠上下文（stacking context）。

CSS 中的 `z-index` 属性控制重叠元素的垂直叠加顺序。`z-index` 只能影响 `position` 值不是 `static` 的元素。

没有定义 `z-index` 的值时，元素按照它们出现在 DOM 中的顺序堆叠（层级越低，出现位置越靠上）。非静态定位的元素（及其子元素）将始终覆盖静态定位（static）的元素，而不管 HTML 层次结构如何。

层叠上下文是包含一组图层的元素。 在一组层叠上下文中，其子元素的 `z-index` 值是相对于该父元素而不是 document root 设置的。每个层叠上下文完全独立于它的兄弟元素。如果元素 B 位于元素 A 之上，则即使元素 A 的子元素 C 具有比元素 B 更高的 `z-index` 值，元素 C 也永远不会在元素 B 之上.

每个层叠上下文是自包含的：当元素的内容发生层叠后，整个该元素将会在父层叠上下文中按顺序进行层叠。少数 CSS 属性会触发一个新的层叠上下文，例如 opacity 小于 1，filter 不是 none，transform 不是 none。

## 请阐述块格式化上下文（Block Formatting Context）及其工作原理。

块格式上下文（BFC）是 Web 页面的可视化 CSS 渲染的部分，是块级盒布局发生的区域，也是浮动元素与其他元素交互的区域。

1. BFC 垂直方向的距离由 margin 决定(属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠，与方向无关)
2. BFC 的区域不会与 float 的元素区域重叠
3. BFC 在页面上是一个隔离的独立容器，容器里面的子元素不会影响到外面元素，反之亦然
4. 计算 BFC 高度时候，浮动子元素也参与计算
5. 每个元素的 margin box 的左边， 与包含块 border box 的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此
6. 内部的 Box 会在垂直方向上一个接一个的放置

BFC 的创造条件：

1. float 属性不为 none
2. position 属性为 absolute 或 fixed
3. display 的值为 inline-block、table-cell、table-caption、flex、inline-flex
4. overflow 的值不为 visible

## 有什么不同的方式可以隐藏内容？

- `visibility: hidden`：元素仍然在页面流中，并占用空间。
- `width: 0; height: 0`：使元素不占用屏幕上的任何空间，导致不显示它。
- `position: absolute; left: -99999px`： 将它置于屏幕之外。
- `text-indent: -9999px`：这只适用于 block 元素中的文本。

## 使用 CSS 预处理的优缺点分别是什么？

优点：

- 提高 CSS 可维护性。
- 易于编写嵌套选择器。
- 引入变量，增添主题功能。可以在不同的项目中共享主题文件。
- 通过混合（Mixins）生成重复的 CSS。
- 将代码分割成多个文件。不进行预处理的 CSS，虽然也可以分割成多个文件，但需要建立多个 HTTP 请求加载这些文件。

缺点：

- 需要预处理工具。
- 重新编译的时间可能会很慢。
- Less 中，变量名称以`@`作为前缀，容易与 CSS 关键字混淆，如`@media`、`@import` 和`@font-face`。

## 如何防止用户复制网页中的文本信息？

## CSS3 的新特性

1. 颜色：新增 RGBA、HSLA 模式
2. 文字阴影：text-shadow
3. 边框：圆角（border-radius）、边框阴影(box-shadow)
4. 背景：背景图片尺寸(background-size)、背景图片原点(background-origin)、背景图片裁切区域(background-clip)
5. 渐变：linear-gradient、radial-gradient
6. 盒子模型：box-sizing
7. 过渡：transition，可实现动画
8. 自定义动画：animate @keyfrom
9. 媒体查询：@media screen and (width:800px)
10. border-image
11. 2D 转换：transform：translate(x,y)、rotate(x,y)、skew(x,y)、scale(x,y)
12. 3D 转换
13. 字体图标：font-face
14. 弹性布局：flex

## CSS3 动画和 JS 动画主要的区别

1. 功能涵盖面，JS 比 CSS3 大
   - 定义动画过程的@keyframes 不支持递归定义，如果有多种类似的动画过程，需要调节多个参数来生成的话，将会有很大的冗余，而 JS 则天然可以以一套函数实现多个不同的动画过程
   - 事件尺度上，@keyframes 的动画粒度粗，而 JS 动画粒度可以很细
   - CSS3 动画里被支持的时间函数非常少，不够灵活
   - 以现有接口，CSS3 动画无法做到支持两个以上的状态变化
2. 实现/重构难度不一，CSS3 比 JS 更简单，性能调优方向固定
3. 对于帧速表现不好的低版本浏览器，CSS3可以做到自然降级，而JS需要撰写额外代码
4. CSS动画有天然事件支持，JS需要自己写事件
5. CSS3有兼容性问题，而JS大多时候没有兼容性问题
