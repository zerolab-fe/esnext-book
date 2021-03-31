# Async / Await

## 简介

在 ES2015 中引入了 Promise 来解决“回调地域”的问题。时隔两年，ES2017 Async Functions 的出现，证明 Promise 不是最终的解决方案。

ES2017 标准引入了 Async Functions 的概念，使得异步操作变得更加方便。Async Functions 是 Generator 函数的语法糖，直观上看，`async` 函数就是将 Generator 函数的星号（`*`）替换成 `async`，将 `yield` 替换成 `await`，仅此而已。

`async` 和 `await`，比起星号和 `yield`，语义更加清晰，更符合语义。并且 `async` 函数自带执行器，不需要像 Generator 函数一样，需要调用 `next` 方法，或者用 [co 模块](https://github.com/tj/co)。`async` 函数的返回值是 Promise 对象，其内部可以看做是多个异步操作，包装成的一个 Promise 对象，而 `await` 命令是内部 `then` 命令的语法糖。

## 语法

### 返回 Promise 对象

`async` 函数返回一个 Promise 对象，如果代码中返回的是非 Promise 对象，JavaScript 会自动将其包装成 `Promise.resolved()` 的值。

```javascript
async function test() {
  return 'hello world'
}

test().then((v) => console.log(v))
// "hello world"
```

也可以显示的返回一个 Promise，结果与上面例子相同。

```javascript
async function test() {
  return Promise.resolve('hello world')
}

test().then((v) => console.log(v))
// "hello world"
```

如果 `async` 函数内部抛出错误，会导致返回的 Promise 对象变为 `reject` 状态。抛出的错误会被 `catch` 方法捕获到。

```javascript
async function test() {
  throw new Error('出错了')
}

test().then(
  (v) => console.log('resolve', v),
  (e) => console.log('reject', e),
)
//reject Error: 出错了
```

### await 命令

`await` 命令只能在 `async` 函数内部使用。

一般情况下， `await` 命令后面是一个 Promise 对象，返回该对象的结果。如果不是，就直接返回对应的值。

```javascript
async function test() {
  return await 'hello world'
  // 等同于
  // return 'hello world'
}

test().then((v) => console.log(v))
// "hello world"
```

另一种情况是， `await` 接收一个 `thenable` 对象（具有可调用 `then` 方法的对象），那么 `await` 会将其等同于 Promise 对象。

```javascript
class Thenable {
  constructor(num) {
    this.num = num
  }
  then(resolve, reject) {
    setTimeout(() => resolve(this.num * 2), 1000)
  }
}

;(async () => {
  const result = await new Thenable(2)
  console.log(result)
})()

// 4
```

上面例子中， `await` 得到了一个带有 `then` 方法的非 Promise 对象，它会调用提供原生函数的 `resolve`、`reject` 作为参数的方法。

### 错误处理

如果一个 Promise 正常 `resolve`，那么 `await` 会返回这个结果，但是在 `reject` 的情况下会抛出一个错误。

```javascript
async function test() {
  await Promise.reject(new Error('出错了！'))
}

test()
  .then((v) => console.log(v))
  .catch((e) => console.log(e))

// Error：出错了
```

注意，和下面写法效果一样。

```javascript
async function test() {
  throw new Error('出错了！')
}
```

任何一个 `await` 语句后面的 Promise 对象变为 `reject` 状态，那么整个 `async` 函数都会中断执行。

```javascript
async function test() {
  await Promise.reject(new Error('出错了'))
  await Promise.resolve('hello world') // 不会执行
}
```

有时，我们希望即使一个异步操作失败，也不会中断后面的异步操作。可以将前面的 `await` 放在 `try...catch` 语句中。

```javascript
async function test() {
  try {
    await Promise.reject(new Error('出错了'))
  } catch (e) {
    console.error(e) // Error: 出错了
  }
  return await Promise.resolve('hello world')
}

test().then((v) => console.log(v))
// hello world
```

如果发生了错误，会被 catch 捕获到。当然如果有多个 `await` 命令，可以统一放在 `try...catch` 语句中处理。

Promise 对象的状态变化
使用注意点

## 实例

### 按顺序完成异步操作

实际开发中，经常遇到一组异步操作，如一组网络请求，需要按顺序完成，然后按照顺序输出结果。

Promise 写法如下。

```javascript
function logInOrder(urls) {
  // 远程读取所有URL
  const textPromises = urls.map((url) => {
    return fetch(url).then((response) => response.text())
  })

  // 按次序输出
  textPromises.reduce((chain, textPromise) => {
    return chain.then(() => textPromise).then((text) => console.log(text))
  }, Promise.resolve())
}
```

上面代码使用 `fetch` 方法，请求一组 URL。每个 `fetch` 都会返回一个 Promise 对象，放入 `textPromises` 数组。然后，使用 `reduce` 方法依次处理每个 Promise 对象，然后使用 `then` 将所有 Promise 对象连接起来，依次输出结果。

下面用 `async` 函数改造。

```javascript
async function logInOrder(urls) {
  for (const url of urls) {
    const response = await fetch(url)
    console.log(await response.text())
  }
}
```

上面代码简洁了很多，问题是所有 `fetch` 操作都是继发，效率很差。并发改造如下。

```javascript
async function logInOrder(urls) {
  // 并发读取远程URL
  const textPromises = urls.map(async (url) => {
    const response = await fetch(url)
    return response.text()
  })

  // 按次序输出
  for (const textPromise of textPromises) {
    console.log(await textPromise)
  }
}
```

上面代码中，虽然 `map` 方法的参数是 `async` 函数，但它是并发执行的，因为只有 `async` 函数内部是继发执行，外部不受影响。后面的 `for..of` 循环内部使用了 `await`，因此实现了按顺序输出。

### 休眠效果

JavaScript 一直没有休眠的语法，但是借助 `await` 命令可以让程序停顿指定的时间。

```javascript
function sleep(interval) {
  return new Promise((resolve) => {
    setTimeout(resolve, interval)
  })
}

// 用法
async function log() {
  for (let i = 1; i <= 5; i++) {
    console.log(i)
    await sleep(1000)
  }
}
```

### 使用 `try...catch` 结构，实现多次重复尝试

在实际开发中，有时当由于网络等原因导致请求报错，我们期望请求能够重试指定的次数。

```javascript
const NUM_RETRIES = 3

async function test() {
  let i
  for (i = 0; i < NUM_RETRIES; ++i) {
    try {
      await fetch('http://google.com/this-throws-an-error')
      break
    } catch (err) {}
  }
  console.log(i) // 3
}

test()
```
