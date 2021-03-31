# 函数参数列表结尾允许逗号

## 简介

ES2017 允许函数的最后一个参数有尾逗号（trailing comma）。此前，函数定义和调用时，都不允许最后一个参数后面出现逗号。

## 语法

```javascript
function clownPuppiesEverywhere(
  param1,
+ param2 // this line
) {
  /* ... */
}
clownPuppiesEverywhere(
  'foo',
+ 'bar' // this line
)
```

上面代码中，如果修改代码添加第三个参数，或者调整参数的次序，就要在原来最后一个参数的后面添加逗号，对于版本管理来说，就会显示添加逗号的那一行发生了变动。看上去又写冗余。

```javascript
function clownPuppiesEverywhere(
  param1,
  param2, // this line
) {
  /* ... */
}

clownPuppiesEverywhere(
  'foo',
  'bar', // this line
)
```

这样的规定也使得，函数参数与数组和对象的尾逗号规则，保持一致了。
