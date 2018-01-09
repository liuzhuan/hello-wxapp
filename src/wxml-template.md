# 模板

可以在模板中定义代码片段，然后在不同地方调用。

## 定义模板

使用 `name` 属性，作为模板的名称。然后在 `<template/>` 内定义代码片段，如：

```xml
<template name="msgItem">
    <view>
        <text>{{index}}: {{msg}}</text>
        <text>Time: {{time}}</text>
    </view>
</template>
```

## 使用模板

使用 `is` 属性，声明需要使用的模板，然后将模板所需要的 `data` 传入，如：

```xml
<template is="msgItem" data="{{...item}}" />
```

```js
Page({
    data: {
        item: {
            index: 0,
            msg: 'this is a template',
            time: '2016-09-15'
        }
    }
})
```

`is` 属性可以使用 Mustache 语法，动态决定模板类型：

```xml
<template name="odd">
    <view> odd </view>
</template>

<template name="even">
    <view> even </view>
</template>

<block wx:for="{{[1, 2, 3, 4, 5]}}">
    <template is="{{ item%2 == 0 ? 'even' : 'odd' }}"/>
</block>
```

## Reference
- [模板·小程序](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/template.html)