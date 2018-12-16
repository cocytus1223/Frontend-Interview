# 1. 性能优化类

## 1.1. 资源合并与压缩

## 1.2. 图片相关优化

- 不同格式图片的特点

1. jpg 有损压缩，压缩率高，不支持透明
2. png 支持透明，浏览器兼容性好
3. webp 压缩程度更好，在 ios webview 中有兼容性问题
4. svg 矢量图，代码内嵌，相对较小，图片样式相对简单的场景(尽量使用，绘制能力有限，图片简单用的比较多)

- 不同格式图片的使用场景

1. jpg：大部分不需要透明图片的业务场景
2. png：大部分需要透明图片的业务场景
3. webp：android 全部(解码速度和压缩率高于 jpg 和 png，但是 ios safari 还没支持)
4. svg：图片样式相对简单的业务场景

- 图片压缩的几种情况

1. 针对真实图片情况，舍弃一些相对无关紧要的色彩信息
2. CSS 雪碧图：把你的网站用到的一些图片整合到一张单独的图片中
   - 优点：减少 HTTP 请求的数量(通过 backgroundPosition 定位所需图片)
   - 缺点：整合图片比较大时，加载比较慢(如果这张图片没有加载成功，整个页面会失去图片信息)facebook 官网任然在用，主要 pc 用的比较多，相对性能比较强
3. Image-inline：将图片的内容嵌到 html 中 //base64 信息，减少网站的 HTTP 请求,如果图片比较小比较多，时间损耗主要在请求的骨干网络
4. 使用矢量图：使用 SVG 进行矢量图的绘制,使用 icon-font 解决 icon 问题
5. 在 android 下使用 webp：webp 的优势主要体现在它具有更优的图像数据压缩算法，能带来更小的图片体积，而且拥有肉眼识别无差异的图像质量；同时具备了无损和有损的压缩模式、Alpha 透明以及动画的特性，在 JPEG 和 PNG 上的转化效果都非常优秀、稳定和统一

## 1.3. css 和 js 的装载与执行

## 1.4. 懒加载与预加载

### 1.4.1. 懒加载

1. **原理**:
   - 懒加载就是将不关键的资源延后加载。
   - 只加载自定义区域（通常是可视区域，但也可以是即将进入可视区域）内需要加载的东西。对于图片来说，先设置图片标签的 src 属性为一张占位图，将真实的图片资源放入一个自定义属性中，当进入自定义区域时，就将自定义属性替换为 src 属性，这样图片就会去下载资源，实现了图片懒加载。
   - 懒加载不仅可以用于图片，也可以使用在别的资源上。比如进入可视区域才开始播放视频等等。
2. **注意**：
   - 关注首屏处理,因为还没滑动
   - 占位，图片大小首先需要预设高度，如果没有设置的话，会全部显示出来

### 1.4.2. 预加载

**原理**：

- 图片等静态资源在使用之前的提前请求
- 资源使用到时能从缓存中加载，提升用户体验
- 页面展示的依赖关系维护

1. 第一种方式：html 标签直接请求下来

   ```html
   <img src="www.pic27.com/sadfafd/dafdsa.jpg" style="display: none" />
   <img src="www.pic27.com/sadfafd/dsaf.jpg" style="display: none" />
   <img src="www.pic27.com/sadfafd/dasfd.jpg" style="display: none" />
   <img src="www.pic27.com/sadfafd/fdsa.jpg" style="display: none" />
   ```

2. 第二种方式：使用 image 对象

   ```js
   var image = new Image();
   image.src = "www.pic26.com/dafdafd/safdas.jpg"；
   ```

3. 第三种方式：使用 xmlhttprequest 对象

   存在跨域问题，但是容易控制

   ```js
   var xmlhttprequest = new XMLHttpRequest()
   xmlhttprequest.onreadystatechange = callback
   xmlhttprequest.onprogress = progressCallback
   xmlhttprequest.open('GET', 'http://image.baidu.com/mouse,jpg', true)
   xmlhttprequest.send()
   function callback() {
     if (xmlhttprequest.readyState == 4 && xmlhttprequest.status == 200) {
       var responseText = xmlhttprequest.responseText
     } else {
       console.log('Request was unsuccessful:' + xmlhttprequest.status)
     }
   }
   function progressCallback(e) {
     e = e || event
     if (e.lengthComputable) {
       console.log('Received' + e.loaded + 'of' + e.total + 'bytes')
     }
   }
   ```

4. 使用 PreloadJS 库
   PreloadJS 提供了一种预加载内容的一致方式，以便在 HTML 应用程序中使用。预加载可以使用 HTML 标签以及 XHR 来完成。默认情况下，PreloadJS 会尝试使用 XHR 加载内容，因为它提供了对进度和完成事件的更好支持，但是由于跨域问题，使用基于标记的加载可能更好。

   ```js
    //使用preload.js
    var queue=new createjs.LoadQueue();//默认是xhr对象，如果是new createjs.LoadQueue(false)是指使用HTML标签，可以跨域
    queue.on("complete",handleComplete,this);
    queue.loadManifest([
    {id:"myImage",src:"http://pic26.nipic.com/20121213/6168183  0044449030002.jpg"},
    {id："myImage2"，src:"http://pic9.nipic.com/20100814/2839526  1931471581702.jpg"}
    ]);
    function handleComplete(){
      var image=queue.getResuLt("myImage");
      document.body.appendChild(image);
    }
   ```

## 1.5. 避免重绘与回流

- 触发页面重布局的一些 css 属性

> width、height、padding、margin、display、border-width、border、min-height

- 定位属性及浮动也会触发重布局

> top、bottom、left、right、position、float、clear

- 改变节点内部文字结构也会触发重布局

> text-align、overflow-y、font-weight、overflow、font-family、line-height、vertical-align、white-space、font-size

### 1.5.1. 优化点：使用不触发回流的方案替代触发回流的方案

只触发重绘不触发回流

```html
color、 border-style、border-radius、 visibility、 text-decoration、
background、background-image、background-position、background-repeat、background-size、
outline、outline-color、outline-style、outline-width box-shadow
```

### 1.5.2. 实战优化点总结

1. 用 translate 替代 top 属性
2. 用 opacity 代替 visibility
   - opacity 不会触发重绘也不会触发回流，只是改变图层 alpha 值，但是必须要将这个图片独立出一个图层
   - visibility 会触发重绘
3. 不要一条一条的修改 DOM 的样式，预先定义好 class，然后修改 DOM 的 className
4. 把 DOM 离线后修改，比如：先把 DOM 给`display:none`（有一次 reflow），然后你修改 100 次，然后再把它显示出来
5. 不要把 DOM 节点的属性值放在一个循环里当成循环的变量
6. 不要使用 table 布局，可能很小的一个小改动会造成整个 table 的重新布局
   - div 只会影响后续样式的布局
7. 动画实现的速度的选择
   - 根据 performance 量化性能优化
8. 对于动画新建图层
9. 启用 gpu 硬件加速(并行运算)，gpu 加速意味着数据需要从 cpu 走总线到 gpu 传输，需要考虑传输损耗
   - transform:translateZ(0)
   - transform:translate#d(0)

## 1.6. 服务端性能优化

## 1.7. Webpack 优化
