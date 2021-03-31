# Promise.prototype.finally

`finally()` 方法返回一个 Promise。在 `promise` 结束时，无论结果是 `fulfilled` 或者是 `rejected`，都会执行指定的回调函数。这为在 Promise 是否成功完成后都需要执行的代码提供了一种方式。

```javascript
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

比如，服务器使用 Promise 处理请求，然后使用 `finally` 方法关掉服务器。

```javascript
server
  .listen(port)
  .then(function () {
    // ...
  })
  .finally(server.stop)
```

还有前端 loading 的场景控制。

```javascript
let isLoading = true

fetch(myRequest)
  .then(function (response) {
    var contentType = response.headers.get('content-type')
    if (contentType && contentType.includes('application/json')) {
      return response.json()
    }
    throw new TypeError("Oops, we haven't got JSON!")
  })
  .then(function (json) {
    /* process your JSON further */
  })
  .catch(function (error) {
    console.log(error)
  })
  .finally(function () {
    isLoading = false
  })
```
