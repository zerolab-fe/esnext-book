# 正则表达式 Unicode 转义

在正则表达式中，我们通常使用 `\d` 匹配任何数字，`\s` 匹配任何不为空格的字符。

在 ES2018 添加了 Unicode 属性转义，用 `\p{}` 匹配所有 Unicode 字符，否定为 `\P{}`。

```javascript
/^\p{ASCII_Hex_Digit}+$/u.test('0123456789ABCDEF') // true
/^\p{ASCII_Hex_Digit}+$/u.test('h')                // false
```

上面代码中，`ASCII_Hex_Digit` 是一个布尔属性，用于检查字符串是否仅包含有效的十六进制数字。

还有许多其他布尔属性，包括 `Uppercase`, `Lowercase`, `White_Space`, `Alphabetic`, `Emoji` 等。

```javascript
/^\p{Lowercase}$/u.test('h') // true
/^\p{Uppercase}$/u.test('H') // true

/^\p{Emoji}+$/u.test('H')   // false
/^\p{Emoji}+$/u.test('🙃🙃') // true
```

除了这些二进制属性之外，还可以检查任何 Unicode 字符属性以匹配特定值。

```javascript
/^\p{Script=Greek}+$/u.test('ελληνικ?') // true
/^\p{Script=Latin}+$/u.test('hey') // true
```

在这个例子中，检查字符串是用希腊语还是拉丁字母写的。

更多属性支持在[这里查看](https://github.com/tc39/proposal-regexp-unicode-property-escapes)
