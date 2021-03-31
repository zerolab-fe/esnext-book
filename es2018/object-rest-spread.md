# 对象的结构赋值和拓展运算符

ES2015 引入了 Rest 参数和扩展运算符（`...`）。但是 ES2015 中的作用对象仅限于数组。

```javascript
restParam(1, 2, 3, 4, 5)

function restParam(p1, p2, ...p3) {
  // p1 = 1
  // p2 = 2
  // p3 = [3, 4, 5]
}

const values = [99, 100, -1, 48, 16]
console.log(Math.max(...values)) // 100
```

ES2018 为对象解构提供了和数组一样的 Rest 参数和展开操作符。

```javascript
const myObject = {
  a: 1,
  b: 2,
  c: 3,
}
const { a, ...x } = myObject
// a = 1
// x = { b: 2, c: 3 }

function foo({ a, ...param }) {
  console.log(a) // 1
  console.log(param) // {b: 2, c: 3}
}
```

由于解构赋值要求等号右边是一个对象，所以如果等号右边是 `undefined` 或 `null`，就会报错，因为它们无法转为对象。

```javascript
let { ...z } = null // 运行时错误
let { ...z } = undefined // 运行时错误
```

解构赋值必须是最后一个参数，否则会报错。

```javascript
let { ...x, y, z } = someObject; // 句法错误
let { x, ...y, ...z } = someObject; // 句法错误
```

解构赋值的拷贝是浅拷贝，即如果一个键的值是复合类型的值（数组、对象、函数）、那么解构赋值拷贝的是这个值的引用，而不是这个值的副本。

```javascript
let obj = { a: { b: 1 } }
let { ...x } = obj
obj.a.b = 2
x.a.b // 2
```

上面代码中，`x` 是解构赋值所在的对象，拷贝了对象 `obj` 的 `a` 属性。`a` 属性引用了一个对象，修改这个对象的值，会影响到解构赋值对它的引用。

由于数组是特殊的对象，所以对象的扩展运算符也可以用于数组。

```javascript
let foo = { ...['a', 'b', 'c'] }
foo
// {0: "a", 1: "b", 2: "c"}
```

如果扩展运算符后面不是对象，则会自动将其转为对象。

```javascript
// 等同于 {...Object(1)}
{...1} // {}

// 等同于 {...Object(true)}
{...true} // {}

// 等同于 {...Object(undefined)}
{...undefined} // {}

// 等同于 {...Object(null)}
{...null} // {}
```

对象的扩展运算符等同于使用 `Object.assign()`方法。

```javascript
let aClone = { ...a }
// 等同于
let aClone = Object.assign({}, a)
```

与数组的扩展运算符一样，对象的扩展运算符后面可以跟表达式。

```javascript
const obj = {
  ...(x > 1 ? { a: 1 } : {}),
  b: 2,
}
```

扩展运算符的参数对象之中，如果有取值函数 `get`，这个函数是会执行的。

```javascript
let a = {
  get x() {
    throw new Error('not throw yet')
  },
}

let aWithXGetter = { ...a } // 报错
```
