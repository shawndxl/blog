canvas issues
-----------------

1、canvas.toDataURl() get error: "Uncaught DOMException: Failed to execute 'toDataURL' on 'HTMLCanvasElement': Tainted canvases may not be exported."

How it come? test down there two patterns

```js
var img = new Image();
img.src = 'img/1.png';
img.onload = function() {
	var canvasData = canvas.toDataURL();
}

```

```js
var img = new Image();
img.src = 'http://example.com/image'; // your doamin img
img.onload = function() {
	var canvasData = canvas.toDataURL();
}

```

you`ll find the second one will get the error "Tainted canvases may not be exported".


How to fix it? @see [stackoverflow](http://stackoverflow.com/questions/22710627/tainted-canvases-may-not-be-exported) 

you may been told just set:

> imgBg.crossOrigin = 'Anonymous'

and, we got another error

> Access to Image at 'http://example.com/image' from origin 'http://192.168.100.47:8101' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://192.168.100.47:8101' is therefore not allowed access.

CORS error we`ve seen a lot , so maybe you know you could set 'Access-Control-Allow-Origin' / 当然服务器存储那边也要开放相应的权限才行，如果是设置了防盗链的图片在服务端就没有相应的权限的话你本地端开启了权限也是没有用的


总结： @see [MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/CORS_enabled_image) canvas 使用了没有权限的跨域图片在使用 canvas.toDataURL()等数据导出函数的时候报错