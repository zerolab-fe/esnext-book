# 非转义序列的模板字符串的修订

带标签的模版字符串应该允许嵌套其他语言（例如 DSLs、LaTeX）。但是，模板字符串默认会将字符串转义，导致无法嵌入其他语言。

```javascript
function latex(strings) {
  // ...
}

let document = latex`
\newcommand{\fun}{\textbf{Fun!}}  // 正常工作
\newcommand{\unicode}{\textbf{Unicode!}} // 报错
\newcommand{\xerxes}{\textbf{King!}} // 报错

Breve over the h goes \u{h}ere // 报错
`
```

上面代码中，变量 `document` 内嵌的模板字符串，对于 LaTEX 语言来说完全是合法的，但是 JavaScript 引擎会报错。原因就在于字符串的转义。

模板字符串会将`\u00FF` 和`\u{42}`当作 Unicode 字符进行转义，所以`\unicode` 解析时报错；而`\x56` 会被当作十六进制字符串转义，所以`\xerxes` 会报错。也就是说，`\u` 和`\x` 在 LaTEX 里面有特殊含义，但是 JavaScript 将它们转义了。

为了解决这个问题，ES2018 放松了对标签模板里面的字符串转义的限制。如果遇到不合法的字符串转义，就返回 `undefined`，而不是报错，并且从 `raw` 属性上面可以得到原始字符串。

```javascript
function tag(strs) {
  strs[0] === undefined
  strs.raw[0] === '\\unicode and \\u{55}'
}
tag`\unicode and \u{55}`
```

上面代码中，模板字符串原本是应该报错的，但是由于放松了对字符串转义的限制，所以不报错了，JavaScript 引擎将第一个字符设置为 `undefined`，但是 `raw` 属性依然可以得到原始字符串，因此 `tag` 函数还是可以对原字符串进行处理。

注意，这种对字符串转义的放松，只在标签模板解析字符串时生效，不是标签模板的场合，依然会报错。

```javascript
let bad = `bad escape sequence: \unicode` // 报错
```
