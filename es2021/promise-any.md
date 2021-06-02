# Promise.any()

## 简介

ES2021 引入了 `Promise.any()`方法。该方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例返回。只要参数实例有一个变成 `fulfilled` 状态，就返回那个已经成功的 Promise，如果所有参数实例都变成 `rejected` 状态，包装实例就会变成 `rejected` 状态。这个方法和 `Promise.all()`是相反的。

这个方法用于返回第一个成功的 Promise 实例。只要有一个 promise 成功此方法就会终止，它不会等待其他的 promise 全部完成。

## 示例

### First to fulfil

```javascript
const pErr = new Promise((resolve, reject) => {
  reject('总是失败');
});

const pSlow = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, '最终完成');
});

const pFast = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, '很快完成');
});

Promise.any([pErr, pSlow, pFast]).then((value) => {
  console.log(value);
  // pFast fulfils first
});
// "很快完成"
```

### Rejections with AggregateError

如果没有 fulfilled (成功的) promise，`Promise.any()` 返回 `AggregateError` 错误。

```javascript
const pErr = new Promise((resolve, reject) => {
  reject('总是失败');
});

Promise.any([pErr]).catch((err) => {
  console.log(err);
});
// "AggregateError: No Promise in Promise.any was resolved"
```

## 总结

| 语法               | 描述                                                                                                      | 阶段   |
| ------------------ | --------------------------------------------------------------------------------------------------------- | ------ |
| Promise.all        | 成功：返回结果数据\n 失败：返回最先 reject 的值                                                           | ES2015 |
| Promise.race       | 返回最先有结果的 Promise 实例，不管成功失败                                                               | ES2015 |
| Promise.allSettled | 只有等到所有这些参数实例都返回结果，不管是 `fulfilled` 还是 `rejected`，包装实例才会结束。                | ES2020 |
| Promise.any        | 返回第一个成功的 Promise 实例。只要有一个 promise 成功此方法就会终止，它不会等待其他的 promise 全部完成。 | ES2021 |
