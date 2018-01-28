# setTimeout

符合 HTML 规范的 `setTimeout` ，可以接收第三个可选参数，用来当作回调函数的实参。但是微信小程序的基础库版本小于 `v1.7.4` 是没有的。

举个例子：

```js
setTimeout(res => console.log(res), 100, 'hello')
```

把上述代码在现代浏览器中执行，会输出 `hello`。在微信开发者工具中执行，如果将基础库版本设定为 `v1.7.4` 或更早版本，将输出 `undefined`。

> 若基础库版本选择 `v1.9.0` 或更新，则行为与现代浏览器保持一致。

所以，为了兼容性，尽可能不使用第三个参数。

## REF

- [setTimeout - MDN][mdn]

[mdn]: https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout