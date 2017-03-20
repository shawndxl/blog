JS 参数传递
------------

get

post

hashchange [@see MDN](https://developer.mozilla.org/zh-CN/docs/Web/Events/hashchange)

event [@see MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Events/Creating_and_triggering_events#The_old-fashioned_way)

> 可用于hybrid app中app向前端页面传递数据，其实原理就是监听事件在触发时事件本身会被传递进来，那么把信息写进事件对象，这样监听到的事件即可以读取到信息

```js
// 前端先添加监听
window.addEventListener('build', function (e) {console.log(e._msg)}, false);

```

```js
// Native app 执行
var event = new Event('build');
event._msg = 'your msg or json';
window.dispatchEvent(event);
```

postMessage [@see MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)

> 可用于向页面中内置的iframe层传递消息

img请求

> 多用于写日志，[@see h5tracker](https://github.com/shawndxl/h5tracker)


hybrid app开发时自定义协议

> 用于 Native App 混合 Web 页面的开发中，Web 页面调用 Native App 数据的场景，大概思路如下

> 两者约定一个协议

```js
var protocol = 'webToApp';
```

> 通过新建iframe 或者 img 把需求传递给Native App的数据传递过去

```js
var action = 'your msg type';
var callbackId = Math.random().toString(36).slice(4,11); // 得到一个唯一的回调ID
var reqUrl = protocol + '://' + action + '?callback_id=' + callbackId + '&name=1&age=2';
```

> Native App 拦截 WebView 中的请求当为约定好的如我们这里设定的 webToApp 协议时做出处理，然后执行约定好的代码。

> 前端提前声明一个全局的方法

```js
window.excuMsg = function(json) {
	var data = JSON.parse(json);
	// do your thing, such as find the object the callbackId in reply json and remove the iframe or img you created with the callbackId as its id..
}
```

> Native 在 webview 中执行

```js
window.excuMsg('{"callbackId":"saf","msg":"your msg"}')
```
