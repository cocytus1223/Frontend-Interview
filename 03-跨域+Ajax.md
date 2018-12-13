# 1. 跨域+ Ajax

## 1.1. 什么是跨域？

跨域是指一个域下的文档或脚本试图去请求另一个域下的资源，这里跨域是广义的。
其实我们通常所说的跨域是狭义的，是由浏览器同源策略限制的一类请求场景。

## 1.2. 什么是同源策略？

同源策略/SOP（Same origin policy）是一种约定，由 Netscape 公司 1995 年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到 XSS、CSFR 等攻击。所谓同源是指**协议+域名+端口**三者相同，即便两个不同的域名指向同一个 ip 地址，也非同源。

同源策略限制以下几种行为：

- Cookie、LocalStorage 和 IndexDB 无法读取
- DOM 和 Js 对象无法获得
- AJAX 请求不能发送

## 1.3. 跨域的解决方案

1. 通过 jsonp 跨域
2. document.domain + iframe 跨域
3. location.hash + iframe
4. window.name + iframe 跨域
5. postMessage 跨域
6. 跨域资源共享（CORS）
7. nginx 代理跨域
8. nodejs 中间件代理跨域
9. WebSocket 协议跨域

### 1.3.1. 通过 jsonp 跨域

通常为了减轻 web 服务器的负载，我们把 js、css，img 等静态资源分离到另一台独立域名的服务器上，在 html 页面中再通过相应的标签从不同域名下加载静态资源，而被浏览器允许，基于此原理，我们可以通过动态创建 script，再请求一个带参网址实现跨域通信。

1. 原生实现：

   ```JavaScript
   var script = document.createElement('script');
   script.type = 'text/javascript';

   // 传参并指定回调执行函数为onBack
   script.src = 'http://www.domain2.com:8080/login?user=admin&callback=onBack';
   document.head.appendChild(script);

   // 回调执行函数
   function onBack(res) {
   alert(JSON.stringify(res));
   }
   ```

   服务端返回如下（返回时即执行全局函数）：

   ```JavaScript
   onBack({"status": true, "user": "admin"})
   ```

2. jquery ajax：

   ```JavaScript
     $.ajax({
     url: 'http://www.domain2.com:8080/login',
     type: 'get',
     dataType: 'jsonp',  // 请求方式为jsonp
     jsonpCallback: "onBack",    // 自定义回调函数名
     data: {}
     });
   ```

3. vue.js：

   ```JavaScript
   this.$http.jsonp('http://www.domain2.com:8080/login', {
   params: {},
   jsonp: 'onBack'
   }).then((res) => {
   console.log(res);
   })
   ```

- Jsonp 优点:
  完美解决在测试或者开发中获取不同域下的数据，用户传递一个 callback 参数给服务端，然后服务端返回数据时会将这个 callback 参数作为函数名来包裹住 JSON 数据，这样客户端就可以随意定制自己的函数来自动处理返回数据了
- Jsonp 缺点:
  Jsonp 只支持 get 请求而不支持 post 请求，也即是说如果想传给后台一个 json 格式的数据，浏览器会报一个 http 状态码 415 错误，告诉你请求格式不正确

### 1.3.2. postMessage 跨域

postMessage 是 HTML5 XMLHttpRequest Level 2 中的 API，且是为数不多可以跨域操作的 window 属性之一，它可用于解决以下方面的问题：

- 页面和其打开的新窗口的数据传递
- 多窗口之间消息传递
- 页面与嵌套的 iframe 消息传递
- 上面三个场景的跨域数据传递

`postMessage(data,origin)`方法接受两个参数

- data：html5 规范支持任意基本类型或可复制的对象，但部分浏览器只支持字符串，所以传参时最好用 JSON.stringify()序列化。
- origin：协议+主机+端口号，也可以设置为"\*"，表示可以传递给任意窗口，如果要指定和当前窗口同源的话设置为"/"。

**使用场景**：窗口 A (http:A.com)向跨域的窗口 B (http:B.com)发送信息。步骤如下。

（1）在 A 窗口中操作如下：向 B 窗口发送数据：

```JavaScript
    // 窗口A(http:A.com)向跨域的窗口B(http:B.com)发送信息
    Bwindow.postMessage('data', 'http://B.com'); //这里强调的是B窗口里的window对象
```

（2）在 B 窗口中操作如下：

```JavaScript
    // 在窗口B中监听 message 事件
    Awindow.addEventListener('message', function (event) {   //这里强调的是A窗口里的window对象
        console.log(event.origin);  //获取 ：url。这里指：http://A.com
        console.log(event.source);  //获取：A window对象
        console.log(event.data);    //获取传过来的数据
    }, false);
```

### 1.3.3. 跨域资源共享（CORS）

普通跨域请求：只服务端设置 Access-Control-Allow-Origin 即可，前端无须设置，若要带 cookie 请求：前后端都需要设置。

需注意的是：由于同源策略的限制，所读取的 cookie 为跨域请求接口所在域的 cookie，而非当前页。

目前，所有浏览器都支持该功能(IE8+：IE8/9 需要使用 XDomainRequest 对象来支持 CORS）)，CORS 也已经成为主流的跨域解决方案。

1. 原生 ajax

   ```JavaScript
   // 前端设置是否带cookie
   xhr.withCredentials = true;
   ```

   示例代码：

   ```JavaScript
   var xhr = new XMLHttpRequest(); // IE8/9需用window.XDomainRequest兼容

   // 前端设置是否带cookie
   xhr.withCredentials = true;

   xhr.open('post', 'http://www.domain2.com:8080/login', true);
   xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
   xhr.send('user=admin');

   xhr.onreadystatechange = function() {
       if (xhr.readyState == 4 && xhr.status == 200) {
           alert(xhr.responseText);
       }
   };
   ```

