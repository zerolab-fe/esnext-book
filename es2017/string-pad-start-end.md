# String padding

## 简介

在 String 对象中，ES2017 增加了两个新的函数，字符串补全长度功能。如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()` 用于头部补全，`padEnd()` 用于尾部补全。

## 语法

> str.padStart(targetLength \[, padString\])  
> str.padEnd(targetLength \[, padString\])

**参数：**

`targetLength`：当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。

`padString`：可选，填充字符串。此参数的默认值为 " "。

**返回值：**

在原字符串开头填充指定的填充字符串直到目标长度所形成的新字符串。

## 实例

原字符串的长度，等于或大于最大长度，则字符串补全不生效，返回原字符串。

```javascript
'xxx'.padStart(2, 'ab') // 'xxx'
'xxx'.padEnd(2, 'ab') // 'xxx'
```

如果用来补全的字符串与原字符串，两者的长度之和超过了最大长度，则会截去超出位数的补全字符串。

```javascript
'abc'.padStart(10, '0123456789')
// '0123456abc'
```

如果省略第二个参数，默认使用空格补全长度。

```javascript
'x'.padStart(4) // '   x'
'x'.padEnd(4) // 'x   '
```
