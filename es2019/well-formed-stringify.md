# 更友好的 JSON.stringify

更友好的 `JSON.stringify`，对于一些超出范围的 `Unicode` 字符串，为其输出转义序列，使其成为有效 `Unicode`。

```javascript
// Before
console.log(JSON.stringify('\uD800')) // "�"

// ES2019
console.log(JSON.stringify('\uD800')) // "\ud800"
```
