# 组件事件

事件系统是组件间交互的主要形式。自定义组件可以触发任意的事件，引用组件的页面可以监听这些事件。

监听自定义组件事件的方法与监听基础组件事件的方法完全一致：

**代码示例：**

```html
<!-- 当自定义组件触发“myevent”事件时，调用“onMyEvent”方法 -->
<component-tag-name bindmyevent="onMyEvent" />
<!-- 或者可以写成 -->
<component-tag-name bind:myevent="onMyEvent" />
```

```js
Page({
  onMyEvent: function(e){
    e.detail // 自定义组件触发事件时提供的detail对象
  }
})
```

自定义组件触发事件时，需要使用 `triggerEvent` 方法，指定事件名、detail对象和事件选项：

**代码示例：**

```html
<!-- 在自定义组件中 -->
<button bindtap="onTap">点击这个按钮将触发“myevent”事件</button>
```

```js
Component({
  properties: {}
  methods: {
    onTap: function(){
      var myEventDetail = {} // detail对象，提供给事件监听函数
      var myEventOption = {} // 触发事件的选项
      this.triggerEvent('myevent', myEventDetail, myEventOption)
    }
  }
})
```

触发事件的选项包括：

```js
/** 事件是否冒泡 */
bubbles: Boolean = false
/** 
* 事件是否可以穿越组件边界，
* 为false时，事件将只能在引用组件的节点树上触发，不进入其他任何组件内部 
*/
composed: Boolean = false

/** 事件是否拥有捕获阶段 */
capturePhase: Boolean = false
```

**代码示例：**

```html
<!-- 页面 page.wxml -->
<another-component bindcustomevent="pageEventListener1">
  <my-component bindcustomevent="pageEventListener2"></my-component>
</another-component>
```

```html
<!-- 组件 another-component.wxml -->
<view bindcustomevent="anotherEventListener">
  <slot />
</view>
```

```html
<!-- 组件 my-component.wxml -->
<view bindcustomevent="myEventListener">
  <slot />
</view>
```

```js
// 组件 my-component.js
Component({
  methods: {
    onTap: function(){
      /** 只会触发 pageEventListener2 */
      this.triggerEvent('customevent', {})
      /** 会依次触发 pageEventListener2、 pageEventListener1 */
      this.triggerEvent('customevent', {}, { bubbles: true })
      /** 会依次触发 pageEventListener2、 anotherEventListener、 pageEventListener1 */
      this.triggerEvent('customevent', {}, { bubbles: true, composed: true })
    }
  }
})
```

## REF

- [组件事件 - 小程序][doc]

[doc]: https://mp.weixin.qq.com/debug/wxadoc/dev/framework/custom-component/events.html