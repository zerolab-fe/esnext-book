# Symbol.prototype.description

## 简介

ES2019 提供了一个实例属性 `description`，直接返回 Symbol 的描述。`description` 是一个只读属性。

## 语法

> Symbol('myDescription').description;  
> Symbol.iterator.description;  
> Symbol.for('foo').description;

创建 Symbol 的时候，可以添加一个描述。

```javascript
const sym = Symbol('foo')
```

上面代码中，`sym` 的描述就是字符串 `foo`。

示例：

```javascript
Symbol('desc').toString() // "Symbol(desc)"
Symbol('desc').description // "desc"
Symbol('').description // ""
Symbol().description // undefined

// well-known symbols
Symbol.iterator.toString() // "Symbol(Symbol.iterator)"
Symbol.iterator.description // "Symbol.iterator"

// global symbols
Symbol.for('foo').toString() // "Symbol(foo)"
Symbol.for('foo').description // "foo"
```
