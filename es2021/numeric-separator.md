# 数值分隔符

## 简介

使用 `_` 对数字进行分割，提高数字的可读性，例如在日常生活中数字通常是每三位数字之间会用 `,` 分割，以方便人快速识别数字。

## 示例

```javascript
// 1000000000 不易辨识
const count1 = 1000000000;

// 1_000_000_000 很直观
const count2 = 1_000_000_000;

console.log(count1 === count2); // true
```
