# Async / 异步

回调

```js
step1(function (value1) {
  step2(value1, function(value2) {
    step3(value2, function(value3) {
      step4(value3, function(value4) {
        // ...
      });
    });
  });
});
```

EventListener

```js
// listen
window.addEventListener('build', function (e) {}, false);

// call
var event = new Event('build');
window.dispatchEvent(event);
```

JQ trigger / on

```js
$.center = $({});

$.center.trigger('message', {
    "type": "text",
	"msg": "helloworld"
});

$.center.on('message', function(e, reply) {})
```

fetch

```js
fetch('https://api.github.com/users/github')
	.then(res => res.json())
	.then(json => console.log(json));
```

Promise

```js
Promise.resolve(step1)
  .then(step2)
  .then(step3)
  .then(step4)
  .then(function (value4) {
    // Do something with value4
  }, function (error) {
    // Handle any error from step1 through step4
  })
  .done();
```

Generator 函数

> 使函数可以暂停

```js
var a = (function* () {
	yield 'a1';
	yield 'a2';
	return 'end';
})();

a.next(); // a1
a.next(); // a2
a.next(); // end
a.next(); // undefined
```

> 测试

```js
var a = (function* (x, y) {
	var a1 = yield (x + 1);
	var a2 = (yield y) + 1;
	return a1 + 1;
})(1, 1);

a.next(); // 2 
a.next(); // 1
a.next(); // undefined  // 无意义

```

```js
var a = (function* (x, y) {
	var a1 = yield (x + 1);
	var a2 = (yield y) + 1;
	return a1 + 1; // 错误
})(1, 1);

a.next(); // 2 
a.next(); // 1
a.next(3); // undefined 

```
> 正确的用法

```js
var a = (function* (x, y) {
	var a1 = yield (x + 1);
	var a2 = (yield y) + 1;
	return a2 + 1;
})(1, 1);

a.next(); // 2 
a.next(); // 1
a.next(3); // 5
```

> 理解暂停的意义，当再次开始时，从上个yield点开始执行，到下个点或者return，a.next传递的参数都是针对当前指针位置返回的函数的，而像上方使用a1去接参数的行为明显是没有意义的.

async/await 

> ES2017 标准引入了 async 函数

> async函数将 Generator函数的星号（*）替换成async，将yield替换成await

```js

```

> 自动执行，输出最后结果,而不需要next

> 返回值是 Promise 对象

```js
async function f() {
  return 'hello world';
}

f().then(v => console.log(v))
// "hello world"
```


* [@see segmentfault](https://segmentfault.com/a/1190000003810652)
* [@see ruanyf](http://www.ruanyifeng.com/blog/2015/04/generator.html)