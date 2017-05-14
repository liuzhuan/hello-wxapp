# 引用

WXML 提供两种文件引用方式 `import` 和 `include`

## import

`import` 可在当前文件使用目标文件定义的 `template`

在 item.wxml 中定义 `item` 的 `template`，比如：

```xml
<template name="item">
    <text>{{text}}</text>
</template>
```

在 index.wxml 中引用了 item.wxml，就可以使用 item 模板：

```xml
<import src="item.wxml" />
<template is="item" data="{{text: 'foobar'}}" />
```

### import 作用域

import 只会 import 目标文件中定义的 template, 而不会 import 目标文件 import 的 template。

## include

`include` 可以将目标文件除了 `<template/>` 的整个代码引入，相当于拷贝到 `include` 位置，如：

```xml
<!-- index.wxml -->
<include src="header.wxml"/>
<view body </view>
<include src="footer.html"/>
```

```xml
<!-- header.wxml -->
<view> header </view>
```

```xml
<!-- footer.wxml -->
<view> footer </view>
```

## Reference
- [引用·小程序](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/import.html)