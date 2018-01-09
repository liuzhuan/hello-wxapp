## 兼容·小程序

https://mp.weixin.qq.com/debug/wxadoc/dev/framework/compatibility.html

文档会在组件、API等页面描述中带上各个功能所支持的版本号。

可以通过 `wx.getSystemInfo` 或者 `wx.getSystemInfoSync` 获取到小程序的基础库版本号。

以下列出了三种兼容方式：接口(Interface)、参数(Parameter)和组件(Component)

### 1. Interface

```javascript
if (wx.chooseAddress) {
  wx.chooseAddress();
}
```

### 2. Parameter

```javascript
/** `sdkVersion` 是 SDK 基础库版本，注意与 `version` 微信版本的区别 */
const sdkVersion = wx.getSystemInfoSync().sdkVersion || '1.0.0';
const [MAJOR, MINOR, PATCH] = sdkVersion.split('.').map(Number);

const canIUse = apiName => {
    if (apiName === 'showModal.cancel') {
        return MAJOR >=1 && MINOR >= 1;
    }

    return true;
}

wx.showModal({
    success: function(res) {
        if (canIUse('showModal.cancel')) {
            console.log(res.cancel);
        }
    }
});
```

### 3. 组件

```javascript
Page({
  data: {
    canIUse: canIUse()
  }
});
```

```html
<button wx:if="{{canIUse}}" open-type="contact">客服消息</button>
<contact-button wx:else></contact-button>
```