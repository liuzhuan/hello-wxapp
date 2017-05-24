# event.target 和 event.currentTarget 的区别

`event.target` 是当前点击的元素，`event.currentTarget` 是注册事件的元素。

举例来说，比如有如下结构：

```html
<view class="outer" id="outer" bindtap="bindTap">
    <view class="inner" id="inner">
    </view>
</view>
```

```javascript
bindTap(e) {
    console.log(e.target.id);
    console.log(e.currentTarget.id);
}
```

当点击 .inner 时，会输出：

```
inner
outer
```

当点击 .outer 时，会输出：

```
outer
outer
```