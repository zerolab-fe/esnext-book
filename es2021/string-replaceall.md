# String.prototype.replaceAll()

## 简介

在 ES2021 之前，要替换掉一个字符串中的所有指定字符，不得不使用正则表达式的 `g` 修饰符。

```javascript
'aabbcc'.replace(/b/g, '_');
// 'aa__cc'
```

ES2021 可以用 `String.prototype.replaceAll()`替换全部字符串而不需要使用正则。

```javascript
'aabbcc'.replaceAll('b', '_');
// 'aa__cc'
```

## 语法

> const newStr = str.replaceAll(regexp|substr, newSubstr|function)

### 参数

**regexp(pattern)**

具有 `g` 修饰符的正则表达式。如果没有 `g` 修饰符，将抛出错误。 TypeError: "replaceAll must be called with a global RegExp".

**substr**

需要匹配的字符串。

**newSubstr(replacement)**

`regexp` 或 `substr` 要替换的字符串。其中可以使用一些特殊字符串：

`$&`：匹配的子字符串。  
`` $` ``：匹配结果前面的文本。  
`$'`：匹配结果后面的文本。  
`$n`：匹配成功的第 n 组内容，n 是从 1 开始的自然数。这个参数生效的前提是，第一个参数必须是正则表达式。  
`$$`：指代美元符号$。

**function(replacement)**

`replaceAll()`的第二个参数也可以是一个函数，该函数的返回值将替换掉第一个参数 `searchValue` 匹配的文本。

该函数可以接受多个参数。第一个参数是捕捉到的匹配内容，第二个参数捕捉到是组匹配（有多少个组匹配，就有多少个对应的参数）。倒数第二个参数是捕捉到的内容在整个字符串中的位置，最后一个参数是原字符串。

```javascript
const str = '123abc456';
const regex = /(\d+)([a-z]+)(\d+)/g;

function replacer(match, p1, p2, p3, offset, string) {
  return [p1, p2, p3].join(' - ');
}

str.replaceAll(regex, replacer);
// 123 - abc - 456
```

### 返回值

它的用法与 `replace()`相同，返回一个新字符串，不会改变原字符串。

## 示例

### 非全局 regex 抛出

```javascript
'aabbcc'.replaceAll(/b/, '.');
// TypeError: replaceAll must be called with a global RegExp
```

正确用法：

```javascript
'aabbcc'.replaceAll(/b/g, '.');
// "aa..cc"
```
