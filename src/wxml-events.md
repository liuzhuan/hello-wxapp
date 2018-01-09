# 事件

## 事件的使用方法

* 在组件中绑定

```xml
<view id="tapTest" data-hi="WeChat" bindtap="tapName">Click me!</view>
```

* 相应 Page 写上相应的事件处理函数，参数是 event

```js
Page({
    tapName: function(event) {
        console.log(event);
    }
})
```

## 事件分类

事件分为冒泡事件和非冒泡事件。

WXML 的冒泡事件列表：

类型 | 触发条件
--- | ---
touchstart | 触摸开始
touchmove | 触摸移动 
touchcancel | 触摸打断
touchend | 触摸结束
tap | 触摸后马上离开
longtap | 触摸超过 350 毫秒再离开

除上表之外的其他组件自定义事件，如无特殊申明，都是非冒泡事件，如 `<form/>` 的 submit 事件，`<input/>` 的 input 事件，`<scroll-view/>` 的 scroll 事件等。

## 事件绑定

事件绑定写法同组件的属性，以 key, value 的形式。

* key 以 `bind` 或 `catch` 开头，后跟事件类型，如 `bindtap`, `catchtouchstart`
* value 是字符串，需要在对应的 Page 中定义同名函数。

`bind` 事件绑定不阻止冒泡事件向上冒泡，`catch` 事件绑定阻止冒泡事件向上冒泡。

```xml
<view id="outter" bindtap="handleTap1">
    outer view
    <view id="middle" catchtap="handleTap2">
        middle view
        <view id="inner" bindtap="handleTap3">
            inner view
        </view>
    </view>
</view>
```

## 事件对象

当组件触发事件时，逻辑层绑定该事件的处理函数会收到一个事件对象。

BaseEvent 基础事件对象属性列表：

属性 | 类型 | 说明
--- | --- | ---
type | String | 事件类型
timeStamp | Integer | 事件生成时的时间戳
target | Object | 触发事件的组件的一些属性值集合
currentTarget | Object | 当前组件的一些属性值集合

CustomEvent 自定义事件对象属性列表（继承自 BaseEvent）:

属性 | 类型 | 说明
--- | --- | ---
detail | Object | 额外信息

TouchEvent 触摸事件对象属性列表（继承自 BaseEvent）:

属性 | 类型 | 说明
--- | --- | ---
touches | Array | 触摸事件，当前触点信息
changedTouches | Array | 当前变化的触点信息

touches 是一个数组，每个元素为一个 Touch 对象（canvas 触摸事件中携带的 touches 是 CanvasTouch 数组）。表示当前停留在屏幕上的触摸点。

Touch 对象

属性 | 类型 | 说明
--- | --- | ---
identifier | Number | 触摸点的标识符
pageX, pageY | Number | 距离文档左上角的距离
clientX, clientY | Number | 距离页面可视区域左上角距离

CanvasTouch 对象

属性 | 类型 | 说明
--- | --- | ---
identifier | Number | 触摸点的标识符
x, y | Number | 距离 Canvas 左上角的距离

## Reference
- [事件·小程序](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/event.html)