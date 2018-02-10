# 录音管理

**wx.getRecorderManager()**

> 基础库 1.6.0 开始支持，低版本需做兼容处理

获取全局唯一的录音管理器 `recorderManager`。

recorderManager 对象的方法列表：

| 方法 | 参数 | 说明 |
| --- | --- | --- |
| start | options | 开始录音 |
| pause | | 暂停录音 |
| resume | | 继续录音 |
| stop | | 停止录音 |
| onStart | callback | 录音开始事件 |
| onPause | callback | 暂停录音事件 |
| onResume | callback | 继续录音事件 |
| onStop | callback | 录音停止事件，会回调文件地址 |
| onFrameRecorded | callback | 已录制完指定帧大小的文件，会回调录音分片结果数据。如果设置了 frameSize ，则会回调此事件 |
| onError | callback | 暂停录音 |

start(options) 说明：

```js
/** 
* 指定录音的时长，单位 ms ，
* 最大值 600000（10 分钟）,默认值 60000（1 分钟） 
*/
duration: number,
/** 采样率，有效值 8000/16000/44100 */
sampleRate: number,
/** 录音通道数，有效值 1/2 */
numberOfChannels: number,
/** 编码码率，有效值见下表格 */
encodeBitRate: number,
/** 音频格式，有效值 aac/mp3 */
format: string,
/** 
* 指定帧大小，单位 KB。
* 传入 frameSize 后，每录制指定帧大小的内容后，会回调录制的文件内容，不指定则不会回调。
* 暂仅支持 mp3 格式。 
*/
frameSize: number,
```

其中，采样率和码率有一定要求，具体有效值如下：

```
采样率   编码码率
8000	  16000 ~ 48000
11025	  16000 ~ 48000
12000	  24000 ~ 64000
16000	  24000 ~ 96000
22050	  32000 ~ 128000
24000	  32000 ~ 128000
32000	  48000 ~ 192000
44100	  64000 ~ 320000
48000	  64000 ~ 320000
```

`onStop(callback)` 回调结果说明：

```js
/** 录音文件的临时路径 */
tempFilePath:string
```

`onFrameRecorded(callback)` 回调结果说明：

```js
/** 录音分片结果数据 */
frameBuffer:ArrayBuffer	
/** 当前帧是否正常录音结束前的最后一帧 */
isLastFrame:boolean
```

`onError(callback)` 回调结果说明：

```js
/** 错误信息 */
errMsg:string
```

示例代码：

```js
const recorderManager = wx.getRecorderManager()

recorderManager.onStart(() => {
  console.log('recorder start')
})

recorderManager.onResume(() => {
  console.log('recorder resume')
})

recorderManager.onPause(() => {
  console.log('recorder pause')
})

recorderManager.onStop((res) => {
  console.log('recorder stop', res)
  const { tempFilePath } = res
})

recorderManager.onFrameRecorded((res) => {
  const { frameBuffer } = res
  console.log('frameBuffer.byteLength', frameBuffer.byteLength)
})

const options = {
  duration: 10000,
  sampleRate: 44100,
  numberOfChannels: 1,
  encodeBitRate: 192000,
  format: 'aac',
  frameSize: 50
}

recorderManager.start(options)
```

## REF

- [录音管理 - 小程序][api]

[api]: https://mp.weixin.qq.com/debug/wxadoc/dev/api/getRecorderManager.html