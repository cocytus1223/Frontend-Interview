# Ajax

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
