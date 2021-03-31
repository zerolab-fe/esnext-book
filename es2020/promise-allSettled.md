# Promise.allSettled

## 简介

如果等待多个 `promise` 返回结果，我们可以用 `Promise.all([promise_1, promise_2])`。但是该方法如果其中一个请求失败了，就会抛出错误。有时候我们希望某个请求失败后，其他请求的结果能够正常返回。针对这种情况 ES2020 引入了 `Promise.allSettled()` 方法 。

## 语法

`Promise.allSettled()`方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。只有等到所有这些参数实例都返回结果，不管是 `fulfilled` 还是 `rejected`，包装实例才会结束。

```javascript
const promise1 = Promise.resolve(3)
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 100, 'foo'))
const promises = [promise1, promise2]

Promise.allSettled(promises).then((results) => results.forEach((result) => console.log(result)))

// Object { status: "fulfilled", value: 3 }
// Object { status: "rejected", reason: "foo" }
```

该方法返回的新的 Promise 实例，一旦结束，状态总是 `fulfilled`，不会变成 `rejected`。状态变成 `fulfilled` 后，Promise 的监听函数接收到的参数是一个数组，该数组的每个成员都是一个对象，对应传入 `Promise.allSettled()`的两个 Promise 实例。每个对象都有 `status` 属性，该属性的值只可能是字符串 `fulfilled` 或字符串 `rejected`。`fulfilled` 时，对象有 `value` 属性，`rejected` 时有 `reason` 属性，对应两种状态的返回值。

有时候，我们不关心异步操作的结果，只关心这些操作有没有结束。这时，`Promise.allSettled()`方法就很有用。
