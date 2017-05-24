# 复杂数据的传递

http://www.acfunc.com/%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%B8%A9%E5%9D%91%E8%AE%B0-%E5%A4%8D%E6%9D%82%E6%95%B0%E6%8D%AE%E7%9A%84%E4%BC%A0%E9%80%92/

作者：zhangzy

日期：2017-01-16

页面跳转进行数据传递是一个必不可少的功能，有很多应用场景需要使用。

比如买商品的流程是“选择商品” -> “购物车” -> “物流信息” -> “确认订单”，所以在这几个不同页面进行跳转的时候，相应的数据也需要传递下去。

在 android 中，我们可以用 `Intent` 来进行数据的传递，新界面通过 `getIntent` 来进行获取。

在 iOS 中，一般是通过 controller 的属性来进行传值。

在 React-Native 中，可以在 `navigator.push` 方法传递对象。

上面三种进行数据的传递都很简单，而且也很方便。

封装一个方法：

```javascript
class Router {
  navigateTo(obj) {
    wx.navigateTo({
      url: obj.url,
      success: function(res) {
        obj.success && obj.success(res);
        if (obj.data) {
          setTimeout(() => {
            let pages = getCurrentPages();
            let curPage = pages[pages.length - 1];
            if (curPage.startPageForData) {
              curPage.startPageForData(obj.data);
            } else {
              curPage.setData(obj.data);
            }
          }, 500);
        }
      },

      fail: function(res) {
        obj.fail && obj.fail(res);
      },

      complete: function(res) {
        obj.complete && obj.complete;
      }
    });
  }
}

const router = new Router();

export default router;
```

实际上还是基于微信提供的 wx.navigateTo 接口，在跳转成功之后，我们获取页面栈里面最上层的一个，也就是最新打开的那个页面，从而进行数据绑定或是执行对应的 `startPageForData` 方法，上面我们加了1000毫秒的延时，主要是当页面跳转之后，可能没那么快注册的栈里面，不延时可能获取的还是之前页面数。

使用方法

```javascript
Page({
    data: {},
    bindTap: function(e) {
        Router.navigateTo({
            url: 'page2',
            data: {
                product: { id:1, count:2 },
                consignee: { name: "zhangzy", phoen: "1222222222", address: "地址" }
            }
        });
    }
})
```

```javascript
// page2.js

Page({
    data: {
        consignee: {},
        product: {}
    },

    startPageForData: function(data) {
        console.log(data);
    }
});
```