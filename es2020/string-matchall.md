# String.prototype.matchAll

## 简介

ES2020 增加了 `String.prototype.matchAll()` 方法，可以一次性取出所有匹配。该方法返回遍历器，相对于数组，如果匹配结果是一个很大的数组，那么遍历器比较节省资源。

## 语法

> str.matchAll(regexp)

**参数**

`regexp`：正则表达式对象。如果所传参数不是一个正则表达式对象，则会隐式地使用 `new RegExp(obj)` 将其转换为一个 `RegExp`。`RegExp` 必须是设置了全局模式 `g` 的形式，否则会抛出异常 `TypeError`。

**返回值**

一个迭代器（不可重用，结果耗尽需要再次调用方法，获取一个新的迭代器）。

## 实例

### Regexp.exec() 和 matchAll()

在 `matchAll` 出现之前，通过在循环中调用 `regexp.exec()` 来获取所有匹配项信息（regexp 需使用 `/g` 标志）

```javascript
const regexp = RegExp('foo[a-z]*', 'g')
const str = 'table football, foosball'
let match

while ((match = regexp.exec(str)) !== null) {
  console.log(`Found ${match[0]} start=${match.index} end=${regexp.lastIndex}.`)
}
// expected output: "Found football start=6 end=14."
// expected output: "Found foosball start=16 end=24."
```

使用 `matchAll` 会得到一个迭代器的返回值，配合 `for...of`, array spread, 或者 `Array.from()` 可以更方便实现功能

```javascript
const regexp = RegExp('foo[a-z]*', 'g')
const str = 'table football, foosball'
const matches = str.matchAll(regexp)

Array.from(str.matchAll(regexp), (m) => m[0])
// Array [ "football", "foosball" ]
```

如果没有 `/g` 标志，`matchAll` 会抛出异常。

```javascript
const regexp = RegExp('[a-c]', '')
const str = 'abc'

Array.from(str.matchAll(regexp), (m) => m[0])
// TypeError: String.prototype.matchAll called with a non-global RegExp argument
```

### matchAll 相比 match 可以获取捕获组

当使用 `match()` 和 `/g` 标志方式获取匹配信息时，捕获组会被忽略

```javascript
var regexp = /t(e)(st(\d?))/g
var str = 'test1test2'

str.match(regexp)
// Array ['test1', 'test2']
```

使用 `matchAll` 可以通过如下方式获取分组捕获

```javascript
let array = [...str.matchAll(regexp)]

array[0]
// ['test1', 'e', 'st1', '1', index: 0, input: 'test1test2', length: 4]
array[1]
// ['test2', 'e', 'st2', '2', index: 5, input: 'test1test2', length: 4]
```
