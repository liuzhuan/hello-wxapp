# 页面关闭数据传递

http://www.acfunc.com/%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%B8%A9%E5%9D%91%E8%AE%B0-%E9%A1%B5%E9%9D%A2%E8%BF%94%E5%9B%9E%E5%80%BC%E4%BC%A0%E9%80%92/

作者：zhangzy

日期：2017-01-14

在页面关闭的时候，将数据传递到上一个界面，这是一个很常用的功能，一般用于数据选择，比如淘宝的收货人选择，需要在收货人管理界面将选择的收货人信息传递到订单界面。

在 Android 中，activity 自带有 `activityForResult`，进行传递。

封装自己的 pageForResult

```javascript
navigateBack(obj){
    let delta = obj.delta ? obj.delta : 1;

    if (obj.data) {
        let pages = getCurrentPages();
        let curPage = pages[pages.length - (delta + 1)];
        if (curPage.pageForResult) {
        curPage.pageForResult(obj.data);
        } else {
        curPage.setData(obj.data);
        }
    }

    wx.navigateBack({
        delta: delta,
        success: function(res) {
        obj.success && obj.success(res);
        },

        fail: function(err) {
        obj.fail && obj.fail(err);
        },

        complete: function() {
        obj.complete && obj.complete();
        }
    })
}
```

使用起来很简单

```javascript
let invoice = {
    type: type,
    header: header,
    content: content,
    header_content: this.data.headerContent
}

Router.navigateBack({
    data: {
        invoice: invoice
    }
});
```

