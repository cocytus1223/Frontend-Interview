# 1. Vue

## 1.1. vue 的两个核心是什么？

### 1.1.1. 数据驱动，也叫双向数据绑定

Vue.js 数据观测原理在技术实现上，利用的是 ES6 的 `Object.defineProperty` 和存储器属性: `getter` 和 `setter`（所以只兼容 IE9 及以上版本），可称为基于依赖收集的观测机制。核心是 VM，即 ViewModel，保证数据和视图的一致性。

### 1.1.2. 组件系统

vue 组件的核心选项:

1. 模板（template）：模板声明了数据和最终展现给用户的 DOM 之间的映射关. 。
2. 初始数据（data）：一个组件的初始数据状态。对于可复用的组件来说，这. 常是私有的状态。
3. 接受的外部参数(props)：组件之间通过参数来进行数据的传递和共享。
4. 方法（methods）：对数据的改动操作一般都在组件的方法内进行。
5. 生命周期钩子函数（lifecycle hooks）：一个组件会触发多个生命周期钩子函数，最新 2.0 版本对于生命周期函数名称改动很大。
6. 私有资源（assets）：Vue.js 当中将用户自定义的指令、过滤器、组件等统称为资源。一个组件可以声明自己的私有资源。私有资源只有该组件和它的子组件可以调用。

## 1.2. 对 Vue 渐进式框架的理解

渐进式代表的含义是：没有多做职责之外的事。

`vue.js` 只提供了 `vue-cli` 生态中最核心的**组件系统**和**双向数据绑定**。

像 `vuex`、`vue-router` 都属于围绕 `vue.js` 开发的库。

比如说，你要使用 Angular，必须接受以下东西：

- 必须使用它的模块机制
- 必须使用它的依赖注入
- 必须使用它的特殊形式定义组件（这一点每个视图框架都有，难以避免）

所以 Angular 是带有比较强的排它性的，如果你的应用不是从头开始，而是要不断考虑是否跟其他东西集成，这些主张会带来一些困扰。

比如说，你要使用 React，你必须理解：

- 函数式编程的理念
- 需要知道什么是副作用
- 什么是纯函数
- 如何隔离副作用
- 它的侵入性看似没有 Angular 那么强，主要因为它是软性侵入

Vue 与 React、Angular 的不同是，但它是渐进的：

- 你可以在原有大系统的上面，把一两个组件改用它实现，当 jQuery 用；
- 也可以整个用它全家桶开发，当 Angular 用；
- 还可以用它的视图，搭配你自己设计的整个下层用。
- 你可以在底层数据逻辑的地方用 OO 和设计模式的那套理念，
- 也可以函数式，都可以，它只是个轻量视图而已，只做了最核心的东西。

## 1.3. Vue 几个常用的指令

- v-if：根据表达式的值的真假条件渲染元素。在切换时元素及它的数据绑定 /组件被销毁并重建。
- v-show：根据表达式之真假值，切换元素的 display CSS 属性。
- v-for：循环指令，基于一个数组或者对象渲染一个列表，vue 2.0 以上必须需配合 key 值使用。
- v-bind：动态地绑定一个或多个特性，或一个组件 prop 到表达式。
- v-on：用于监听指定元素的 DOM 事件，比如点击事件。绑定事件监听器。
- v-model：实现表单输入和应用状态之间的双向绑定
- v-pre：跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。跳过大量没有指令的节点会加快编译。
- v-once：只渲染元素和组件一次。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。

## 1.4. v-if 和 v-show 的区别

- 共同点：

  v-if 和 v-show 都是动态显示 DOM 元素。

- 区别：

1. 编译过程：v-if 是真正的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。

2. 编译条件：v-if 是惰性的：如果在初始渲染时条件为假，则什么也不做。直到条件第一次变为真时，才会开始渲染条件块。v-show 不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

3. 性能消耗：v-if 有更高的切换消耗。v-show 有更高的初始渲染消耗。

4. 应用场景：v-if 适合运行时条件很少改变时使用。v-show 适合频繁切换。

## 1.5. vue 常用的修饰符

