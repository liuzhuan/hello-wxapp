# 🌍 使用地图组件

**Table of Contents**

- [名词解释](#%E5%90%8D%E8%AF%8D%E8%A7%A3%E9%87%8A)
  - [WGS84](#wgs84)
  - [GCJ-02](#gcj-02)
  - [火星坐标系统](#%E7%81%AB%E6%98%9F%E5%9D%90%E6%A0%87%E7%B3%BB%E7%BB%9F)
- [组件](#%E7%BB%84%E4%BB%B6)
  - [markers](#markers)
- [API](#api)
  - [获取位置](#%E8%8E%B7%E5%8F%96%E4%BD%8D%E7%BD%AE)
  - [查看位置](#%E6%9F%A5%E7%9C%8B%E4%BD%8D%E7%BD%AE)
  - [地图组件控制](#%E5%9C%B0%E5%9B%BE%E7%BB%84%E4%BB%B6%E6%8E%A7%E5%88%B6)

## 名词解释

### WGS84

WGS84: World Geodetic System 1984，是为 GPS 全球定位系统使用而建立的坐标系统。通过遍布世界的卫星观测站观测到的坐标建立，其初次 WGS84 的精度为 1-2m。

在 1994 年 1 月 2 号，通过 10 个观测站在 GPS 测量方法上改正，得到了 WGS84（G730），G 表示由 GPS 测量得到，730 表示为 GPS 时间第 730 个周。

1996 年，National Imagery and Mapping Agency (NIMA) 为美国国防部 (U.S.Departemt of Defense, DoD)做了一个新的坐标系统。这样实现了新的 WGS 版本：WGS（G873）。其因为加入了 USNO 站和北京站的改正，其东部方向加入了 31-39cm 的改正。所有的其他坐标都有在 1 分米之内的修正。

第三次精化：WGS84（G1150），于 2002 年 1 月 20 日启用。

### GCJ-02

GCJ-02 是由中国国家测绘局（G表示Guojia国家，C表示Cehui测绘，J表示Ju局）制订的地理信息系统的坐标系统。

它是一种对经纬度数据的加密算法，即加入随机的偏差。

国内出版的各种地图系统（包括电子形式），必须至少采用 GCJ-02 对地理位置进行首次加密。

### 火星坐标系统

一种国家保密插件，也叫做加密插件或者加偏或者 SM 模组，其实就是对真实坐标系统进行人为的加偏处理，按照特殊的算法，将真实的坐标加密成虚假的坐标，而这个加偏并不是线性的加偏，所以各地的偏移情况都会有所不同。而加密后的坐标也常被人称为**火星坐标系统**。

所有的电子地图、导航设备，都需要加入国家保密插件。第一步，地图公司测绘地图，测绘完成后，送到国家测绘局，将真实坐标的电子地图，加密成“火星坐标”，这样的地图才是可以出版和发布的，然后才可以让 GPS 公司处理。第二步，所有的 GPS 公司，只要需要汽车导航的，需要用到导航电子地图的，都需要在软件中加入国家保密算法，将 COM 口读出来的真实的坐标信号，加密转换成国家要求的保密的坐标。这样，GPS 导航仪和导航电子地图就可以完全匹配，GPS 也就可以正常工作了。

### BD09

使用百度地图的服务，需使用 BD09 坐标。

若使用非 BD09 坐标、未经过坐标转换（非 BD09 转成 BD09 ）直接叠加在地图上，地图展示位置会偏移，因此通过其他坐标（WGS84、GCJ02）调用服务时，需先将其他坐标转换为 BD09 。

### 小结

- `WGS84`：为一种大地坐标系，也是目前广泛使用的 GPS 全球卫星定位系统使用的坐标系。
- `GCJ02`：又称火星坐标系，是由中国国家测绘局制定的地理坐标系统，是由 WGS84 加密后得到的坐标系。
- `BD09`：为百度坐标系，在 GCJ02 坐标系基础上再次加密。其中 `bd09ll` 表示百度经纬度坐标，`bd09mc` 表示百度墨卡托米制坐标。

## 组件

具体参数详见[地图组件文档][component]。常用的属性有：

```
id: String 用于指定地图 ID
show-location: Boolean 显示带有方向的当前定位点
markers: Array 标记点
```

`map` 组件使用的经纬度是火星坐标系。从百度地图点击的坐标点有偏移。

### markers

标记点用于在地图上显示标记的位置

其属性包括：

```js
id: Number,
latitude: Number,
longitude: Number,
title: String,
iconPath: String,
rotate: Number,
// 标注的透明度，范围 0~1
alpha: Number
width: Number,
height: Number,
// 自定义标记点上方的气泡窗口
callout: Object = { content, color, fontSize, borderRadius, bgColor, padding, display, textAlign },
// 为标记点旁边增加标签
label: Object = { content, color, fontSize, x, y, borderWidth, borderColor, borderRadius, bgColor, padding, textAlign },
// 经纬度在标注图标的锚点，默认底边中点
anchor: Object,
```

- `callout.display` 'BYCLICK' 点击显示；'ALWAYS' 常显
- `callout.textAlign` 文本对齐方式，left, right, center

## API

### 获取位置

**wx.getLocation**

获取当前的地理位置、速度。

```js
wx.getLocation({
  type: String,
  altitude: Boolean,
  success: function (res) {
    const { 
      latitude,             // 纬度，浮点数，范围 -90~90
      longitude,            // 经度，浮点数，范围 -180~180
      speed,                // 速度，浮点数，单位 m/s
      accuracy,             // 位置的精确度
      altitude,             // 高度，单位 m
      verticalAccuracy,     // 垂直精度，单位 m（Android 无法获取，返回 0）
      horizontalAccuracy    // 水平精度，单位 m
    } = res
  },
  fail: function (res) {},
  complete: function (res) {}
})
```

- `type` 默认为 `wgs84` 返回 gps 坐标，`gcj02` 返回可用于 `wx.openLocation` 的坐标
- `altitude` 传入 `true` 会返回高度信息，由于获取高度需要较高精确度，会减慢接口返回速度。默认为 `false`

**wx.chooseLocation**

打开地图选择位置。需要用户授权 `scope.userLocation`。

```js
wx.chooseLocation({
  success: function (res) {
    const {
      name,     // 位置名称
      address,  // 详细地址
      latitude, // 纬度
      longitude // 经度
    } = res
  },
  fail: function (res) {},
  complete: function (res) {}
})
```

### 查看位置

**wx.openLocation**

​使用微信内置地图查看位置。

```js
wx.openLocation({
  latitude: Float,
  longitude: Float,
  scale: int,
  name: String,
  address: String,
  success: function(res) {},
  fail: function(res) {},
  complete: function(res) {}
})
```

- `scale` 缩放比例，范围 5~18，默认为 18。

### 地图组件控制

**wx.createMapContext**

创建并返回 map 上下文 `mapContext` 对象。在自定义组件下，第二个参数传入组件实例 `this`，以操作组件内 `<map/>` 组件。

mapContext

`mapContext` 通过 `mapId` 跟一个 `<map/>` 组件绑定，通过它可以操作对应的 `<map/>` 组件。它的方法列表如下：

```js
// 获取当前地图中心的经纬度，返回的是 gcj02 坐标系
getCenterLocation({
  success(res) {
    const { longitude, latitude } = res
  }
})

// 将地图中心移动到当前定位点，需要配合 map 组件的 show-location 使用
moveToLocation()

// 平移 marker，带动画
translateMarker({
  markerId: Number,
  destination: Object,
  autoRotate: Boolean,
  rotate: Number,
  // 动画持续时长，默认值 1000ms，平移与旋转分别计算
  duration: Number,
  // 动画结束回调函数
  animationEnd: function(res) {},
  fail: function(res) {}
})

// 缩放视野展示所有经纬度
includePoints({
  points: [{ latitude, longitude }],
  padding: [top, right, bottom, left]
})

// 获取当前地图的视野范围
getRegion({
  success(res) {
    // 西南角与东北角的经纬度
    const { southwest, northeast } = res
  }
})

// 获取当前地图的缩放级别
getScale({
  success(res) {
    const { scale } = res
  }
})
```

## REF

- [WGS84 - 百度百科][wgs84]
- [火星坐标系统 - 百度百科][mars]
- [GCJ-02 - 百度百科][gcj02]
- [地图组件][component]
- [获取位置 - 小程序][location]
- [地图组件控制][api.map]
- [为何您的坐标不准？如何纠偏？][cnblog], by *酸奶小妹*
- [WGS84 坐标转火星坐标（iOS 篇）][keakon], by 孙竞, 2011/07/02
- [百度坐标（BD09）、国测局坐标（火星坐标，GCJ02）、和WGS84坐标系互转][cnodejs], by *wandergis*
- [坐标系说明书 - 百度地图开放平台][baidu.coordinate]

[wgs84]: https://baike.baidu.com/item/WGS84/4380144
[gcj02]: https://baike.baidu.com/item/GCJ-02/1913612
[mars]: https://baike.baidu.com/item/%E7%81%AB%E6%98%9F%E5%9D%90%E6%A0%87%E7%B3%BB%E7%BB%9F/6734069
[component]: https://developers.weixin.qq.com/miniprogram/dev/component/map.html#map
[location]: https://developers.weixin.qq.com/miniprogram/dev/api/location.html
[api.map]: https://developers.weixin.qq.com/miniprogram/dev/api/api-map.html
[cnblog]: http://www.cnblogs.com/milkmap/p/3627940.html
[keakon]: https://www.keakon.net/2011/07/02/WGS84%E5%9D%90%E6%A0%87%E8%BD%AC%E7%81%AB%E6%98%9F%E5%9D%90%E6%A0%87%EF%BC%88iOS%E7%AF%87%EF%BC%89
[cnodejs]: https://cnodejs.org/topic/564c0a27e4766d487f6fe38d
[baidu.coordinate]: http://lbsyun.baidu.com/index.php?title=coordinate