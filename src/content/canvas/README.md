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

所有的 [api][api] 如下所示：

```javascript
wx.createCanvasContext(canvasId, this)
wx.canvasToTempFilePath({
    x: 0,
    y: 0,
    width: 100,
    height: 100,
    destWidth: 100,
    destHeight: 100,
    canvasId: 'myCanvas',
    fileType: 'jpg', // jpg | png
    quality: 1.0, // (0, 1]
    success: function(res) {
        console.log(res.tempFilePath)
    }
})

ctx.setFillStyle('red')
ctx.setStrokeStyle('red')

ctx.clip()

ctx.rect(10, 10, 150, 75)
ctx.fillRect(10, 10, 150, 75)
ctx.strokeRect(10, 10, 150, 75)
ctx.clearRect(0, 0, 100, 100)
ctx.setShadow(offsetX, offsetY, blur, color)

ctx.createLinearGradient(x0, y0, x1, y1)
ctx.createCircularGradient(x, y, r)
ctx.addColorStop(0, 'red')

ctx.setLineWidth(5)
ctx.setLineCap('butt') // butt | round | square
ctx.setLineJoin('bevel') // bevel | round | miter
ctx.setMiterLimit(10)

ctx.beginPath()
ctx.closePath()

ctx.moveTo()
ctx.lineTo()
ctx.arc(x, y, r, sAngle, eAngle, counterclockwise)
ctx.quadraticCurveTo(cpx, cpy, x, y)
ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)

ctx.scale(scaleWidth, scaleHeight)
ctx.rotate(20 * Math.PI / 180)
ctx.translate(20, 20)

ctx.setFontSize(12)
ctx.fillText()
ctx.setTextBaseline('top') // top | bottom | middle | normal
ctx.setTextAlign('left') // left | center | right

ctx.stroke()
ctx.fill()

ctx.setGlobalAlpha(0.2)
ctx.save()
ctx.restore()
ctx.draw(reserve, callback) // true
ctx.drawImage(imageResource, x, y, width, height)
```

### REF

- [canvas 组件][component]
- [canvas API][api]

[component]: https://mp.weixin.qq.com/debug/wxadoc/dev/component/canvas.html
[api]: https://mp.weixin.qq.com/debug/wxadoc/dev/api/canvas/reference.html