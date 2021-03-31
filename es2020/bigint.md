# BigInt

## 简介

JavaScript 所有数字都保存成 64 位浮点数，这给数值的表示带来了很大限制。一是数值的精度只能到 53 个二进制（相当于 16 个十进制位），大于这个范围的整数，JavaScript 是无法精确表示的，这使得 JavaScript 不适合进行科学和金融方面的精确计算。二是大于或等于 2 的 1024 次方的数值，JavaScript 无法表示，会返回 `Infinity`。

ES2020 引入了一种新的数据类型 BigInt（大整数），来解决这个问题，这是 ECMAScript 的第八种数据类型。BigInt 只用来表示整数，没有位数的限制，任何位数的整数都可以精确表示。

## 语法

### 定义一个 `BigInt`

可以用在一个整数字面量后面加 `n` 的方式定义一个 `BigInt`。

```javascript
const a = 9007199254740991n
```

调用函数 `BigInt()`。

```javascript
const b = BigInt(9007199254740991)
```

### 类型判断

使用 `typeof` 判断类型，`BigInt` 对象返回 "bigint"

```javascript
typeof 1n === 'bigint' // true
typeof BigInt('1') === 'bigint' // true
```

使用 `Object` 包装后， `BigInt` 被认为是一个普通 "object"

```javascript
typeof Object(1n) === 'object' // true
```

### 类型转换

可以使用 `Boolean()`、`Number()`和 `String()`这三个方法，将 BigInt 可以转为布尔值、数值和字符串类型。

```javascript
Boolean(0n) // false
Boolean(1n) // true
Number(1n) // 1
String(1n) // "1"
```

另外，取反运算符（`!`）也可以将 BigInt 转为布尔值。

```javascript
!0n // true
!1n // false
```

### 运算

以下操作符可以和 `BigInt` 一起使用： `+`、`*`、`-`、`**`、`%` 。除 `>>>`（无符号右移）之外的位操作也可以支持。因为 `BigInt` 都是有符号的， `>>>`（无符号右移）不能用于 `BigInt`。[为了兼容 asm.js](https://github.com/tc39/proposal-bigint/blob/master/ADVANCED.md#dont-break-asmjs)，`BigInt` 不支持单目 (`+`) 运算符。

除法运算 `/` 结果会向零取整，也就是说不会返回小数部分。

```javascript
const expected = 4n / 2n
// 2n

const rounded = 5n / 2n
// 2n, not 2.5n
```

BigInt 不能与普通数值进行混合运算。

```javascript
1n + 1.3 // 报错
```

上面代码报错是因为无论返回的是 BigInt 或 Number，都会导致丢失精度信息。比如 `(2n**53n + 1n) + 0.5` 这个表达式，如果返回 BigInt 类型，`0.5` 这个小数部分会丢失；如果返回 Number 类型，有效精度只能保持 53 位，导致精度下降。

同样的原因，如果一个标准库函数的参数预期是 Number 类型，但是得到的是一个 BigInt，就会报错。

```javascript
// 错误的写法
Math.sqrt(4n) // 报错

// 正确的写法
Math.sqrt(Number(4n)) // 2
```

上面代码中，`Math.sqrt` 的参数预期是 Number 类型，如果是 BigInt 就会报错，必须先用 `Number` 方法转一下类型，才能进行计算。

### 比较

`BigInt` 和 `Number` 不是严格相等的，但是宽松相等的。

```javascript
0n === 0
// false

0n == 0
// true
```

两者也可以混在一个数组内并排序。

```javascript
const mixed = [4n, 6, -12n, 10, 4, 0, 0n]
// [4n, 6, -12n, 10, 4, 0, 0n]

mixed.sort()
// [-12n, 0, 0n, 10, 4n, 4, 6]
```

### 条件

`BigInt` 转换成 `Boolean` 的时表现跟 `Number` 类似。

```javascript
if (0n) {
  console.log('Hello from the if!')
} else {
  console.log('Hello from the else!')
}
// "Hello from the else!"

0n || 12n
// 12n

0n && 12n
// 0n

Boolean(0n)
// false

Boolean(12n)
// true

!12n
// false

!0n
// true
```

### 静态方法

#### BigInt.asIntN()

将 BigInt 值转换为一个 -2<sup>width-1</sup> 与 2<sup>width-1</sup>-1 之间的有符号整数。

#### BigInt.asUintN()

将一个 BigInt 值转换为 0 与 2<sup>width</sup>-1 之间的无符号整数。

#### BigInt.parseInt()

近似于` Number.parseInt()`，将一个字符串转换成指定进制的 BigInt。

### 实例方法

BigInt 对象继承了 Object 对象的两个实例方法。

- `BigInt.prototype.toString()`
- `BigInt.prototype.valueOf()`

继承了 Number 对象的一个实例方法。

- `BigInt.prototype.toLocaleString()`
