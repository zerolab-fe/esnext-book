# JSON 超集

之前如果 JSON 字符串中包含有行分隔符(`\u2028`) 和段落分隔符(`\u2029`)，那么在解析过程中会报错。

```javascript
JSON.parse('"\u2028"')
// SyntaxError
```

现在 ES2019 对它们提供了支持。

```javascript
JSON.parse('"\u2028"')
// ''
```
