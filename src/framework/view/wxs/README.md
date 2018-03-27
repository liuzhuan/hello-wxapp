# WXS 脚本

WXS（`WeiXin Script`）是小程序的一套脚本语言，结合 WXML，可以构建出页面的结构。

注意

1. wxs 不依赖于运行时的基础库版本，可以在所有版本的小程序中运行。
2. wxs 与 javascript 是不同的语言，有自己的语法，并不和 javascript 一致。
3. wxs 的运行环境和其他 javascript 代码是隔离的，wxs 中不能调用其他 javascript 文件中定义的函数，也不能调用小程序提供的API。
4. wxs 函数不能作为组件的事件回调。
5. 由于运行环境的差异，在 iOS 设备上小程序内的 wxs 会比 javascript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。

下面是将时间戳转换为时间字符串的例子：

```js
Page({
  data: {
    timestamp: 1522117395730
  }
})
```

```html
<wxs module="m1">
  function format(ts) {
    var d = getDate(ts)
    return [d.getFullYear(), d.getMonth()+1, d.getDate()].join('-')
      + ' '
      + [d.getHours(), d.getMinutes(), d.getSeconds()].join(':')
  }

  module.exports.format = format
</wxs>

<view>{{ timestamp }}</view>
<view>{{ m1.format(timestamp) }}</view>
```

注意，wxs 获取当前日期不可以使用 `new Date()`，而需要使用 `getDate()` 代替。

## REF

- [WXS][wxs]
- [数据类型 - date][date]

[wxs]: https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxs/
[date]: https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxs/06datatype.html#date