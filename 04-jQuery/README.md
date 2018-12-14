# jQuery

## `$(this)` 和 this 的区别是什么？

- `$(this)` 返回一个 jQuery 对象，你可以对它调用多个 jQuery 方法，比如用 text() 获取文本，用 val()获取值等等。
- this 代表当前元素，它是 JavaScript 关键词中的一个，表示上下文中的当前 DOM 元素

## Jq 对象和 DOM 对象是怎么相互转化的？

- jquery 转 DOM 对象：jQuery 对象是一个数组对象，可以通过[index]的得到相应的 DOM 对象还可以通过 get[index]去得到相应的 DOM 对象。
- DOM 对象转 jQuery 对象:\$(DOM 对象)

## 你了解 jq 的链式编程吗？知道如何实现的吗？

## jQuery.fn 的 init 方法返回的 this 指的是什么对象？为什么要返回 this？

this 执行 init 构造函数自身，其实就是 jQuery 实例对象，返回 this 是为了实现 jQuery 的链式
操作

## Jq.extend 和 Jq.fn.extend 是用来做什么的？有什么区别？

jQuery 为开发插件提拱了两个方法，分别是：

1. jQuery.fn.extend();用来扩展 jQuery 实例
2. jQuery.extend();用来扩展 jQuery 对象本身

## Jq 的 each 循环是如何跳出的？

## Jq 的源码有看过吗？如果看过是怎么理解 jq 源码的？