2. jQuery ajax

   ```JavaScript
   $.ajax({
       ...
     xhrFields: {
         withCredentials: true    // 前端设置是否带cookie
     },
     crossDomain: true,   // 会让请求头中包含跨域的额外信息，但不会含cookie
       ...
   });
   ```

3. vue 框架

   - axios 设置：

   ```JavaScript
   axios.defaults.withCredentials = true
   ```

   - vue-resource 设置：

   ```JavaScript
   Vue.http.options.credentials = true
   ```

### 1.3.4. WebSocket 协议跨域

WebSocket protocol 是 HTML5 一种新的协议。它实现了浏览器与服务器全双工通信，同时允许跨域通讯，是 server push 技术的一种很好的实现。
原生 WebSocket API 使用起来不太方便，我们使用 Socket.io，它很好地封装了 webSocket 接口，提供了更简单、灵活的接口，也对不支持 webSocket 的浏览器提供了向下兼容。

```html
<div>user input：<input type="text" /></div>
<script src="./socket.io.js"></script>
<script>
  var socket = io('http://www.domain2.com:8080')

  // 连接成功处理
  socket.on('connect', function() {
    // 监听服务端消息
    socket.on('message', function(msg) {
      console.log('data from server: ---> ' + msg)
    })

    // 监听服务端关闭
    socket.on('disconnect', function() {
      console.log('Server socket has closed.')
    })
  })

  document.getElementsByTagName('input')[0].onblur = function() {
    socket.send(this.value)
  }
</script>
```

## 1.4. Ajax 的优缺点是什么？

- 优点：
  1. 最大的一点是页面无刷新，在页面内与服务器通信，给用户的体验非常好。
  2. 使用异步方式与服务器通信，不需要打断用户的操作，具有更加迅速的响应能力。
  3. 可以把以前一些服务器负担的工作转嫁到客户端，利用客户端闲置的能力来处理，减轻服务器和带宽的负担，节约空间和宽带租用成本。并且减轻服务器的负担，ajax 的原则是“按需取数据”，可以最大程度的减少冗余请求，和响应对服务器造成的负担。
  4. 基于标准化的并被广泛支持的技术，不需要下载插件或者小程序。
- 缺点：
  1. ajax 不支持浏览器 back 按钮。
  2. 安全问题 AJAX 暴露了与服务器交互的细节。
  3. 对搜索引擎的支持比较弱。
  4. 破坏了程序的异常机制。
  5. 不容易调试

## 1.5. get 和 post 有什么区别？

![区别](https://user-gold-cdn.xitu.io/2018/11/16/1671bc56731bf8a8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 1.6. 你是怎么理解 ajax 的？Ajax 的原理能说一下吗？

Ajax 的原理简单来说通过 XmlHttpRequest 对象来向服务器发异步请求，从服务器获得数据，然后用 javascript 来操作 DOM 而更新页面。这其中最关键的一步就是从服务器获得请求数据。

1. 创建 XMLHttpRequest 对象
2. 建立连接，设置为 GET 发送：xmlHttp.open("GET", "/ajax/getHint.jsp?q=" + txtValue, true);
3. 发送数据：xmlHttp.send();
4. 注册回调方法：xmlHttp.onreadystatechange = ajaxCallback;
5. 执行回调：

```JavaScript
function ajaxCallback(){
//响应就绪时
if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
searchInput.value = xmlHttp.responseText;
 }
}
```

## 1.7. 你对同步和异步是怎么理解的？

- 同步：同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务。
- 异步：不进入主线程、而进入"任务队列"（task queue）的任务，只有等主线程任务执行完毕，"任务队列"开始通知主线程，请求执行任务，该任务才会进入主线程执行

## 1.8. 页面传参有没有出现过乱码的情况？如果有你是怎么解决的？

## 1.9. 在前后端数据交互的时候，有处理过（参数）数据加密的情况吗？

## 1.10. Json 字符串和 json 对象怎么相互转换？（重要）

- `JSON.parse()` 从一个字符串中解析出 json 对象
- `JSON.stringify()` 从一个对象中解析出字符串

## 1.11. Jq 封装的 Ajax 有多少个 callback？一般页面的 loading 层（加载的动画）都是写到那个回调函数里面？


## 1.13. http 的常用的状态码遇到过那些？分别代表什么意思？

- 1xx：指示信息——表示请求已接受，继续处理
- 2xx：成功——表示请求已被成功接收
- 3xx：重定向——要完成请求必须进行更进一步的操作
- 4xx：客户端错误——请求有语法错误或请求无法实现
- 5xx：服务器错误——服务器未能实现合法的请求

200 OK：客户端请求成功
206 Partial Content：客户发送了一个带有 Range 头的 GET 请求，服务器完成了它
301 Moved Permanently：永久重定向，所请求的页面已经转移到新的 url
302 Found：临时重定向，所请求的页面已经临时转移至新的 url
304 Not MOdified：缓存，客户端有缓冲的文档并发出了一个条件性的请求，服务器告诉客户，原来缓冲的文档还可以继续使用
400 Bad Request：客户端有语法错误，不能被服务器所理解
401 Unauthorized：请求未经授权，这个状态码必须和 WWW-Authenticate 报头域一起使用
403 Forbidden：对被请求页面的访问被禁止
404 Not Found：请求资源不存在
500 Internal Server Error：服务器发生不可预期的错误原来缓冲的文档还可以继续使用
503 Server Unavailable：请求未完成，服务器临时过载或宕机，一段时间后可能恢复正常

## 1.14. 使用 Async 会注意哪些东西

## 1.15. Async 里面有多个 await 请求，可以怎么优化（请求是否有依赖）

## 1.16. Promise 和 Async 处理失败的时候有什么区别
