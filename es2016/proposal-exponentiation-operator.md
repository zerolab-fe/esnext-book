# 求幂运算符(\*\*)

## 语法

求幂运算符 `**` 等价于 `Math.pow()`，但是它被引入语言本身，而不是库函数。

```javascript
Math.pow(x, y) // 9

let squared = 3 ** 2 // 9

let num = 3
num **= 2
console.log(num) // 9
```
