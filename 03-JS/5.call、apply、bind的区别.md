# call、apply、bind 的区别

函数的上下文调用模式。任何函数都可以调用这三个方法

## `call()`

**作用**：
可以用 `call()` 来调用函数，并修改函数中 `this` 的指向

**语法**：
`func.call(obj,value1,value2);`

**参数**：
第一个参数: 改变 `this` 的指向
后面的参数: 被调用函数要传入的实参，以逗号分隔
如果 `call()` 没有参数，或者第一个参数为 `null`，函数内的 `this` 指向 `window`

**示例**：

```JavaScript
function sum(n1, n2){
    return n1 + n2;
}

function callSum(n1, n2){
    return sum.call(this, n1, n2);
}

alert(callSum(10,10))//20
```

## `apply()`

**作用**：
可以用 `apply()` 来调用函数，并修改函数中 `this` 的指向

**语法**：
`func.apply(obj,[value1,value2])；`

**参数**：
第一个参数: 改变 `this` 的指向
第二个参数: 传入一个数组或伪数组，里面存放被调用函数需要的实参

**示例**：

```JavaScript
function fn(n1, n2){
    console.log(n1 + n2);//300
    console.log(this);//Object
}

fn.apply({name: "ww"},  [100, 200] );
```

## `bind()`

**作用**：
不会调用函数，克隆一个新的函数，其 `this` 值会被绑定到传给`bind()`函数的值

**语法**：
`func.bind(obj,value1,value2)();`
`func.bind(obj)(value1,value2);`

**参数**：
第一个参数: 改变 `this` 的指向
后面的参数: 被调用函数要传入的实参，以逗号分隔

### `call()`和 `apply()`的区别

相同点：

1. 他们都是 `function.prototype` 对象中定义的方法
2. 第一个参数是都是函数内部的 `this` 的值

不同点：

1. `call()`方法将函数的参数从第二个参数开始依次排开；
2. `apply()`方法的第二个参数是一个数组对象，数组的第一个参数表示函数的第一个实参，以此类推。

## 模拟实现 call 和 apply

可以从以下几点来考虑如何实现

不传入第一个参数，那么默认为 window
改变了 this 指向，让新的对象可以执行该函数。那么思路是否可以变成给新的对象添加一个函数，然后在执行完以后删除？

```js
Function.prototype.myCall = function(context) {
  var context = context || window
  // 给 context 添加一个属性
  // getValue.call(a, 'yck', '24') => a.fn = getValue
  context.fn = this
  // 将 context 后面的参数取出来
  var args = [...arguments].slice(1)
  // getValue.call(a, 'yck', '24') => a.fn('yck', '24')
  var result = context.fn(...args)
  // 删除 fn
  delete context.fn
  return result
}
```

以上就是 call 的思路，apply 的实现也类似

```js
Function.prototype.myApply = function(context) {
  var context = context || window
  context.fn = this

  var result
  // 需要判断是否存储第二个参数
  // 如果存在，就将第二个参数展开
  if (arguments[1]) {
    result = context.fn(...arguments[1])
  } else {
    result = context.fn()
  }

  delete context.fn
  return result
}
```

bind 和其他两个方法作用也是一致的，只是该方法会返回一个函数。并且我们可以通过 bind 实现柯里化。

同样的，也来模拟实现下 bind

```js
Function.prototype.myBind = function(context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  var _this = this
  var args = [...arguments].slice(1)
  // 返回一个函数
  return function F() {
    // 因为返回了一个函数，我们可以 new F()，所以需要判断
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
}
```
