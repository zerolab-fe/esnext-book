# Object.fromEntries()

## 简介

`Object.fromEntries()`方法把键值对列表转换为一个对象，是 `Object.entries()`的逆操作。

```javascript
Object.fromEntries([
  ['foo', 'bar'],
  ['baz', 42],
])
// { foo: "bar", baz: 42 }
```

## 语法

> Object.fromEntries(iterable);

**iterable**  
类似 `Array`、`Map` 或者其它实现了可迭代协议的可迭代对象。

## 示例

### Map 转化为 Object

```javascript
const map = new Map([
  ['foo', 'bar'],
  ['baz', 42],
])
const obj = Object.fromEntries(map)
console.log(obj) // { foo: "bar", baz: 42 }
```

### Array 转化为 Object

```javascript
const arr = [
  ['0', 'a'],
  ['1', 'b'],
  ['2', 'c'],
]
const obj = Object.fromEntries(arr)
console.log(obj) // { 0: "a", 1: "b", 2: "c" }
```

### 对象转换

`Object.fromEntries` 是与 `Object.entries()`相反的方法，用数组处理函数可以像下面这样转换对象：

```javascript
const object1 = { a: 1, b: 2, c: 3 }

const object2 = Object.fromEntries(Object.entries(object1).map(([key, val]) => [key, val * 2]))

console.log(object2)
// { a: 2, b: 4, c: 6 }
```

### 配合 URLSearchParams 对象，将查询字符串转为对象

```javascript
Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'))
// { foo: "bar", baz: "qux" }
```
