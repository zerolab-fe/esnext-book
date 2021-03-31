# 正则表达式具名捕获组

正则表达式使用`()`进行组匹配。

```javascript
const RE_DATE = /(\d{4})-(\d{2})-(\d{2})/

const matchObj = RE_DATE.exec('1999-12-31')
const year = matchObj[1] // 1999
const month = matchObj[2] // 12
const day = matchObj[3] // 31
```

组匹配的一个问题是，每一组的匹配含义不容易看出来，而且只能用索引取值，如果组的顺序变了，引用也需要修改。

ES2018 引入了具名捕获组（Named Capture Groups），允许为每一个组匹配指定一个名字。使用方式为 `(?<Name>x)`，尖括号(`<` 和 `>`) 用于组名，`<Name>` 为指定的属性名，“x”是要匹配的字符串。

```javascript
let users = `姓氏: 李, 名字: 雷
姓氏: 韩, 名字: 梅梅`

let regexpNames = /姓氏: (?<first>.+), 名字: (?<last>.+)/gm
let match = regexpNames.exec(users)

do {
  console.log(`Hello ${match.groups.first} ${match.groups.last}`)
} while ((match = regexpNames.exec(users)) !== null)

// Hello 李 雷
// Hello 韩 梅梅
```

如果具名组没有匹配，那么对应的 `groups` 对象属性会是 `undefined`。

```javascript
const RE_OPT_A = /^(?<as>a+)?$/
const matchObj = RE_OPT_A.exec('')

matchObj.groups.as // undefined
matchObj.groups // {as: undefined}
'as' in matchObj.groups // true
```
