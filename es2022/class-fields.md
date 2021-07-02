# Class 字段相关特性

## 使用声明式的类属性

ES6 版本的 `Class` 中，我们只能给这个类创建可以公共访问的实例方法和静态方法，如果希望声明类字段的话，只能在 `constructor` 中直接通过向 `this` 赋值实现：

```javascript
class IncreasingCounter {
  constructor() {
    this._count = 0;
  }
  get value() {
    console.log('Getting the current value!');
    return this._count;
  }
  increment() {
    this._count++;
  }
}
```

在 ES2022 的提案中，我们就可以使用声明式的类字段：

```javascript
class IncreasingCounter {
  _count = 0;
  get value() {
    console.log('Getting the current value!');
    return this._count;
  }
  increment() {
    this._count++;
  }
}
```

这种新写法的好处是，所有实例对象自身的属性都定义在类的头部，看上去比较整齐，一眼就能看出这个类有哪些实例属性。

## 私有属性、私有方法、getter/setters

在属性或者方法之前添加`#`可以声明为私有属性或者私有方法。

```javascript
class Counter {
  #xValue = 0;

  constructor() {
    super();
    // ...
  }

  get #x() {
    return #xValue;
  }
  set #x(value) {
    this.#xValue = value;
  }
}
```

`#xValue` 是一个私有属性，它的读写都通过`get #x()`和`set #x()`来完成。

## static

```javascript
class Student {
  // 静态私有属性（只能在此Class内部使用）
  static #staticFieldPrivateName = '1';

  // 静态公有属性
  static staticFieldPublicName = '2';

  // 静态公有方法（ES6已支持）
  static staticPublicMethod() {}

  // 静态私有方法（只能在此Class内部使用）
  static #staticPrivateMethod() {}
}
```
