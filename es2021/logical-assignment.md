# 逻辑赋值运算符

## 简介

逻辑赋值运算符结合**逻辑运算符**和**赋值运算符**，它让代码变得简短、让变量和对象属性条件赋值变得简单。ES2021 将推出三个新的逻辑操作符，每一个操作符都有两个运算元。

## 语法

expr1 ||= expr2

expr1 &&= expr2

expr1 ??= expr2

```javascript
a ||= b;
// 等效于以下两种写法
// a || (a = b); a为falsy时执行右边
// if(!a) a = b;

a &&= b;
// 等效于以下两种写法
// a && (a = b); a为truth时执行右边
// if(a) a = b

a ??= b;
// 等效于以下两种写法
// a ?? (a = b); a为undefined或null时执行右边
// if(a === nudefined || a === null) a = b;
```

## 示例

```javascript
let obj = {};

obj.x ??= 0;
console.log(obj.x); // 0

obj.x ||= 1;
console.log(obj.x); // 1

obj.x &&= 2;
console.log(obj.x); // 2
```

## 注意事项

```javascript
a = a || b; // 与 a ||= b 不等价
a = a && b; // 与 a &&= b 不等价
a = a ?? b; // 与 a ??= b 不等价
```

不等价的原因在于，无论上面的每个逻辑表达式结果如何，都会进行赋值运算；而逻辑赋值运算符只会在条件成立的情况下进行赋值
