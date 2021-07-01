# WeakRefs

## 简介

WeakRef 对象包含对对象的弱引用，这个弱引用被称为该 WeakRef 对象的 target 或者是 referent。对对象的弱引用是指当该对象应该被 GC 回收时不会阻止 GC 的回收行为。而与此相反的，一个普通的引用（默认是强引用）会将与之对应的对象保存在内存中。只有当该对象没有任何的强引用时，JavaScript 引擎 GC 才会销毁该对象并且回收该对象所占的内存空间。

## 语法

### 构造函数

`WeakRef()`用来创建一个 WeakRef 对象。

### 实例方法

`WeakRef.prototype.deref()`返回当前实例的 WeakRef 对象所绑定的 target 对象，如果该 target 对象已被 GC 回收则返回 `undefined`。

## 示例

这个例子演示了在一个 DOM 元素中启动一个计数器，当这个元素不存在时停止:

```javascript
class Counter {
  constructor(element) {
    // Remember a weak reference to the DOM element
    this.ref = new WeakRef(element);
    this.start();
  }

  start() {
    if (this.timer) {
      return;
    }

    this.count = 0;

    const tick = () => {
      // Get the element from the weak reference, if it still exists
      const element = this.ref.deref();
      if (element) {
        element.textContent = ++this.count;
      } else {
        // The element doesn't exist anymore
        console.log('The element is gone.');
        this.stop();
        this.ref = null;
      }
    };

    tick();
    this.timer = setInterval(tick, 1000);
  }

  stop() {
    if (this.timer) {
      clearInterval(this.timer);
      this.timer = 0;
    }
  }
}

const counter = new Counter(document.getElementById('counter'));
counter.start();
setTimeout(() => {
  document.getElementById('counter').remove();
}, 5000);
```

## 为什么尽量避免使用

正确使用 WeakRef 对象需要仔细的考虑，最好尽量避免使用。避免依赖于规范没有保证的任何特定行为也是十分重要的。何时、如何以及是否发生垃圾回收取决于任何给定 JavaScript 引擎的实现。**GC 在一个 JavaScript 引擎中的行为有可能在另一个 JavaScript 引擎中的行为大相径庭，或者甚至在同一类引擎，不同版本中 GC 的行为都有可能有较大的差距。**GC 目前还是 JavaScript 引擎实现者不断改进和改进解决方案的一个难题。
