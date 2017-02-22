# 列表渲染

在组件上使用 `wx:for` 控制属性绑定一个数组，就能使用数组重复渲染该组件。

默认数组的当前项索引值为 `index`，当前项变量名是 `item`

```xml
<view wx:for="{{array}}">
    {{index}}: {{item.message}}
</view>
```

```js
Page({
    data: {
        array: [
            { message: 'foo' },
            { message: 'bar' }
        ]
    }
})
```

使用 `wx:for-item` 指定当前元素的变量名；`wx:for-index` 指定当前索引值变量名：

```xml
<view wx:for="{{array}}" wx:for-index="idx" wx:for-item="itemName">
    {{idx}}: {{itemName.message}}
</view>
```

`wx:for` 也可以嵌套，下面是一个九九乘法表：

```xml
<view wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="i">
    <view wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="j">
        <view wx:if="{{ i <= j }}">
            {{i}} * {{j}} = {{ i * j}}
        </view>
    </view>
</view>
```

## block wx:for

类似 `block wx:if`，也可以将 `wx:for` 用在 `<block />` 标签上，以渲染一个包含多节点的结构块。例如：

```xml
<block wx:for="{{[1, 2, 3]}}">
    <view>{{index}}:</view>
    <view>{{item}}</view>
</block>
```

## wx:key

如果希望列表项保持自己的特征和状态，需要使用 `wx:key` 来制定唯一标识符。

`wx:key` 的值以两种形式提供：

1. 字符串。
2. 保留关键字 `*this`。需要 item 本身是一个唯一字符串或数字。

示例代码：

```xml
<switch wx:for="{{objectArray}}" wx:key="unique" style="display:block;">
    {{item.id}}
</switch>
<button bindtap="switch">Switch</button>
<button bindtap="addToFront">Add to the front</button>

<switch wx:for="{{numberArray}}" wx:key="*this" style="display:block;">
    {{item}}
</switch>
<button bindtap="addNumberToFront">Add to the front</button>
```

```js
Page({
    data: {
        objectArray: [
            { id: 5, unique: 'unique_5' },
            { id: 4, unique: 'unique_4' },
            { id: 3, unique: 'unique_3' },
            { id: 2, unique: 'unique_2' },
            { id: 1, unique: 'unique_1' },
            { id: 0, unique: 'unique_0' }
        ],

        numberArray: [1, 2, 3, 4]
    },

    switch: function(e) {
        const length = this.data.objectArray.length;
        for (let i = 0; i < length; i++) {
            const x = Math.floor(Math.random() * length);
            const y = Math.floor(Math.random() * length);
            const temp = this.data.objectArray[x];
            this.data.objectArray[x] = this.data.objectArray[y];
            this.data.objectArray[y] = temp;
        }

        this.setData({
            objectArray: this.data.objectArray
        });
    },

    addToFront: function(e) {
        const length = this.data.objectArray.length;
        this.data.objectArray = [{ 
                id: length, 
                unique: 'unique_' + length 
            }].concat(this.data.objectArray);
        this.setData({
            objectArray: this.data.objectArray
        })
    },

    addNumberToFront: function(e) {
        this.data.numberArray = [this.data.numberArray.length + 1].concat(this.data.numberArray);
        this.setData({
            numberArray: this.data.numberArray
        })
    }
})
```

## Reference
- [列表渲染·小程序](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/list.html)