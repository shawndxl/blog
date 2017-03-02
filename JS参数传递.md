
* get
* post
* [hashchange](https://developer.mozilla.org/zh-CN/docs/Web/Events/hashchange)
* [event](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Events/Creating_and_triggering_events#The_old-fashioned_way)


```js
// 前端先添加监听
window.addEventListener('build', function (e) {console.log(e._msg)}, false);

```

可用于hybrid app中app向页面传递数据

```js
// Native app 执行
var event = new Event('build');
event._msg = 'your msg..';
window.dispatchEvent(event);
```

* [postMessage](https://www.smashingmagazine.com/2014/11/styling-and-animating-svgs-with-css/#style-cascades)

* [img请求]()

多用于写日志

* hybrid app开发时自定义协议

