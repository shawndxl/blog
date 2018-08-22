IOS Universal Links / 通用链接
-----------------

出发点：产品需求在web中打开页面直接跳到本地app中。实现这个可以通过IOS的Universal Links。app工程师跟web工程师协作开发。此主要叙述web部分的工作遇到的问题。

> 市场上有专门实现该功能的库——魔窗mlink，较好实现可参考一直播，虽亲测也不怎么完美

1、nginx配置

```shell
# 假设：servername www.baidu.com
location = /apple-app-site-association {
    default_type application/json;
    return 200 '{"applinks":{"apps":[],"details":[{"appID":"get appId from IOS engineer","paths":["/whateveryouwant/*"]}]}}';
}
```

> 在安装app的时候IOS会自动去包中写的位置发起一个HTTPS的请求，所以nginx配置必须包含HTTPS配置，IOS检测后，只有后续页面中有链接不管是否是https只要符合host加path，就可以出发跳转

> 做了nginx配置之后，可以做个测试，访问https://www.baidu.com/apple-app-site-association，配置生效的话应该得到 JSON

```json
{"applinks":{"apps":[],"details":[{"appID":"get appId from IOS engineer","paths":["/whateveryouwant/*"]}]}}
```

2、触发场景分两种

1) 任意域名的中间页用户主动触发后的跳转，且跳转的链接跟nginx中设置的paths参数对应，如：

```html
<a href="https://www.baidu.com/whateveryouwant/index.php"></a>
```

> 直接打开https://www.baidu.com/whateveryouwant/index.php是不能触发的，且由其他页面跳转但并非用户主动触发（比如直接php header跳转，js location 跳转）也是无效的，有点类似IOS非用户主动触发不能播放音频视频一样。

2) 触发IOS自身的域名识别，备忘录能够触发IOS的选择，从**app中打开

> app页面对于由web跳进来的页面在右上角会显示链接，点击链接可以再回去，再次进入则受到了影响，再次打开则不生效，此时可以通过方法二选择从app中打开，然后再次通过微信、QQ、safari就都可以了

> 在配置生效或者更改后，所有的用户都需要重新安装app才可以，因为只有重新安装后才会触发IOS请求JSON矫正本地数据