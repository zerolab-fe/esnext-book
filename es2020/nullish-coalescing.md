# 空值合并操作符

## 简介

当读取对象属性的时候，如果某个属性的值是 `null` 或 `undefined`，有时候需要为它们指定默认值。常见做法是通过`||`运算符指定默认值。

```javascript
const text = settings.headerText || 'Hello, world!'
```

开发者的目的是，属性的值为 `null` 或 `undefined`，默认值生效，但是属性的值如果为空字符串或 `false` 或 `0`，默认值也会生效。

为了避免这种情况，ES2020 引入了**空值合并操作符**`??`。空值合并操作符是一个**逻辑操作符**，当左侧的操作数为 `null` 或者 `undefined` 时，返回其右侧操作数，否则返回左侧操作数。

## 语法

### 使用空值合并操作符

在这个例子中，使用空值合并操作符为常量提供默认值，保证常量不为 `null` 或者 `undefined`。

```javascript
const nullValue = null
const emptyText = '' // 空字符串，是一个假值，Boolean("") === false
const someNumber = 42

const valA = nullValue ?? 'valA 的默认值'
const valB = emptyText ?? 'valB 的默认值'
const valC = someNumber ?? 0

console.log(valA) // "valA 的默认值"
console.log(valB) // ""（空字符串虽然是假值，但不是 null 或者 undefined）
console.log(valC) // 42
```

### 短路

与 OR 和 AND 逻辑操作符相似，当左表达式不为 `null` 或 `undefined` 时，不会对右表达式进行求值。

```javascript
function foo() {
  console.log('函数 foo 被调用了')
}

console.log(undefined ?? foo())
// "函数 foo 被调用了"

console.log(false ?? foo())
// 右侧表达式没有被执行
```

### 与 AND 或 OR 操作符共用，需要带 ()

```javascript
// 报错
lhs && middle ?? rhs
lhs ?? middle && rhs
lhs || middle ?? rhs
lhs ?? middle || rhs
```

加入括号表示优先级

```javascript
;(lhs && middle) ?? rhs
lhs && (middle ?? rhs)(lhs ?? middle) && rhs
lhs ?? (middle && rhs)(lhs || middle) ?? rhs
lhs || (middle ?? rhs)(lhs ?? middle) || rhs
lhs ?? (middle || rhs)
```
