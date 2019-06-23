# 如何滚动到某个页面元素？

页面滚动需要使用 [`wx.pageScrollTo()`](https://developers.weixin.qq.com/miniprogram/dev/api/ui/scroll/wx.pageScrollTo.html)

```js
wx.pageScrollTo({
  // 页面目标位置，单位 px
  scrollTop: 20,
  // 滚动动画时长，单位 ms
  duration: 200,
})
```

如何获取某个元素的位置呢？需要使用 [`wx.createSelectorQuery()`](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/wx.createSelectorQuery.html)，以便[获取界面上的节点信息](https://developers.weixin.qq.com/miniprogram/dev/framework/view/selector.html)。

```js
const query = wx.createSelectorQuery()
query.select('#the-id').boundingClientRect()
query.selectViewport().scrollOffset()
query.exec(function(res){
  res[0].top       // #the-id节点的上边界坐标
  res[1].scrollTop // 显示区域的竖直滚动位置
})
```

完整的代码如下：

```js
function getScrollTop (el, next) {
  if (!el) return;

  const query = wx.createSelectorQuery();
  query.select(el).boundingClientRect();
  query.selectViewport().scrollOffset();
  
  query.exec(function(res) {
      if (!res) return;

      const [target, screen] = res;
      if (!target || !screen) return;

      const targetTop = target.top || 0;
      const screenScrollTop = screen.scrollTop || 0;
      const targetScrollTop = targetTop + screenScrollTop;
      if (next && typeof next === 'function') {
          next(targetScrollTop);
      }
  })
}
```