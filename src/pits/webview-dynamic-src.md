# 动态地址的 `web-view`

在某些安卓手机（比如小米 Note 3）中，如果 `web-view` 的 `src` 是动态变化的，一定要整体生成链接地址，不要在 `wxml` 中拼接地址。否则，会导致错误的网络请求。

比如，内嵌 `web-view` 页面如下：

```html
<web-view src="http://example.com/id={{ id }}"></web-view>
```

```js
Page({
  data: {
    id: ''
  },
  onLoad (options) {
    const { id } = options
    this.setData({ id })
  }
})
```

进入页面，`onLoad` 生命周期执行之前，`data.id` 为空，`web-view` 渲染的页面是 `http://example.com/id=`。此时可能会导致页面异常。如果异常造成页面阻塞（比如，弹出 `alert` 对话框），会妨碍 `onLoad` 的执行，从而异常会持续保持。

## 解决方案

当 `web-view` 链接地址存在动态参数时，最好在 `js` 逻辑层生成完整链接地址后，统一替换 `src`。对于上述例子进行改造，如下：

```html
<web-view src="{{ url }}"></web-view>
```

```js
Page({
  data: {
    url: ''
  },
  onLoad (options) {
    const { id } = options
    const url = `http://example.com/id=${id}`
    this.setData({ url })
  }
})
```

改造后，即使没有获取到 `id`，也只是出现短暂的空白页面，不会导致明显的异常。