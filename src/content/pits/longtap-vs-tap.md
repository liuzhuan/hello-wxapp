# 长按与点击事件冲突

http://www.acfunc.com/%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%B8%A9%E5%9D%91%E8%AE%B0-%E9%95%BF%E6%8C%89%E4%B8%8E%E7%82%B9%E5%87%BB%E4%BA%8B%E4%BB%B6%E5%86%B2%E7%AA%81/

作者：zhangzy

日期：2017-01-17

小程序通过 `bindtap` 和 `bindlongtap` 分别绑定点击和长按事件：

```html
<view bindtap="bindTap">Tap Me!</view>
<view bindlongtap="bindLongTap">Long Tap Me!</view>
```

根据文档，手指触控后，超过 350ms 就会执行长按事件。

如果一个元素要同时处理点击和长按，就会发生冲突。执行长按后，紧接着松开就会执行“点击”。

以下是解决方案：

```html
<view bindtouchstart="bindTouchStart" bindtouchend="bindTouchEnd" bindlongtap="bindLongTap" bindtap="bindTap">Tap & Long Tap Me!</view>
```

```javascript
bindTouchStart(e) {
    this.startTime = e.timeStamp;
},

bindTouchEnd(e) {
    this.endTime = e.timeStamp;
},

bindTap(e) {
    if (this.endTime - this.startTime < 350) {
        console.log('tap');
    }
},

bindLongTap(e) {
    console.log('long tap');
}
```