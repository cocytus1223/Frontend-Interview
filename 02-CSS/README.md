# CSS 类

## 什么是盒子模型？

- 所有 HTML 元素都可以看作是一个盒子，包括 margin（外边距）、padding（内边距）、border（边框）、content（内容）
- IE 盒子模型的 content 部分包含了 border 和 padding，标准盒子模型不包括。
- W3C：box-sizing:content-box;（浏览器默认）
  IE：box-sizing:border-box;

## 定位有几种方式？每种方式定位的基准是什么吗？那种方式会脱标？他们之间会有层级关系吗，谁的层级会高些？

| 定位方式                     | 定位基准                       | 是否脱标 |
| ---------------------------- | ------------------------------ | -------- |
| 静态定位`position:static;`   | 所有的标准流都是静态定位       | 否       |
| 相对定位`position:relative;` | 自己的标准流                   | 否       |
| 绝对定位`position:absolute;` | 绝对元素往上找的第一个定位元素 | 是       |
| 固定定位`position: fixed;`   | 浏览器窗口                     | 是       |

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

## css reset 和 normalize.css 有什么区别

- 两者都是通过重置样式，保持浏览器样式的一致性；
- 前者几乎为所有标签添加了样式，后者保持了许多浏览器样式，保持尽可能的一致；
- 后者修复了常见的桌面端和移动端浏览器的 bug：包含了 HTML5 元素的显示设置、预格式化文字的 font-size 问题、在 IE9 中 SVG 的溢出、许多出现在各浏览器和操作系统中的与表单相关的 bug。
- 前者中含有大段的继承链；
- 后者模块化，文档较前者来说丰富；