# 自定义组件

从小程序基础库版本 **1.6.3** 开始，小程序支持简洁的组件化编程。

## 创建自定义组件

类似于页面，一个自定义组件由 `json` `wxml` `wxss` `js` 4 个文件组成。要编写一个自定义组件，首先需要在 json 文件中进行自定义组件声明（将 `component` 字段设为 `true` 可将这一组文件设为自定义组件）：

```json
{
  "component": true
}
```

同时还要编写 wxml 和 wxss。

**代码示例：**

```html
<!-- 这是自定义组件的内部WXML结构 -->
<view class="inner">
  {{innerText}}
</view>
<slot></slot>
```

```css
/* 这里的样式只应用于这个自定义组件 */
.inner {
  color: red;
}
```

⚠️ 注意：**在组件 wxss 中不应使用 ID 选择器、属性选择器和标签名选择器**。

> 不使用 ID，因为多个组件 ID 不唯一；不用属性和标签名，可能选择面过于宽泛，超出组件范围。

在自定义组件的 js 文件中，需要使用 `Component()` 来注册组件，并提供组件的属性定义、内部数据和自定义方法。

组件的属性值和内部数据将被用于组件 wxml 的渲染，其中，属性值是可由组件外部传入的。

**代码示例：**

```js
Component({
  properties: {
    // 这里定义了 innerText 属性，属性值可以在组件使用时指定
    innerText: {
      type: String,
      value: 'default value',
    }
  },
  data: {
    // 这里是一些组件内部数据
    someData: {}
  },
  methods: {
    // 这里是一个自定义方法
    customMethod: function(){}
  }
})
```

## 使用自定义组件

使用已注册的自定义组件前，首先要在页面的 json 文件中进行引用声明。此时需要提供每个自定义组件的标签名和对应的自定义组件文件路径：

```json
{
  "usingComponents": {
    "component-tag-name": "path/to/the/custom/component"
  }
}
```

这样，在页面的 wxml 中就可以像使用基础组件一样使用自定义组件。节点名即自定义组件的标签名，节点属性即传递给组件的属性值。

**代码示例：**

```html
<view>
  <!-- 以下是对一个自定义组件的引用 -->
  <component-tag-name inner-text="Some text"></component-tag-name>
</view>
```

自定义组件的 wxml 节点结构在与数据结合之后，将被插入到引用位置内。

⚠️ 注意：

- 对于基础库的 1.5.x 版本， 1.5.7 也有部分自定义组件支持。
- 因为WXML节点标签名只能是小写字母、中划线和下划线的组合，所以自定义组件的标签名也只能包含这些字符。
- 自定义组件也是可以引用自定义组件的，引用方法类似于页面引用自定义组件的方式（使用 `usingComponents` 字段）。
- 自定义组件和使用自定义组件的页面所在项目根目录名不能以“wx-”为前缀，否则会报错。
- 旧版本的基础库不支持自定义组件，此时，引用自定义组件的节点会变为默认的空节点。

## REF

- [自定义组件 - 小程序][doc]

[doc]: https://mp.weixin.qq.com/debug/wxadoc/dev/framework/custom-component/