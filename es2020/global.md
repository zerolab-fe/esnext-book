# globalThis 对象

## 简介

JavaScript 语言存在一个顶层对象，它提供全局环境（即全局作用域），所有代码都是在这个环境中运行。但是，顶层对象在各种实现里面是不统一的。在 Web 中，可以通过 `window`、`self` 或者 `frames` 取到全局对象，但是在 Web Workers 中，只有 `self` 可以。在 Node.js 中，它们都无法获取，必须使用 `global`。

同一段代码为了能够在各种环境，都能取到顶层对象，现在一般是使用 `this` 变量，但是有局限性。

- 全局环境中，`this` 会返回顶层对象。但是，Node.js 模块中 `this` 返回的是当前模块，ES6 模块中 `this` 返回的是 `undefined`。
- 函数里面的 `this`，如果函数不是作为对象的方法运行，而是单纯作为函数运行，`this` 会指向顶层对象。但是，严格模式下，这时 `this` 会返回 `undefined`。
- 不管是严格模式，还是普通模式，`new Function('return this')()`，总是会返回全局对象。虽然可以通过此方式获取全局对象，但是，如果浏览器用了 CSP（Content Security Policy，内容安全策略），那么 `eval`、`new Function` 这些方法都可能无法使用。

ES2020 引入了 `globalThis` 作为顶层对象。也就是说，在任何环境下，都可以通过 `globalThis` 来拿到顶层对象。

## 示例

在 `globalThis` 出现之前

```javascript
var getGlobal = function () {
  if (typeof self !== 'undefined') {
    return self
  }
  if (typeof window !== 'undefined') {
    return window
  }
  if (typeof global !== 'undefined') {
    return global
  }
  throw new Error('unable to locate global object')
}

var globals = getGlobal()

if (typeof globals.setTimeout !== 'function') {
  // 此环境中没有 setTimeout 方法！
}
```

有了 `globalThis` 之后

```javascript
if (typeof globalThis.setTimeout !== 'function') {
  //  此环境中没有 setTimeout 方法！
}
```
