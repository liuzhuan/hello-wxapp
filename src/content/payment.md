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

## 开放模式

在进行微信支付开发之前，深刻理解微信支付的账号关系非常有助于你使用微信以及微信支付的能力。微信支付对商户开放的所有面对用户使用的 api，都是由 appid 和 mch_id 成对使用的。微信支付开放的能力主要分2大类，普通模式和服务商模式。

### 普通商户版

最常规的普通模式，适用于有自己开发团队或外包开发商的直连商户收款。

![normal pay](https://pay.weixin.qq.com/wiki/doc/api/img/chapter7_10_1.png)

- appid 必须为最后拉起收银台的小程序 appid； 
- mch_id 为和 appid 成对绑定的支付商户号，收款资金会进入该商户号； 
- trade_type 请填写 JSAPI； 
- openid 为 appid 对应的用户标识，即使用 `wx.login` 接口获得的 openid

![normal flow](https://pay.weixin.qq.com/wiki/doc/api/img/wxa-7-2.jpg)

mch_id 是什么？

微信支付分配的商户号。可以在小程序的微信公众平台 /【微信支付】/【M-A授权】看到。

### 服务商版

第三方服务商申请自己的服务号 appid，并通过该服务号 appid 申请服务商 mch_id，以此获得微信支付服务商能力。再通过服务商 mch_id 为所服务的特约商户申请创建微信支付 sub_mch_id，创建好的 sub_mch_id 默认和服务商的 mch_id 建立父子授权关系。以此来使用微信支付提供的开放接口，对特约商户及用户提供服务。

同时，微信支付为服务商模式下的每一条“mch_id-sub_mch_id父子授权关系”上，都开放了一些开发配置能力供服务商配置，包括不限于支付授权目录、推荐关注的 appid、sub_appid 等。拿小程序支付举例，服务商订单由哪个小程序调用 js 拉起支付，则需要在特约商户开发配置中将该小程序 appid 配置成 sub_appid。每条父子关系上的sub_appid 可以为多，用以满足不同的场景需求，但每笔交易只能使用 1 个。

![server flow](https://pay.weixin.qq.com/wiki/doc/api/img/chapter7_10_2.png)

最常规的第三方模式，第三方帮特约商户申请商户号并为他进行支付开发，第三方本身不经手资金，支付成功后资金直接进入特约商户商户号。

![normal server diagram](https://pay.weixin.qq.com/wiki/doc/api/img/chapter7_10_3.png)

注意事项：

- appid 为和服务商商户号绑定的服务商 appid，一般情况为认证的服务号 appid； 
- mch_id 为服务商商户号，目前仅在认证服务号后台（mp.weixin.qq.com）开放申请服务商商户号，申请开通后即在微信支付系统创建绑定关系； 
- sub_mch_id 为和服务商商户号有父子绑定关系的子商户号； 
- sub_appid 为服务商模式的场景 appid，在小程序中拉起支付时该字段必传； 
- trade_type 请填写 JSAPI； 
- openid 为 appid 对应的微信用户标识； 
- sub_openid 为 sub_appid 对应的微信用户标识，小程序服务商模式下单中的 openid 和 sub_openid 必须至少传其中一个，在小程序中拉起支付一般情况下只能获取到 sub_openid，即使用 wx.login 接口获得的 openid

## Reference
- [微信支付·小程序](https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-pay.html)
- [【微信支付】微信小程序支付开发者文档](https://pay.weixin.qq.com/wiki/doc/api/wxa/wxa_api.php?chapter=7_3&index=1)
- [业务流程](https://pay.weixin.qq.com/wiki/doc/api/wxa/wxa_api.php?chapter=7_4&index=2)
- [微信支付 | 商户平台][pay]
- [普通商户接入][normal]
  - [普通商户接入业务流程][normal-flow]
- [服务商接入][sl]
- [银行服务商接入][bank]
- [微信支付开放模式介绍][mode]

[pay]: https://pay.weixin.qq.com/
[normal]: https://pay.weixin.qq.com/wiki/doc/api/index.html
[normal-flow]: https://pay.weixin.qq.com/wiki/doc/api/wxa/wxa_api.php?chapter=7_4&index=3
[sl]: https://pay.weixin.qq.com/wiki/doc/api/sl.html
[bank]: https://pay.weixin.qq.com/wiki/doc/api/bank.html
[mode]: https://pay.weixin.qq.com/wiki/doc/api/wxa/wxa_sl_api.php?chapter=7_10&index=1