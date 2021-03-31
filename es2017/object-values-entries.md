# Object.values / Object.entries

## 简介

在 Object 中，ES2017 新增了两个新的函数，分别是 `Object.values` 函数和 `Object.entries` 函数。

`Object.values()` 方法返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用 `for...in` 循环的顺序相同（区别在于 for-in 循环枚举原型链中的属性）。

`Object.entries()` 方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 `for...in` 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）。

两者枚举属性的顺序与通过手动循环对象的属性值所给出的顺序相同，如果属性名为数值的属性，是按照数值大小，从小到大遍历的。

## 语法

### Object.values()

#### 属性值为字符串

```javascript
const obj = { foo: 'bar', baz: 'test' }

Object.values(obj)
// ["bar", "test"]
```

#### 属性值为数字

```javascript
const obj = { 666: 'a', 2: 'b', 8: 'c' }

Object.values(obj)
// ["b", "c", "a"]
```

上面例子中，属性名为数值，按照数值大小遍历，因此返回的顺序是 `b`、`c`、`a`。

#### 只返回对象本身的可遍历属性

```javascript
const obj = Object.create({}, { p: { value: 42 } })

Object.values(obj)
// []
```

上面代码中，`Object.create` 方法的第二个参数添加的对象属性 `enumerable` 默认是 `false`，`Object.values` 不会返回这个属性。只要把 `enumerable` 改成 `true`，`Object.values` 就会返回属性 `p` 的值。

```javascript
const obj = Object.create(
  {},
  {
    p: {
      value: 42,
      enumerable: true,
    },
  },
)

Object.values(obj)
// [42]
```

#### 属性值为 Symbol

```javascript
Object.values({ [Symbol()]: 123, foo: 'abc' })
// ['abc']
```

`Object.values` 会过滤属性名为 Symbol 值的属性。

#### 参数不是对象

如果 `Object.values` 方法的参数是一个字符串，会返回各个字符组成的一个数组。

```javascript
Object.values('foo')
// ['f', 'o', 'o']
```

如果参数不是对象，`Object.values` 会先将其转为对象。

由于数值和布尔值的包装对象，都不会为实例添加非继承的属性。所以，Object.values 会返回空数组。

```javascript
Object.values(42) // []
Object.values(true) // []
```

### Object.entries()

除了返回值不一样，该方法的行为与 `Object.values` 基本一致。

#### 遍历对象的属性

```javascript
const obj = { a: 5, b: 7, c: 9 }
for (const [key, value] of Object.entries(obj)) {
  console.log(`${key}: ${value}`)
}
// "a: 5", "b: 7", "c: 9"
```

#### 将 `Object` 转为 `Map` 结构

`new Map()` 构造函数接受一个可迭代的 `entries`。借助 `Object.entries` 方法你可以很容易的将 `Object` 转换为 `Map`:

```javascript
var obj = { foo: 'bar', baz: 42 }
var map = new Map(Object.entries(obj))

console.log(map)
// Map { foo: "bar", baz: 42 }
```
