自我总结
-------

### CSS

* LESS [嵌套、变量、函数等]

* animation: [@see Web动画]

* backgournd: [@see CodePen-HobbitRoom] [@see css-objects]

* box-shadow: [@see PixArt]


### JS

* 基本数据类型

* 对象

> 普通对象

> 函数对象

> 原型对象

> 原型链

* 函数式编程: 解耦，模块化

> 柯里化 / thunk函数 一个意思

> 递归，尾递归优化

* 算法 / 数学

> 排序算法 [@see GitHub-sortAlgorithms] ，按照别人的库写的，但当时是能够完全自己默写全部排序算法类型的，不过之后也没有项目能够用到，现在只记得算法大致的意思，具体步骤需要再去熟悉。算法类型有：冒泡排序，选择排序，插入排序，希尔排序，堆栈排序，递归排序，归并排序，堆排序。

> Tween 函数模型（含贝塞尔曲线） [@see GitHub-Tween & CodePen-TweenCurve]

> 常用数学公式 [@see GitHub-Math]，目前仅出于自己项目中用到的一些数学公式原型的函数进行总结，比如已知圆心，半径，角度，求该角度圆上的点的坐标。

> 常用数学概念：斐波那契数列，贝塞尔曲线，栈，队列，链表，集合，字典，树

> 列表（有序不一定唯一）-> Array

> 有序集合（有序唯一的项 value）-> Set

> 字典（无序唯一的键值组合 key-value）-> Object

> 字典（无序唯一的项 value-value）-> Map

> 在实际项目中我们会有种种数据结构的需求，只要是有逻辑的，在现实中则一定有相应的数学模型，这些数学模型的特质即对应数据结构对象的属性，我们只要能够用函数实现数学模型的种种属性，即可以说我们创建了该数学模型对应的数据结构。

* ES6

> 过了一遍，并没有太多的项目使用ES6的经验，不过一般的箭头函数，生成器函数没什么问题。

> bable： 测试过，并没有太多的项目使用经验

* H5动画

> 对各类H5动画都比较熟悉[@see Web动画.md]，对应的实例有 H5战斗，指纹解锁，方形抽奖转盘，圆形抽奖转盘，模拟探探等

* H5游戏

> Team 做过扫雷，赛车，你画我猜等，我没有参与，不过也是比较熟悉的。

* Canvas

> [@see H5分享图片合成] [@see Tween Curve] [@see 婚礼邀请函]

* SVG

> [@see SVG-demos]

* WebApp

> 参与过一个非常复杂的WebApp的后期优化与维护，自己为了了解项目做过整体图层分析，模块解耦，通信机制梳理等工作。

* HybirdApp

> 其实个人认为我们现在App上的模式就属于HybirdApp，整体架构依然是Native，不过很多页面、功能的实现都靠Web来实现，Native与Web之间定义了通信协议，可以用来传递信息，也可以添加事件，来更加灵活的传递信息，非常适合快速开发，快速迭代的产品节奏。

* 异步 [@see async.md]

* 编码 [@see 编码.md]

### 库

* Jquery

> 刚入门时用的比较多，也用过很多Jq的库，后来单位用的zepto，现在偶尔用。

* vue

> 一般，在1.0的时候结合 webpack 做过一些组件化的demo，实际项目中没有用过组建化的东西，只是使用一些生命周期的属性，method，data渲染，computed计算等，了解vue-resources，没用过，实际中我们前期一直在使用的jhtmls，结合 事件代理模式，就我们的业务来看，感觉在稳定性，兼容性上比vue要好。

* 其他库

> jhtmls jdists swiper iscroll echarts h5ajax h5tap tier(同事写的移动端弹窗库)等


### 工具

* 包管理工具：npm bower composer

* 常用网站：github.com gitlab.com copend.io , 绘图 processon naotu

* 本地工具：Sublime SourceTree Xshell Xftp BeyondCompare Fiddler


* 构建工具

> FIS3: 写过一个简单的插件 [@see npmjs.org fis-parser-webp]，服务器部署，接收配置，本地配置，都没问题。

> webpack: 去年用过几天，写过一个项目[@see h5LocalChart & vue+webpack]，用的不多，主要还是fis3


* 持续集成 / CI

> Jenkins 由本地gitlab 添加钩子，完成后自动向ci发起通知，并添加system账号为master权限，ci新建该项目并接受该gitlab地址的项目。build when a change is pushed to gitlab.

栗子

```shell
export PATH=$PATH:/usr/local/bin:/usr/local/git/bin

# 安装依赖

bower install
npm install

if [ ${GIT_BRANCH} == "origin/master" ]
then
  npm run develop
elif [ ${GIT_BRANCH} == "origin/online" ]
then
	# 可以以此设置线上线下不同配置，以此保护关键信息
  yes | cp -R /home/webdir/ci/deploy-10.**.**.**/**/* ./		

  npm run online
fi
```


### Node

> 写过一些相关的项目，ws，express，fs，require，co，ejs等都用过，一般，由于我们本身项目太杂，东一榔头，西一棒槌的，并没有能好好的深耕nodejs

> pm2部署没什么问题

### PHP

> 写过微信公众号接入，消息回复、JSSDK分享、Oauth2认证（微信\QQ\微博）、微信办公等项目，熟悉整套H5分享、授权、注册的流程，不过本身PHP功底差，属于能写，边查变写类型。

### 数据库

* MySQL

* Redis


### Linux

> 有个人的阿里云服务器，偶尔会捣鼓重装，部署些环境

### Nginx

* 路由

> 配置Nginx路由没什么问题，不同域名配置，rewrite，set，正则匹配等，比如 YaaErr.com 的路由 YaaErr.com/room/room_id 等规则

* 日志

> 能够通过conf文件得到日志信息，并进行一般的查询

### Shell

> 写过两三个相关的项目 [@see shell.md]，一般，靠查询与实践



### 个人

* Yaaerr.com 芽儿：目前是一个Transform + 动画 + ws 的伪3D聊天室，其实这个产品是我对Web的期许和看法的参造物，详见 [@see FOE]。

* jishuqidian.com 技术奇点：15年的时候做了前后台的功能，包括编辑文章，上传图片等，感觉太low暂时空着。
