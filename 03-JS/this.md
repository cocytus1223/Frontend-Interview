# 1. this

> 涉及面试题：如何正确判断 this？箭头函数的 this 是什么？

`this`是 JavaScript 语言的一个关键字。

它是函数运行时，在函数体内部自动生成的一个对象，只能在函数体内部使用。

```JavaScript
function test() {
　 this.x = 1;
}
```

函数`test`运行时，内部会自动有一个`this`对象可以使用。

## 1.1. 函数调用

函数可以直接被调用，这个时候`this`被绑定到了全局对象。

```JavaScript
var x = 1;
function test() {
// this指向的是window
  console.log(this.x);
}
// window.test()
test();                     //1
```

## 1.2. 对象方法调用

函数还可以作为某个对象的方法调用，那么`this`就是指这个上级对象。

```JavaScript
    var obj = {
      name: 'yy',
      sayHello: function () {

        // this =>  obj
        console.log(this);
      }
    }

    obj.sayHello();
```

## 1.3. 构造函数调用

所谓构造函数，就是生成一个新的对象。这时，这个`this`就是指这个对象。

```JavaScript
    function Person() {

      this.name = 'yy';
      this.sayHello = function () {
        console.log(this);
      }
    }

    var p = new Person();
    p.sayHello();
```

## 1.4. apply 调用

`apply()`是函数的一个方法，作用是改变函数的调用对象。它的第一个参数就表示改变后的调用这个函数的对象。因此，这时`this`指的就是这第一个参数。

```JavaScript
var x = 0;
function test() {
　console.log(this.x);
}

var obj = {};
obj.x = 1;
obj.m = test;
obj.m.apply() // 0
```

`apply()`的参数为空时，默认调用全局对象。因此，这时的运行结果为`0`，证明`this`指的是全局对象。

## 1.5. 箭头函数中的 this

```js
function a() {
  return () => {
    return () => {
      console.log(this)
    }
  }
}
console.log(a()()())
```

首先箭头函数其实是没有 `this` 的，箭头函数中的 `this` 只取决包裹箭头函数的第一个普通函数的 `this`。在这个例子中，因为包裹箭头函数的第一个普通函数是 a，所以此时的 `this` 是 `window`。

## 总结

以上就是 this 的规则了，但是可能会发生多个规则同时出现的情况，这时候不同的规则之间会根据优先级最高的来决定 this 最终指向哪里。
首先，`new` 的方式优先级最高，接下来是 `bind` 这些函数，然后是 `obj.foo()` 这种调用方式，最后是 `foo` 这种调用方式，同时，箭头函数的 `this` 一旦被绑定，就不会再被任何方式所改变。

![this](https://user-gold-cdn.xitu.io/2018/11/15/16717eaf3383aae8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
