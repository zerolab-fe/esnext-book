# 可选的 catch 参数

JavaScript 语言的 `try...catch` 结构，以前明确要求 `catch` 命令后面必须跟参数，接受 `try` 代码块抛出的错误对象。

```javascript
try {
  // ...
} catch (err) {
  // 处理错误
}
```

上面代码中，`catch` 命令后面带有参数 `err`。

很多时候，`catch` 代码块可能用不到这个参数。但是，为了保证语法正确必须写。ES2019 允许 `catch` 语句省略参数。

```javascript
try {
  // ...
} catch {
  // ...
}
```
