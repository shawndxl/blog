JS 基础
-----------

### call / apply

call 和 apply 应该都比较熟悉，但如果平常用的不多可能会记忆不深刻或反复的忘掉，所以深入理解其存在的原因以及使用场景，并在平常加以使用，可能会更有效果，今天我们来彻底的理解一下这两个函数的意义。

1 如何使用 / How

[查看MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 准确来讲是 Function.prototype.call()、Function.prototype.apply() 所以从定义上看就可以看到是函数的方法，我们做以下测试

```js
function A() {}
var B = {};

A.call(B); // ok
A.call(1); // ok
A.call(''); // ok
A.call([]); // ok
A.call(function() {}); // ok

(function() {}).call(1); // ok
(() => {}).call(''); // ok 

({}).call(''); // error -> Uncaught TypeError: (intermediate value).call is not a function
[].call(); // error -> Uncaught TypeError: [].call is not a function

({ fn: () => {} }).fn.call() // ok
```

> 所以从语法也可以看到，这个是函数的方法

> 再结合MDN定义：该方法在使用一个指定的this值和若干个指定的参数值的前提下调用某个函数或方法.

> call() 和 apply() 方法类似，只有一个区别，就是call()方法接受的是若干个参数的列表，而apply()方法接受的是一个包含多个参数的数组。

```js
((a) => a + 1).call(1, 1);
// 2
((a) => a + 1).apply(1, [1]); // 可以看出call 和 apply 只有传递参数的区别，如果不传递参数，他们是一模一样的
// 2
((a) => a + 1).apply('', [1]); // 可以看出其传递的参数是给前方的主体使用的
// 2
```

结论，用法总结A为函数 B为任意类型 A.call(B, '') 或者 A.apply(B, [''])

2 有何作用 / What

> ((a) => a + 1).call(1, 1) 这个例子其实从实际讲其实是没有意义的，因为直接运行函数即可

```js
((a) => a + 1)(1)
// 2
```

> 我们举个有意义的例子

```js
var Parent = function() {
	this.name = 'myName';
	this.age = 20;
};

var o = {};
Parent.call(o);
console.log(o); // 可以看到o "继承" 了Parent的属性
// Object {name: "myName", age: 20}
```

> 很多关于call、apply函数的介绍都喜欢称之为 继承 或者 构造，但我们要看清楚实际情况，其实跟继承一毛钱关系都没有，Parent归根到底只是一个函数，只是这个函数的内容是对this本身赋值，而call、apply函数的意义就是把另外一个对象B传递过来作为this本身，所以最终的结果就是执行了A函数之后，B的属性被赋值了，所以再次查看B，就好像是继承一样，其实并没有继承的概念，他跟运行 a +１一样，只是函数执行。


> 结论，A.call(B, c) 其实际意义就是 函数A调用B作为其this的主体，或者以对象B为this调用A函数，c 为函数A的参数

3 为什么用 / Why

> 其实任何函数从语法都可以随意call任何类型的数据，但我们之所以用，是因为call和apply会传递进来B作为this，那么其实我们只在下面两种情况下才会用到这两个call、apply

> 1) 调用B中this的值来执行A的函数内容，以产生需要的效果

```js
var Cat = {
	name: 'cat',
	food: 'fish',
	say: function() {
		console.log(this.name + ' like ' + this.food)
	}	
};
Cat.say(); // 我们定义一个对象猫，他有一个 说 的方法，当调用时执行，自己 + 喜欢 + 吃的食物
// cat like fish

// 假如我们再定义一个对象狗，但我们不想去定义够的 说 的方法了，因为他跟猫的 say() 除了主体都一样，那么我们就可以使用call或者apply了
var Dog = {
	name: 'dog',
	food: 'bone'
};
Cat.say.call(Dog);
// dog like bone 
console.log(Dog.say); // Dog 并没有继承say属性，Dog主体只是一次性执行了一个名叫say的方法而已
// undefined
```

> 2) 执行A的函数内容对B this本身的属性或值，进行改变

```js
var b = [0, 2, 4];
var c = [1, 3, 5];
// 假如我们的需求是把数组 c 的值全部都push到b中去，我们想使用push方法，而不想使用concat方法

b.push(c); // 如果我们直接这么用，结果是把数组c直接编程一个元素放进去了
// [0, 2, 4, Array[3]]

b.push(1,3,5); // 这样肯定是可以的
b.push.call(b, 1, 3, 5); // 相当于 b.push(1,3,5)
[].push.call(b, 1, 3, 5); // 也可以
Array.prototype.push.call(b, 1, 3, 5); // 也可以

// 但是上述方法都是要靠手动拆分数组c的，不手动拆分不行吗？把数组传递进去作为一个参数行不行，可以，正好就是apply干的事儿
Array.prototype.push.apply(b, c); // b是this主体，执行了之后数组b发生了变化，而数组c是没有变化的，数组c在这里只是参数
// [0, 2, 4, 1, 3, 5]
```

### 异步

async

Promise

Generator 函数


[@see ruanyf](http://www.ruanyifeng.com/blog/2015/04/generator.html)


### 闭包

函数的声明周期

变量的作用域

闭包的概念closure (also lexical closures or function closures)

> 去理解一个词，不要去背中文名字，而去理解其原本的英文名字的含义，去理解为什么叫这个名字。closure 只是一个缩写被翻译成闭包，lexical 和 function, 我们可以粗鄙的翻译成 词语上的关闭，或者函数上的关闭，其实就能大概体味出为什么叫这么个名字了

```js

```

[@see wikipedia](https://en.wikipedia.org/wiki/Closure_(computer_programming))
[@see MDN]()
[@see ruanyf]()