**v-on 指令常用修饰符**：

- .stop - 调用 event.stopPropagation()，禁止事件冒泡。
- .prevent - 调用 event.preventDefault()，阻止事件默认行为。
- .capture - 添加事件侦听器时使用 capture 模式。
- .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
- .{keyCode | keyAlias} - 只当事件是从特定键触发时才触发回调。
- .native - 监听组件根元素的原生事件。
- .once - 只触发一次回调。
- .left - (2.2.0) 只当点击鼠标左键时触发。
- .right - (2.2.0) 只当点击鼠标右键时触发。
- .middle - (2.2.0) 只当点击鼠标中键时触发。
- .passive - (2.3.0) 以 { passive: true } 模式添加侦听器

注意： 如果是在自己封装的组件或者是使用一些第三方的 UI 库时，会发现并不起效果，这时就需要用`·.native 修饰符了，如：

```html
//使用示例：
<el-input
  v-model="inputName"
  placeholder="搜索你的文件"
  @keyup.enter.native="searchFile(params)"
>
</el-input>
```

**v-bind 指令常用修饰符**：

- .prop - 被用于绑定 DOM 属性 (property)。
- .camel - (2.1.0+) 将 kebab-case 特性名转换为 camelCase. (从 2.1.0 开始支持)
- .sync (2.3.0+) 语法糖，会扩展成一个更新父组件绑定值的 v-on 侦听器。

**v-model 指令常用修饰符**：

- .lazy - 取代 input 监听 change 事件
- .number - 输入字符串转为数字
- .trim - 输入首尾空格过滤

## 1.6. v-on 可以监听多个方法吗？

v-on 可以监听多个方法，例如：

```html
<input
  type="text"
  :value="name"
  @input="onInput"
  @focus="onFocus"
  @blur="onBlur"
/>
```

但是同一种事件类型的方法，只会响应第一个，例如：

```html
<a href="javascript:;" @click="methodsOne" @click="methodsTwo"></a>
```

只会响应 methodsOne 方法。

## 1.7. Vue 中的 key 值有什么用？

**key 值**：用于管理可复用的元素。因为 Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做使 Vue 变得非常快，但是这样也不总是符合实际需求。

2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。

例如，如果你允许用户在不同的登录方式之间切换：

```html
<template v-if="loginType === 'username'">
  <label>Username</label> <input placeholder="Enter your username" />
</template>

<template v-else>
  <label>Email</label> <input placeholder="Enter your email address" />
</template>
```

那么在上面的代码中切换 loginType，loginType 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，</span>不会被替换掉，仅仅是替换了它的 placeholder`。

这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达这两个元素是完全独立的，不要复用它们。只需添加一个具有唯一值的 key 属性即可：

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input" />
</template>

<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input" />
</template>
```

现在，每次切换时，输入框都将被重新渲染。

## 什么是\$nextTick？

简单回答：因为 Vue 的异步更新队列，\$nextTick 是用来知道什么时候 DOM 更新完成的。

异步更新队列：

Vue 在观察到数据变化时并不是直接更新 DOM，而是开启一个队列，并缓冲在同一个事件循环中发生的所以数据改变。在缓冲时会去除重复数据，从而避免不必要的计算和 DOM 操作。然后，在下一个事件循环 tick 中，Vue 刷新队列并执行实际（已去重的）工作。所以如果你用一个 for 循环来动态改变数据 100 次，其实它只会应用最后一次改变，如果没有这种机制，DOM 就要重绘 100 次，这固然是一个很大的开销。

Vue 会根据当前浏览器环境优先使用原生的 Promise.then 和 MutationObserver，如果都不支持，就会采用 setTimeout 代替。

事实上，在执行 this.showDiv = true 时，div 仍然还是没有被创建出来，直到下一个 vue 事件循环时，才开始创建。

## Vue 组件中 data 为什么必须是函数？

因为一个组件是可以共享的，但他们的 data 是私有的，所以每个组件都要 return 一个新的 data 对象，返回一个唯一的对象，不要和其他组件共用一个对象。
