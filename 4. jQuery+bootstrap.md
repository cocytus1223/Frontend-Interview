# 1. jQuery + Bootstrap

## 1.1. Jq 和 zepto 的区别是什么？他们一般的使用场景？

- 相同点：
  Zepto 最初是为移动端开发的库，是 jQuery 的轻量级替代品，因为它的 API 和 jQuery 相似，而文件更小。Zepto 最大的优势是它的文件大小，只有 8k 多，是目前功能完备的库中最小的一个，尽管不大，Zepto 所提供的工具足以满足开发程序的需要。大多数在 jQuery 中·常用的 API 和方法 Zepto 都有，Zepto 中还有一些 jQuery 中没有的。另外，因为 Zepto 的 API 大部分都能和 jQuery 兼容，所以用起来极其容易，如果熟悉 jQuery，就能很容易掌握 Zepto。你可用同样的方式重用 jQuery 中的很多方法，也可以方面地把方法串在一起得到更简洁的代码，甚至不用看它的文档。
- 不同点：
  针对移动端程序，Zepto 有一些基本的触摸事件可以用来做触摸屏交互（tap 事件、swipe 事件），Zepto 是不支持 IE 浏览器的，这不是 Zepto 的开发者 Thomas Fucks 在跨浏览器问题上犯了迷糊，而是经过了认真考虑后为了降低文件尺寸而做出的决定，就像 jQuery 的团队在 2.0 版中不再支持旧版的 IE（6 7 8）一样。因为 Zepto 使用 jQuery 句法，所以它在文档中建议把 jQuery 作为 IE 上的后备库。那样程序仍能在 IE 中，而其他浏览器则能享受到 Zepto 在文件大小上的优势，然而它们两个的 API 不是完全兼容的，所以使用这种方法时一定要小心，并要做充分的测试。

## 1.2. `$(this)` 和 this 的区别是什么？

- `$(this)` 返回一个 jQuery 对象，你可以对它调用多个 jQuery 方法，比如用 text() 获取文本，用 val()获取值等等。
- this 代表当前元素，它是 JavaScript 关键词中的一个，表示上下文中的当前 DOM 元素

## 1.3. Jq 对象和 DOM 对象是怎么相互转化的？

- jquery 转 DOM 对象：jQuery 对象是一个数组对象，可以通过[index]的得到相应的 DOM 对象还可以通过 get[index]去得到相应的 DOM 对象。
- DOM 对象转 jQuery 对象:\$(DOM 对象)

## 1.4. 你了解 jq 的链式编程吗？知道如何实现的吗？

## 1.5. jQuery.fn 的 init 方法返回的 this 指的是什么对象？为什么要返回 this？

this 执行 init 构造函数自身，其实就是 jQuery 实例对象，返回 this 是为了实现 jQuery 的链式
操作

## 1.6. Jq.extend 和 Jq.fn.extend 是用来做什么的？有什么区别？

jQuery 为开发插件提拱了两个方法，分别是：

1. jQuery.fn.extend();用来扩展 jQuery 实例
2. jQuery.extend();用来扩展 jQuery 对象本身

## 1.7. Jq 的 each 循环是如何跳出的？

## 1.8. Jq 的源码有看过吗？如果看过是怎么理解 jq 源码的？

## 1.9. Bootstrap 有使用过吗？为什么使用？

## 1.10. Bootstrap 的栅格系统是什么？工作原理是什么？+++

## 1.11. Bootstrap 中设置分页的 class？

## 1.12. Bootstrap 如何设置响应式表格？

## 1.13. 使用 Bootstrap 激活或禁用按钮要如何操作？

## 1.14. Bootstrap 中有关元素浮动及清除浮动的 class？

## 1.15. 对于各类尺寸的设备，Bootstrap 设置的 class 前缀分别是什么？
