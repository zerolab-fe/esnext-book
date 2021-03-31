# Array.prototype.includes()

## 简介

`includes()` 方法用来判断一个数组是否包含一个指定的值。如果包含则返回 `true`，否则返回 `false`。

## 语法

> arr.includes(valueToFind\[, fromIndex\])

**valueToFind**

需要查找的元素值。

**fromIndex**

可选，从 `fromIndex` 索引处开始查找 `valueToFind`。如果为负值，则按升序从 `array.length + fromIndex` 的索引开始搜 （即使从末尾开始往前跳 `fromIndex` 的绝对值个索引，然后往后搜寻）。默认为 0。

## 示例

### 基本使用

```javascript
[1, 2, 3].includes(2) // true
[1, 2, 3].includes(4) // false
[1, 2, 3].includes(3, 3) // false
[1, 2, 3].includes(3, -1) // true
[1, 2, NaN)].includes(NaN) // true
```

### fromIndex 大于等于数组长度

如果 `fromIndex` 大于等于数组的长度，则会返回 `false`，且该数组不会被搜索。

```javascript
var arr = ['a', 'b', 'c']

arr.includes('c', 3) // false
arr.includes('c', 100) // false
```

### 计算出的索引小于 0

如果 `fromIndex` 为负值，计算出的索引将作为开始搜索 `searchElement` 的位置。如果计算出的索引小于 0，则整个数组都会被搜索。

```javascript
// array length is 3
// fromIndex is -100
// computed index is 3 + (-100) = -97

var arr = ['a', 'b', 'c']

arr.includes('a', -100) // true
arr.includes('b', -100) // true
arr.includes('c', -100) // true
arr.includes('a', -2) // false
```
