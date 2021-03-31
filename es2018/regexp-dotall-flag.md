# 正则表达式 dotAll 模式

正则表达式中，点（`.`）是一个特殊字符，代表任意的单个字符，但是有两个例外。一个是四个字节的 UTF-16 字符，这个可以用 `u` 修饰符解决；另一个是行终止符（line terminator character）。

所谓行终止符，就是该字符表示一行的终结。以下四个字符属于“行终止符”。

- U+000A 换行符（`\n`）
- U+000D 回车符（`\r`）
- U+2028 行分隔符（line separator）
- U+2029 段分隔符（paragraph separator）

```javascript
;/foo.bar/.test('foo\nbar')
// false
```

上面代码中，因为`.`不匹配`\n`，所以正则表达式返回 `false`。

S2018 引入 `s` 修饰符，使得`.`可以匹配任意单个字符。

```javascript
;/foo.bar/s.test('foo\nbar') // true
```

这被称为 `dotAll` 模式，即点（dot）代表一切字符。

`dotAll` 是一个只读的属性，属于单个正则表达式实例。如果使用了 `s` 修饰符，`dotAll` 的值将返回 Boolean 类型的 `true`，否则将返回 `false`。

```javascript
const re = /foo.bar/s
// 另一种写法
// const re = new RegExp('foo.bar', 's');

re.test('foo\nbar') // true
re.dotAll // true
re.flags // 's'
```

`/s` 修饰符和多行修饰符`/m` 不冲突，两者一起使用的情况下，`.`匹配所有字符，而`^`和`$`匹配每一行的行首和行尾。
