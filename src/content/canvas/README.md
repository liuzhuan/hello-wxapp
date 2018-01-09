## 画布

小程序的画布 canvas。

canvas 画布的默认尺寸是 300 x 225。

### 组件

canvas 组件具有以下属性：

- `canvas-id` 唯一标识符
- `disable-scroll` 禁止滚动
- `bindtouchstart`
- `bindtouchmove`
- `bindtouchend`
- `bindtouchcancel`
- `bindlongtap` 长按 500ms 后触发
- `binderror` 错误处理函数，`detail = { errMsg: 'something wrong' }`

⚠️ 注意：canvas 是客户端原生组件，层级最高，`z-index` 对其无效。请勿在 `scroll-view`, `swiper`, `picker-view`, `movable-view` 中使用 canvas 组件。

### API

![Canvas API](../../assets/weapp-canvas.png)

### REF

- [canvas 组件][component]
- [canvas API][api]

[component]: https://mp.weixin.qq.com/debug/wxadoc/dev/component/canvas.html
[api]: https://mp.weixin.qq.com/debug/wxadoc/dev/api/canvas/reference.html