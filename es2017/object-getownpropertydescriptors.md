# Object.getOwnPropertyDescriptors

## 简介

ES5 的 `Object.getOwnPropertyDescriptor()` 方法会返回某个对象属性的描述对象（descriptor）。ES2017 引入了 `Object.getOwnPropertyDescriptors()` 方法，返回指定对象所有自身属性（非继承属性）的描述对象。

## 语法

基本用法如下。

```javascript
const obj = {
  foo: 123,
  get bar() {
    return 'abc'
  },
}

Object.getOwnPropertyDescriptors(obj)
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: get bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } }
```

该方法的实现非常容易。

```javascript
function getOwnPropertyDescriptors(obj) {
  const result = {}
  for (let key of Reflect.ownKeys(obj)) {
    result[key] = Object.getOwnPropertyDescriptor(obj, key)
  }
  return result
}
```

该方法引入的目的，主要是为了解决 `Object.assign()` 无法正确拷贝 `get` 属性和 `set` 属性的问题。`Object.assign` 方法总是拷贝一个属性的值，而不会拷贝它背后的赋值方法或取值方法。

这时，`Object.getOwnPropertyDescriptors()` 方法配合 `Object.defineProperties()` 方法，就可以实现正确拷贝。

```javascript
const source = {
  set foo(value) {
    console.log(value)
  },
}

const target2 = {}
Object.defineProperties(target2, Object.getOwnPropertyDescriptors(source))

Object.getOwnPropertyDescriptor(target2, 'foo')
// { get: undefined,
//   set: [Function: set foo],
//   enumerable: true,
//   configurable: true }
```

## 实例

### 浅拷贝一个对象

配合 `Object.create()` 方法，将对象属性克隆到一个新对象。

```javascript
Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj))
```

### 创建子类

创建子类的典型方法是定义子类，将其原型设置为超类的实例，然后在该实例上定义属性。这么写很不优雅，特别是对于 getters 和 setter 而言。可以下面方式设置原型：

```javascript
function superclass() {}
superclass.prototype = {
  // 在这里定义方法和属性
}
function subclass() {}
subclass.prototype = Object.create(
  superclass.prototype,
  Object.getOwnPropertyDescriptors({
    // 在这里定义方法和属性
  }),
)
```
