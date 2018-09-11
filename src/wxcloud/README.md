# 小程序云开发

> 云开发为开发者提供完整的云端支持，弱化后端和运维概念，无需搭建服务器，使用平台提供的 API 进行核心业务开发，即可实现快速上线和迭代，同时这一能力，同开发者已经使用的云服务相互兼容，并不互斥。

云开发能力从基础库 `2.2.3` 开始支持。

`project.config.json` 中增加了字段 `cloudfunctionRoot` 用于指定存放云函数的目录。

首先需要开通云开发。开通后自动获得一套云开发环境。

各个环境相互隔离，每个环境都包含独立的数据库实例、存储空间、云函数配置等资源。每个环境都有唯一的环境ID标识。

建议每一个正式环境都搭配一个测试环境。

云端运行环境为 `Node.js`，开发时需要使用 `Node.js` 和 `npm`。

使用云开发需在云函数目录中安装 `wx-server-sdk` 依赖：

```sh
$ npm install --save wx-server-sdk
```

## 三大基础能力

1. 云函数：在云端运行的代码。
2. 数据库：一个可以在小程序前端和云函数中读写的JSON数据库。
3. 存储：在小程序前端直接上传/下载云端文件。

### 1. 云函数

> 云函数是一段运行在云端的代码，无需管理服务器，在开发工具内编写、一键上传部署即可运行后端代码。

> 小程序内提供了专门用于云函数调用的 API。开发者可以在云函数内获取到每次调用的上下文（`appid`、`openid`等），无需维护复杂的鉴权机制，即可获取天然可信任的用户登录态（`openid`）。

比如，定义一个云函数，命名为 add，功能是将传入的两个参数 a 和 b 相加：

```js
// index.js 是入口文件
exports.main = (event, context) => {
  let { userInfo, a, b } = event;
  let { opneId, appId } = userInfo;
  let sum = a + b;

  return {
    openId,
    appId,
    sum
  }
}
```

开发者工具中上传部署云函数后，可以在小程序中如此调用：

```js
// callback style
wx.cloud.callFunction({
  name: 'add',
  data: {
    a: 12,
    b: 19
  },
  complete: console.log
})

// promise style
wx.cloud.callFunction({
  name: 'add',
  data: {
    a: 12,
    b: 19
  }
}).then(console.log);
```

### 2. 数据库

云开发提供了一个 JSON 数据库，每条记录都是一个JSON格式的对象。一个数据库可以有多个集合（相当于关系型数据库中的表），集合可看作一个JSON数组。

关系型数据库和JSON数据库的概念对应关系如下：

| 关系型 | 文档型 |
| --- | --- |
| 数据库 database | 数据库 database |
| 表 table | 集合 collection |
| 行 row | 记录 record/doc |
| 列 column | 字段 field |

每条记录都有一个 `_id` 字段用以唯一标志一条记录、一个 `_openid` 字段用以标志记录的创建者，即小程序的用户。

特别需要注意的是，在管理端（控制台和云函数）中创建的不会有 `_openid` 字段，因为这是属于管理员创建的记录。

数据库 API 分为小程序端和服务端两部分。

数据库API包含增删改查的能力，使用API操作数据库只需三步：

1. 获取数据库引用
2. 构造查询/更新条件
3. 发出请求

以下是一个小程序中查询数据库的例子：

```js
const db = wx.cloud.database();
db.collection('books').where({
  publishInfo: {
    country: 'United States'
  }
}).get({
  success: function(res) {
    console.log(res);
    // => [{ "title": "The Catcher in the Rye", ... }]
  }
})
```

### 3. 存储

在小程序端可以分别调用 `wx.cloud.uploadFile` 和 `wx.cloud.downloadFile` 完成上传和下载云文件操作。

下面简单的几行代码，即可实现在小程序内让用户选择一张图片，然后上传到云端管理的功能：

```js
wx.chooseImage({
  success: chooseResult => {
    wx.cloud.uploadFile({
      cloudPath: 'my-photo.png',
      filePath: chooseResult.tempFilePaths[0],
      success: res => {
        console.log('上传成功', res);
      }
    })
  }
})
```

如果要深入理解，可以参考[开发指引](./guide.md)。

## REF

- [小程序·云开发][started]

[started]: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/getting-started.html