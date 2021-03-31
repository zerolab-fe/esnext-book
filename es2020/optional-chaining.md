# Optional Chaining 可选链操作符

## 简介

在编程中，如果读取对象内部的某个属性，往往需要判断一下该对象是否存在。比如，要读取 `message.body.user.firstName`，安全的写法是写成下面这样。

```javascript
const firstName =
  (message && message.body && message.body.user && message.body.user.firstName) || 'default'
```

上面例子中，`firstName` 属性在对象的第四层，所以需要判断四次，每一层是否有值。

这样的层层判断非常麻烦，因此 ES2020 引入了“可选链操作符”（optional chaining operator）`?.`，简化上面的写法。

```javascript
const firstName = message?.body?.user?.firstName || 'default'
```

## 语法

> obj?.prop // 对象属性  
> obj?.\[expr\] // 对象属性  
> arr?.\[index\] // 数组取值  
> func?.(args) // 函数或对象方法的调用

### 基本使用

```javascript
let myMap = new Map()
myMap.set('foo', { name: 'baz', desc: 'inga' })

myMap.get('bar')?.name
// undefined
```

### 表达式

```javascript
let nestedProp = obj?.['prop' + 'Name']
```

### 函数调用

```javascript
let result = someInterface.customMethod?.()
```

### 访问数组

```javascript
let arrayItem = arr?.[42]
```

### 短路计算

当在表达式中使用可选链时，如果左操作数是 `null` 或 `undefined`，表达式将不会被计算，例如：

```javascript
let potentiallyNullObj = null
let x = 0
let prop = potentiallyNullObj?.[x++]

console.log(x) // x 将不会被递增，依旧输出 0
```

### delete 运算符

```javascript
delete a?.b
// 等同于
a == null ? undefined : delete a.b
```

上面代码中，如果 `a` 是 `undefined` 或 `null`，会直接返回 `undefined`，而不会进行 `delete` 运算。

### 使用空值合并操作符

空值合并操作符可以在使用可选链时设置一个默认值：

```javascript
let customer = {
  name: 'Carl',
  details: { age: 82 },
}
let customerCity = customer?.city ?? '木子朗'
console.log(customerCity) // "木子朗"
```

### 括号的影响

如果属性链有圆括号，链判断运算符对圆括号外部没有影响，只对圆括号内部有影响。

```javascript
;(a?.b).c(
  // 等价于
  a == null ? undefined : a.b,
).c
```

上面代码中，`?.`对圆括号外部没有影响，不管 `a` 对象是否存在，圆括号后面的`.c` 总是会执行。

一般来说，使用`?.`运算符的场合，不应该使用圆括号。

### 不能用于赋值

```javascript
let object = {}
object?.property = 1 // Uncaught SyntaxError: Invalid left-hand side in assignment
```

### 右侧不得为十进制数值

为了保证兼容以前的代码，允许 `foo?.3:0` 被解析成 `foo ? .3 : 0`，因此规定如果`?.`后面紧跟一个十进制数字，那么`?.`不再被看成是一个完整的运算符，而会按照三元运算符进行处理，也就是说，那个小数点会归属于后面的十进制数字，形成一个小数。
