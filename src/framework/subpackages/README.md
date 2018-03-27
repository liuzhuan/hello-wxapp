# 分包加载

> 微信 6.6 客户端（2017-12-11），1.7.3 及以上基础库开始支持，请更新至最新客户端版本，开发者工具请使用 1.01.1712150 及以上版本。

目前小程序分包大小有以下限制：

- 整个小程序所有分包大小不超过 4M
- 单个分包/主包大小不能超过 2M

## 使用方法

假设支持分包的小程序目录结构如下：

```
├── app.js
├── app.json
├── app.wxss
├── packageA
│   └── pages
│       ├── cat
│       └── dog
├── packageB
│   └── pages
│       ├── apple
│       └── banana
├── pages
│   ├── index
│   └── logs
└── utils
```

开发者通过在 `app.json` `subPackages` 字段声明项目分包结构：

```json
{
  "pages":[
    "pages/index",
    "pages/logs"
  ],
  "subPackages": [
    {
      "root": "packageA",
      "pages": [
        "pages/cat",
        "pages/dog"
      ]
    }, 
    {
      "root": "packageB",
      "pages": [
        "pages/apple",
        "pages/banana"
      ]
    }
  ]
}
```

## 打包原则

- 声明 `subPackages` 后，将按 `subPackages` 配置路径进行打包，`subPackages` 配置路径外的目录将被打包到 `app`（主包） 中
- `app`（主包）也可以有自己的 `pages`（即最外层的 pages 字段）
- `subPackage` 的根目录不能是另外一个 `subPackage` 内的子目录
- 首页的 TAB 页面必须在 `app`（主包）内

## 引用原则

- `packageA` 无法 require `packageB` JS 文件，但可以 require `app`、自己 package 内的 JS 文件
- `packageA` 无法 import `packageB` 的 template，但可以 require `app`、自己 package 内的 template
- `packageA` 无法使用 `packageB` 的资源，但可以使用 `app`、自己 package 内的资源

## 低版本兼容

由微信后台编译来处理旧版本客户端的兼容，后台会编译两份代码包，一份是分包后代码，另外一份是整包的兼容代码。 新客户端用分包，老客户端还是用的整包，完整包会把各个 `subpackage` 里面的路径放到 pages 中。

## REF

- [分包加载][subpackages]

[subpackages]: https://mp.weixin.qq.com/debug/wxadoc/dev/framework/subpackages.html
[6.6]: http://weixin.qq.com/cgi-bin/readtemplate?lang=zh_CN&t=page/faq/ios/660/index&faq=ios_660