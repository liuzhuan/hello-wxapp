## `wx.getSystemInfoSync()` 获取的屏幕高度有问题

据 [api][api] 介绍，`wx.getSystemInfoSync().windowHeight` 返回可使用窗口高度。

在真机测试中，发现小米、vivo 等安卓手机上多次返回的 `windowHeight` 值不一致，官方也确认这是[已知问题][known]。

### 解决方案

方案2，3待验证

方案一：延迟 600ms 读取

方案二：`wx.createSelectorQuery` 的 `selectViewport` 方法获取显示区域的尺寸

方案三：给page设置宽高100%,然后给里面的某个子元素设置宽高100%，获取到该元素的高度。

## REF

- [设备 API - 小程序][api]
- [wx.getSystemInfoSync()获取到的高度有问题 - 微信开发者论坛][known]

[api]: https://mp.weixin.qq.com/debug/wxadoc/dev/api/systeminfo.html#wxgetsysteminfosync
[height-error]: https://developers.weixin.qq.com/blogdetail?action=get_post_info&docid=0004282a4a4810c8692647a1c5bc00&highline=getSystemInfoSync&token=&lang=zh_CN
[height-error2]: https://developers.weixin.qq.com/blogdetail?action=get_post_info&docid=0000c68e7d02608be216a146059400&highline=getSystemInfoSync&token=&lang=zh_CN
[known]: https://developers.weixin.qq.com/blogdetail?action=get_post_info&docid=000cc0f09dc758077326b9a1451800&highline=getSystemInfoSync&token=&lang=zh_CN