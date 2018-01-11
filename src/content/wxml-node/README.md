## WXML 节点信息

所有 API 函数如下：

```javascript
/** return a SelectorQuery instance */
var query = wx.createSelectorQuery()
query.select('#the-id').boundingClientRect()
query.selectViewport().scrollOffset()
query.exec(function(res) {
  res[0].top // the top border location
  res[1].scrollTop // viewport's vertical scroll section
})
```

SelectorQuery 对象的方法列表：

> lib 1.6+

```javascript
/** return component query */
selectorQuery.in(component)

/** return a NodesRef instance, return only the first matched node */
selectorQuery.select(selector)

/** return all the matched nodes */
selectorQuery.selectAll(selector)

/** return a NodesRef instance about current viewport */
selectorQuery.selectViewport()

/** get bounding box */
nodesRef.boundingClientRect(callback)
wx.createSelectorQuery().select('#the-id').boundingClientRect(function(rect){
  rect.id,
  rect.dataset,
  rect.left,
  rect.right,
  rect.top,
  rect.bottom,
  rect.width,
  rect.height
})

nodesRef.scrollOffset(callback)
wx.createSelectorQuery().selectViewport().scrollOffset(function(res){
  res.id,
  res.dataset,
  res.scrollLeft,
  res.scrollTop
}).exec()

nodesRef.fields(fields, callback)
wx.createSelectorQuery().select('#the-id').fields({
  dataset: true,
  size: true,
  scrollOffset: true,
  properties: ['scrollX', 'scrollY']
}, function(res) {
  res.dataset,
  res.width,
  res.height,
  res.scrollLeft,
  res.scrollTop,
  res.scrollX,
  res.scrollY
}).exec()

selectorQuery.exec(callback)
```

selector 支持的语法包括：

- `#the-id`
- `.a-class.another-class`
- `.the-parent > .the-child`
- `.the-ancestor .the-descendant`
- `.the-ancestor >>> .the-descendant`
- `#a-node, .some-other-nodes`

## REF

- [WXML节点信息API][api]

[api]: https://mp.weixin.qq.com/debug/wxadoc/dev/api/wxml-nodes-info.html