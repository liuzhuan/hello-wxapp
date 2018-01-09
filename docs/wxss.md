# WXSS

WXSS 具有 CSS 大部分特性。为了更适合开发微信小程序，对 CSS 进行了扩充和修改。

与 CSS 相比扩展特性有：
* 尺寸单位
* 样式导入

## 尺寸单位

rpx(responsive pixel): 可根据屏幕尺寸自适应。规定屏幕宽度为 750rpx。如在 iPhone6 上，屏幕宽度为 375px，共有 750 个物理像素，则 750rpx = 375px = 750物理像素，1rpx = 0.5rpx = 1物理像素

设备 | rpx 换算 px (屏幕宽度/750) | px 换算 rpx (750/屏幕宽度)
--- | -------------------------| ------------------------
iPhone5 | 1rpx = 0.42px | 1px = 2.34rpx
iPhone6 | 1rpx = 0.5px | 1px = 2rpx
iPhone6 Plus | 1rpx = 0.552px | 1px = 1.81rpx

## 样式导入

使用 `@import` 可以导入外联样式表。

```css
/* common.wxss */
.small-p {
    padding: 5px;
}
```

```css
/* app.wxss */
.middle-p {
    padding: 15px;
}
```

## 内联样式

框架组件支持使用 `style`, `class` 属性控制组件的样式。

* style: 静态组件统一写到 class ，style 接收动态样式。

```xml
<view style="color:{{color}};"></view>
```

* class: 指定样式规则

## 选择器

目前支持的选择器有：

* .class
* #id
* element
* element, element
* ::after
* ::before

## Reference
- [WXSS·小程序](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxss.html)