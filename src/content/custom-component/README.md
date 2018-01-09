# 自定义组件

2017 年 11 月 2 日官方论坛的 LastLeaf 发帖《自定义组件公测说明及入门介绍》。

> 现在，小程序基础库已经包含与自定义组件相关的支持，我们决定对这些特性开放公测。有了这些特性之后，你可以：

- 将一些复用性强的代码抽象成“自定义组件”，它们的使用方法与基础库内置的 view 、 button 等组件非常相似。
- 或者将页面拆分成几个低耦合的部分，降低页面局部更新时的渲染开销。
- 还可以将你自己制作的“自定义组件”代码文件开源出来，与其他开发者共享你的成果！

为项目启用自定义组件的方法是：

1. 用开发者工具打开小程序项目设置，**在项目设置中勾选“预览/上传时使用新特性”**，并确认使用**基础库版本是 1.6.3 或更高**；
2. 如果需要在手机上预览，请确认手机上安装的微信是最新版本（**6.5.16或更高**）。

## 如何编写一个自定义组件

每个自定义组件由四个代码文件组成：

- json 文件：用于放置一些最基本的组件配置。
- wxml 文件：组件模版。
- wxss 文件：组件的样式，只在组件内部节点上生效（这个文件是可选的）。
- js 文件：组件的js代码，承载组件的主要逻辑。

这四个文件与编写一个页面时用到的四个文件非常类似，但也有区别。下面将介绍如何利用这四个文件编写一个简单的自定义组件。

### 组件配置

编写一个自定义组件，首先应在小程序代码目录中合适的位置创建一个 json 文件。这里我们把文件放置在小程序的 `components` 目录下，名为 `custom-checkbox.json` 。

![custom components](https://mmbiz.qlogo.cn/mmbiz_png/MOicxgtDNAjDDvhcvbW1byaaiaOoBmicp9yueJIOKLmzZJOLMo7aB5Qzhk611dufiaGH6BgfJsAB6BdgIeic3IwjLBw/0?)

文件中包含内容：

```json
{
  "component": true
}
```

### 组件模板

在同一目录下，创建一个模版文件 `custom-checkbox.wxml` 。这个文件的写法与页面模版很类似：

```html
<!-- components/custom-checkbox.wxml -->
<view>
  <checkbox id="checkbox" checked />
  <label for="checkbox" class="label" bindtap="labeltap"> {{title}} </label>
</view>
```

这个模版将在组件中渲染出一个 checkbox 和一个 label 。

### 组件样式

同样类似于页面， wxss 文件中可以指定组件节点的样式。其中的样式仅在组件内部生效。需要注意的是，样式**只能通过类选择器**（如 `.the-class-name` ）来指定：

```css
/* components/custom-checkbox.wxss */
.label {
 color: blue;
}
```

### 组件定义

组件的 js 文件中必须包含组件定义。**定义时使用 `Component` 构造器**：

```js
// components/custom-checkbox.js
Component({
  properties: {
    title: {
      type: String
    }
  },
  data: {},
  methods: {
    labeltap: function() {
      console.log('label被点击了一下！')
    }
  }
})
```

简单的 Component 构造器调用包含3个定义段： `methods` 中的方法可以用来包含组件的事件回调函数； `data` 是组件的内部数据，可以用 `this.setData` 方法来改变； `properties` 中声明这个组件的属性，上例中声明了 title 属性，这样，组件外部在使用组件时就可以指定这个属性的值。

这样我们就编写好了 `custom-checkbox` 这个组件。

> 与 React 组件化的思路有些类似：

| 对比 | 微信小程序 | React |
| --- | --- | --- |
| 属性 | properties | props |
| 状态 | data | state |
| 设置状态 | setData | setState |

## 如何使用自定义组件

使用上面这个自定义组件的方法很简单。首先，在需要使用这个组件的页面对应的 json 文件中，添加下面的自定义组件声明：

```json
{
  "usingComponents": {
    "custom-checkbox": "/components/custom-checkbox"
  }
}
```

然后就可以在页面对应的 wxml 文件中使用了：

这样，在页面上最终呈现的效果如下：

![Custom Component](https://mmbiz.qlogo.cn/mmbiz_png/MOicxgtDNAjDDvhcvbW1byaaiaOoBmicp9yAgDibZibrFLwNtFlqmmgOqjibn5Cpdu0zCrPvlFGzH1USEkAJp4gga1PA/0?)

在自定义组件中引用其它自定义组件的方法也类似。

## 相关资源

- Min: 一套面向小程序组件化开发的解决方案。

## REF

- [自定义组件 - 微信小程序官方文档][doc]
- http://developers.weixin.qq.com/blogdetail?action=get_post_info&lang=zh_CN&token=&docid=34fde56e6ee55dac840952f4bf46cbfb
- https://mp.weixin.qq.com/debug/wxadoc/dev/framework/custom-component/index.html
  - https://mp.weixin.qq.com/debug/wxadoc/dev/framework/custom-component/events.html
  - https://mp.weixin.qq.com/debug/wxadoc/dev/framework/custom-component/behaviors.html
  - https://mp.weixin.qq.com/debug/wxadoc/dev/framework/custom-component/component.html
- https://github.com/meili/min-cli
- https://github.com/meili/minui

[doc]: https://mp.weixin.qq.com/debug/wxadoc/dev/framework/custom-component/