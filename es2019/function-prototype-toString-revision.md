# Function.prototype.toString

ES2019 对函数实例的 `toString()`方法做出了修订。

`toString()` 方法返回当前函数源代码的字符串，以前会省略注释和空格。

```javascript
function /* foo comment */ foo() {}

foo.toString()
// function foo() {}
```

修改后的 `toString()`方法，返回一模一样的原始代码。

```javascript
function /* foo comment */ foo() {}

foo.toString()
// "function /* foo comment */ foo () {}"
```
