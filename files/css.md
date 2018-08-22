CSS issues
---------

### 移动端

IOS vw显示问题

> 别人已经有深切解析了，直接[查看](http://www.zhangxinxu.com/wordpress/2016/08/vw-viewport-responsive-layout-typography/)

Iphone5s 的transition问题


[overflow]

1）重构页面导致的 “ 卡顿 ”

```css
.element {
  background: url(../img/demo.png);
  overflow-y: auto; /* 加入在手机端，这个页面比较长，当上下滑动重构 body 的 img 背景时，会造成加载不到上方或下方 body 的 img 背景，用起来感觉卡卡的，去掉该属性后正常 */
}
```

2）把元素 “ 挤 ” 走了 （iphone 测试）

> 写一个四方的 div ，底部是一个绝对定位的 1/5 高的子元素 p ，假如我们现在的需求是给 div 设置圆角

```less
// LESS
div {
  position: relative;
  width: 5rem;
  height: 5rem;
  border-radius: 0.5rem;
  
  p {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 1rem;
  }
}
```

> 直观上如上方就可以了，但是现实中发现不行，因为虽然给父元素 div 设置了圆角，但左下角跟右下角还是四四方方的---因为子元素 p 的棱角溢出了
> 直观上考虑可以直接给 div 设置一个 overflow：hidden 解决；

```less
div {
  position: relative;
  width: 5rem;
  height: 5rem;
  border-radius: 0.5rem;
  overflow: hidden; // 逻辑很正确，没问题，但在iphone上打开页面发现子元素 p 不知道为什么底部跟右侧都跟 div 有了一条非常细小的距离，就像被什么挤偏了一样，因为我们明明设置了 bottom：0 啊！
  
  p {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 1rem;
  }
}
```

> 好吧 换另一种方法实现吧

```less
div {
  position: relative;
  width: 5rem;
  height: 5rem;
  border-radius: 0.5rem;
  
  p {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 1rem;
    border-bottom-left-radius: 0.5rem; // 给子元素也设置个圆角即可
    border-bottom-right-radius: 0.5rem; // 给子元素也设置个圆角即可
  }
}
```

[collapsing margins] 垂直方向外边距折叠

* [stackoveflow-由来及解决方案](http://stackoverflow.com/questions/1762539/margin-on-child-element-moves-parent-element)
* [知乎-由来](https://www.zhihu.com/question/20585258?rf=32230304)
* [w3官方文档-collapsing-margins](https://www.w3.org/TR/CSS2/box.html#collapsing-margins)
* [w3官方文档-Block formatting contexts](https://www.w3.org/TR/CSS2/visuren.html#block-formatting)
 
[auto]

> 1）问题表现：在某些安卓中低端手机中，浏览器不支持 background-size: auto 100%; 中的auto

> 解决方案：安卓因为可以修改浏览器内核导致部分厂商已优化性能为理由删减部分浏览器功能导致，把 auto 设置为一个固定值即可；

[rem]

> 1）问题表现：在IOS8.3 iphone5s 上，使用rem作为边框 0.5rem border，导致边框有一条缝隙，会透视到底层的颜色，使用px则没有问题

[text-size-adjust]

> 1) 问题现象，当父元素的实际宽度超过了视窗宽度也就是100%之后，字体的大小变变的不受控制，比如当设置width = 29rem， width = 130%；都可以复现问题，测试只在部分IOS机型出现,测试机型为iphone5s v8.3，以及 iphone6 v9.3.2，复现问题需要设置 meta width=device-width,测试代码及解决方法如下

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>xxx</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=0">
  <style>
  /*问题现象，当父元素的实际宽度超过了视窗宽度也就是100%之后，字体的大小变变的不受控制，比如当设置width = 29rem， width = 130%；都可以复现问题，测试只在部分IOS机型出现,测试机型为iphone5s v8.3，以及 iphone6 v9.3.2，复现问题需要设置meta width=device-width*/
  * {
    box-sizing: border-box;
  }
  .box1 {
    width: 150%;
    /* 解决方法1. 宽度不超过100%即可,注意盒子模型的几个属性margin\padding\border的值都会影响到元素的宽度的实际范围的,所以此处使用了box-sizing: border-box;来消除这种影响 */

    /* 解决方法2. 即使宽度超过100%，设置一个具体数值的高度即可，可以是px或者rem，但不可以是百分比,目前并不明白为什么设置了之后可以解决 */
    /*height: 5rem;*/

    /* 解决方法3. 设置-webkit-text-size-adjust: none; 即可*/
    /*-webkit-text-size-adjust: none;*/

    /* 问题原因：在移动设备中浏览器为了防止出现字体太小看不清楚，有一个字体自适应算法(text inflation algorithm) text-size-adjust，当元素宽度超过了100%，即超过视窗之后，字体放大，目前查看IOS有该问题，安卓没有问题，如果关闭该行为，设置none即可*/
    /* MDN原话：When a focused element containing text use 100 % of the width of the screen, its text size is increased until it reached a readable size, without modifying the layout and removing the need of an horizontal scrollbar.*/
    /* 不成熟的翻译：当其中包含文字的元素使用到了视窗宽度100%的时候，其中的字体会增大到一个可读的大小*/
    /* MDN 链接:https://developer.mozilla.org/en-US/docs/Web/CSS/text-size-adjust */
    color: red;
    border: 1px solid #666;
  }
  .box2 {
    width: 100%;
    color: green;
    border: 1px solid #666;
  }

  .text1 {
    font-size: .8rem;
  }

  .text2 {
    font-size: 1rem;
  }
  </style>
</head>

<body>
  <ul class="box1">
    ul.box1
    <li class="text1">我的字体是0.8rem</li>
    <li class="text2">我的字体是1rem</li>
  </ul>

  <ul class="box2">
    ul.box2
    <li class="text1">我的字体是0.8rem</li>
    <li class="text2">我的字体是1rem</li>
  </ul>

  <div class="box1">
    div.box1
    <div class="text1">我的字体是0.8rem</div>
    <div class="text2">我的字体是1rem</div>
  </div>

  <div class="box2">
    div.box2
    <div class="text1">我的字体是0.8rem</div>
    <div class="text2">我的字体是1rem</div>
  </div>
  <p>box1跟box2的文字大小一样是符合预期的，如果不一样则是不符合预期的,问题原因及解决方法见style部分注释</p>
</body>

</html>
```

![webwxgetmsgimg](https://cloud.githubusercontent.com/assets/13380433/16004594/0513601a-3195-11e6-8705-794e5fba0773.jpg)

* [参考文档-MDN:text-size-adjust](https://developer.mozilla.org/en-US/docs/Web/CSS/text-size-adjust)

