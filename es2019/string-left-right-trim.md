# String.prototype.trimStart / String.prototype.trimEnd

ES2019 对字符串实例新增了 `trimStart()`和 `trimEnd()`这两个方法。它们的行为与 `trim()`一致，`trimStart()`消除字符串头部的空格，`trimEnd()`消除尾部的空格。它们返回的都是新字符串，不会修改原始字符串。

```javascript
const s = '  abc  '

s.trim() // "abc"
s.trimStart() // "abc  "
s.trimEnd() // "  abc"
```

上面代码中，`trimStart()`只消除头部的空格，保留尾部的空格。`trimEnd()`也是类似行为。

除了空格键，这两个方法对字符串头部（或尾部）的 tab 键、换行符等不可见的空白符号也有效。

为了与 `String.prototype.padStart` 等函数保持一致，标准方法名称为` trimStart`。 但是，出于 Web 兼容性原因，浏览器还部署了额外的两个方法，`trimLeft()`是 `trimStart()`的别名，`trimRight()`是 `trimEnd()`的别名。
