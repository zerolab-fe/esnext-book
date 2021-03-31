# 异步迭代器

## for await...of

在 `async/await` 的某些时刻，你可能尝试在同步循环中调用异步函数。例如：

```javascript
async function process(array) {
  for (let i of array) {
    await doSomething(i)
  }
}
```

ES2018 引入异步迭代器（asynchronous iterators），`await` 可以和 `for...of` 循环一起使用，以串行的方式运行异步操作。

```javascript
async function process(array) {
  for await (let i of array) {
    doSomething(i)
  }
}
```

`for await...of` 循环也可以用于同步遍历器。

```javascript
;(async function () {
  for await (const x of ['a', 'b']) {
    console.log(x)
  }
})()
// a
// b
```
