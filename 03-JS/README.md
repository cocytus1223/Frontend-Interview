# JavaScript

## js 各种位置的区别

### DOM 对象

#### 只读属性

所谓的只读属性指的是 DOM 节点的固有属性，该属性只能通过 js 去获取而不能通过 js 去设置，而且获取的值是只有数字并不带单位的（px,em 等），如下：

1. clientWidth 和 clientHeight
   该属性指的是元素的可视部分宽度和高度，即 padding+content，如果没有滚动条，即为元素设定的高度和宽度，如果出现滚动条，滚动条会遮盖元素的宽高，那么该属性就是其本来宽高减去滚动条的宽高
2. offsetWidth 和 offsetHeight
   表示可视区域的宽度和高度，即 border+padding+content 的宽度和高度并包含滚动条，该属性和其内部的内容是否超出元素大小无关，只和本来设定的 border 以及 width 和 height 有关
3. clientTop 和 clientLeft：表示边框 border 的厚度，在未指定的情况下一般为 0
4. offsetLeft 和 offsetTop
   当前元素，相对于其 offsetParent 左边距离和上边距离，即当前元素的 border 到包含它的 offsetParent 的 border 的距离
5. scrollHeight 和 scrollWidth
   表示了所有区域的高度，包含了因为滚动被隐藏的部分。

#### 可读可写属性

1. scrollTop 和 scrollLeft
   当元素其中的内容超出其宽高的时候，元素被卷起的高度和宽度。
2. obj.style.\*属性
   对于一个 dom 元素，它的 style 属性返回的是一个对象，这个对象中的任意一个属性是可读写的。如 obj.style.top,obj.style.wdith 等，在读的时候，他们返回的值常常是带有单位的(如 px),同时，对于这种方式，
   它只能够获取到该元素的行内样式，而并不能获取到该元素最终计算好的样式，这就是在读取属性值得时候和以上只读属性的区别，要获取计算好的样式，请使用 obj.currentstyle（IE）和 getComputedStyle(IE 之外的浏览器)。另一方面，这些属性能够被赋值，js 运动的原理就是通过不断修改这些属性的值而达到其位置改变的，需要注意的是，给这些属性赋值的时候需要带单位的要带上单位，否则不生效。

### Event 对象

在 js 中，对于元素的运动的操作通常都会涉及到 event 对象，而 event 对象也存在很多位置属性，且由于浏览器兼容性问题会导致这些属性间相互混淆

1. clientX 和 clientY
   这对属性是当事件发生时，鼠标点击位置相对于浏览器（可视区）的坐标，即浏览器左上角坐标的（0,0），该属性以浏览器左上角坐标为原点，计算鼠标点击位置距离其左上角的位置，不管浏览器窗口大小如何变化，都不会影响点击位置的坐标。
2. screenX 和 screenY
   事件发生时鼠标相对于屏幕的坐标，以设备屏幕的左上角为原点，事件发生时鼠标点击的地方即为该点的 screenX 和 screenY 值
3. offsetX 和 offsetY
   当事件发生时，鼠标点击位置相对于该事件源的位置，即点击该 div，以该 div 左上角为原点来计算鼠标点击位置的坐标
4. pageX 和 pageY
   事件发生时鼠标点击位置相对于页面的位置，通常浏览器窗口没有出现滚动条时，该属性和 event.clientX 及 event.clientY 是等价的，但是当浏览器出现滚动条的时候，pageX 通常会大于 clientX，因为页面还存在被卷起来的部分的宽度和高度
