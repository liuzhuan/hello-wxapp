# 小程序支付流程

## wx.requestPayment(OBJECT)

发起微信支付。

OBJECT 参数说明：

参数 | 类型 | 必填 | 说明
---- | --- | --- | ---
timeStamp | String | 是 | 时间戳，秒数
nonceStr | String | 是 | 随机字符串，长度在32个字符以下
package | String | 是 | 统一下单接口返回的 prepay_id 参数值
signType | String | 是 | 暂支持 MD5
paySign | String | 是 | 签名，具体签名方案
success | Function | 否 | 成功回调函数
fail | Function | 否 | 失败回调函数
complete | Function | 否 | 接口调用结束的回调函数

回调结果：

回调类型 | errMsg | 说明
------ | ------- | ---
success | requestPayment:ok | 调用支付成功
fail | requestPayment:fail cancel | 用户取消支付
fail | requestPayment:fail (detail message) | 调用支付失败，其中 detail message 为后台返回的详细失败原因

示例代码

```js
wx.requestPayment({
	'timeStamp': '',
	'nonceStr': '',
	'package': '',
	'signType': 'MD5',
	'paySign': '',
	'success': function(res) {},
	'fail': function(res) {}
})
```

> 这些参数从哪些接口获得？

![小程序支付模式](https://pay.weixin.qq.com/wiki/doc/api/img/wxa-7-2.jpg)


## Reference
- [微信支付·小程序](https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-pay.html)
- [【微信支付】微信小程序支付开发者文档](https://pay.weixin.qq.com/wiki/doc/api/wxa/wxa_api.php?chapter=7_3&index=1)
- [业务流程](https://pay.weixin.qq.com/wiki/doc/api/wxa/wxa_api.php?chapter=7_4&index=2)