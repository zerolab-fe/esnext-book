# Shared Memory and Atomics

## 简介

ECMAScript 2017 引入了一个新的构造函数 `SharedArrayBuffer` 和 具有辅助函数的命名空间对象 `Atomics`。

## 语法

### SharedArrayBuffer

JavaScript 是单线程，Web worker 引入了多线程：主线程用来与用户互动，Worker 线程用来承担计算任务。每个线程的数据都是隔离的，通过 `postMessage()` 通信。

```javascript
// 主线程
const w = new Worker('worker.js')

w.postMessage('hello')

w.onmessage = function (ev) {
  console.log(ev.data)
}
```

```javascript
// worker.js
onmessage = function (ev) {
  console.log(ev.data)
  postMessage('hi')
}
```

上面代码中，主线程创建了一个 Woker 线程，该线程与主线程之间通过 `postMessage()` 通信。

线程之间的数据交换可以是各种格式，这种数据交换采用复制机制，即一个进程将要分享的数据复制一份，通过 `postMessage` 方法发送给另一个进程。如果数据量较大，通信的效率会明显较低。共享内存的方式，会大大提高效率。

`SharedArrayBuffer` 对象用来表示一个通用的，固定长度的原始二进制数据缓冲区，类似于 `ArrayBuffer` 对象，唯一的区别是后者无法共享数据。

```javascript
new SharedArrayBuffer(length)
```

**参数**

`length`：所创建的数组缓冲区的大小，以字节(byte)为单位。

**返回值**

一个大小指定的新 `SharedArrayBuffer` 对象。其内容被初始化为 0。

**示例**

```javascript
// 主线程

// 新建 1KB 共享内存
const sharedBuffer = new SharedArrayBuffer(1024)

// 主线程将共享内存的地址发送出去
w.postMessage(sharedBuffer)

// 在共享内存上建立视图，供写入数据
const sharedArray = new Int32Array(sharedBuffer)
```

```javascript
// Worker 线程
onmessage = function (ev) {
  // 主线程共享的数据，就是 1KB 的共享内存
  const sharedBuffer = ev.data

  // 在共享内存上建立视图，方便读写
  const sharedArray = new Int32Array(sharedBuffer)

  // ...
}
```

共享内存也可以在 Worker 线程创建，发给主线程。

**注意：** `SharedArrayBuffer` 与 `ArrayBuffer` 一样，本身是无法读写的，必须在上面建立视图，然后通过视图读写。

### Atomics

多线程共享内存，最大的问题就是防止两个线程同时修改某个地址，或者说，当一个线程修改共享内存以后，必须有一个机制让其他线程同步。SharedArrayBuffer API 提供 `Atomics` 对象，保证所有共享内存的操作都是“原子性”的，并且可以在所有线程内同步。

> 原子操作：多个共享内存的线程能够同时读写同一位置上的数据。原子操作会确保正在读或写的数据的值是符合预期的，即下一个原子操作一定会在上一个原子操作结束后才会开始，其操作过程不会中断。

与一般的全局对象不同，`Atomics` 不是构造函数，因此不能使用 `new` 操作符调用，也不能将其当作函数直接调用。`Atomics` 的所有属性和方法都是静态的（与 `Math` 对象一样）。

#### 方法

#### Atomics.store()，Atomics.load()

`store()` 方法用来向共享内存写入数据，并返回该值。 `load()` 方法用来从共享内存读出数据。

> Atomics.store(typedArray, index, value)  
> Atomics.load(typedArray, index)

**typedArray**

一个指定类型的 shared 数组. 类型为 `Int8Array`, `Uint8Array`, `Int16Array`, `Uint16Array`, `Int32Array`, 或者 `Uint32Array` 其中一个。

**index**

`typedArray` 中用来存储 value 的位置。

**value**

要存储的数字。

```javascript
var sab = new SharedArrayBuffer(1024)
var ta = new Uint8Array(sab)

Atomics.store(ta, 0, 12) // 12
Atomics.load(ta, 0) // 12
```

#### Atomics.exchange()

将数组中指定的元素更新为给定的值，并返回该元素更新前的值。

> Atomics.exchange(typedArray, index, value)

```javascript
var sab = new SharedArrayBuffer(1024)
var ta = new Uint8Array(sab)

Atomics.exchange(ta, 0, 12) // returns 0, the old value
Atomics.load(ta, 0) // 12
```

#### Atomics.wait()，Atomics.notify()

这两个方法相当于锁内存，即在一个线程进行操作时，让其他线程休眠（建立锁），等到操作结束，再唤醒那些休眠的线程（解除锁）。

> Atomics.wait(typedArray, index, value\[, timeout\])

**typedArray**

一个共享的 `Int32Array`。

**index**

`typedArray` 中要唤醒的目标索引。

**value**

给定需要检测的位置索引的预期值。

**timeout**

可选，超时前等待的毫秒数。 `Infinity`, 如未提供该参数，将为无穷大。

> Atomics.notify(typedArray, index, count)

**count**

要通知的正在休眠的代理的数量。默认是 `+Infinity`。

**返回值**

一个 String 字符串，值为 "ok", "not-equal", 或 "timed-out" 三种之一。

如果 `sharedArray[index]` 不等于 `value`，就返回字符串 `not-equal`，否则就进入休眠。如果 `Atomics.notify()` 方法唤醒，就返回字符串 `ok`；如果因为超时唤醒，就返回字符串 `timed-out`。

```javascript
// Worker 线程
self.addEventListener(
  'message',
  (event) => {
    const sharedArray = new Int32Array(event.data)
    const arrayIndex = 0
    const expectedStoredValue = 50
    Atomics.wait(sharedArray, arrayIndex, expectedStoredValue)
    console.log(Atomics.load(sharedArray, arrayIndex))
  },
  false,
)
```

上面代码中，`Atomics.wait()` 方法等同于告诉 Worker 线程，只要满足给定条件（`sharedArray` 的 `0` 号位置等于 `50`），就在这一行 Worker 线程进入休眠。

```javascript
// 主线程
const newArrayValue = 100
Atomics.store(sharedArray, 0, newArrayValue)
const arrayIndex = 0
const queuePos = 1
Atomics.notify(sharedArray, arrayIndex, queuePos)
```

上面代码中，sharedArray 的 0 号位置改为 100，然后就执行 Atomics.notify()方法，唤醒在 sharedArray 的 0 号位置休眠队列里的一个线程。

注意，浏览器的主线程不宜设置休眠，这会导致用户失去响应。而且，主线程实际上会拒绝进入休眠。

##### Atomics.add()

将指定位置上的数组元素与给定的值相加，并返回相加前该元素的值。

> Atomics.add(typedArray, index, value)

```javascript
var sab = new SharedArrayBuffer(1024)
var ta = new Uint8Array(sab)

Atomics.add(ta, 0, 12) // returns 0, the old value
Atomics.load(ta, 0) // 12
```

##### Atomics.and()

将指定位置上的数组元素与给定的值相与，并返回与操作前该元素的值。

> Atomics.and(typedArray, index, value)

```javascript
var sab = new SharedArrayBuffer(1024)
var ta = new Uint8Array(sab)
ta[0] = 5

Atomics.and(ta, 0, 1) // returns 0, the old value
Atomics.load(ta, 0) // 1
```

##### Atomics.compareExchange()

如果数组中指定的元素与给定的值相等，则将其更新为新的值，并返回该元素原先的值。

> Atomics.compareExchange(typedArray, index, expectedValue, replacementValue)

**expectedValue**

用于比较的值。

**replacementValue**

将要替换上的值。

```javascript
var sab = new SharedArrayBuffer(1024)
var ta = new Uint8Array(sab)
ta[0] = 7

Atomics.compareExchange(ta, 0, 7, 12) // returns 7, the old value
Atomics.load(ta, 0) // 12
```

##### Atomics.isLockFree()

返回一个布尔值，表示 `Atomics` 对象是否可以处理某个 `size` 的内存锁定。如果返回 `false`，应用程序就需要自己来实现锁定。

> Atomics.isLockFree(size)

```javascript
Atomics.isLockFree(1) // true
Atomics.isLockFree(2) // true
Atomics.isLockFree(3) // false
Atomics.isLockFree(4) // true
Atomics.isLockFree(5) // false
Atomics.isLockFree(6) // false
Atomics.isLockFree(7) // false
Atomics.isLockFree(8) // false
Atomics.isLockFree(Float64Array.BYTES_PER_ELEMENT) // false,Atomics方法无法处理Float64Array
```

##### Atomics.or()

将指定位置上的数组元素与给定的值相或，并返回或操作前该元素的值。

> Atomics.or(typedArray, index, value)

```javascript
var sab = new SharedArrayBuffer(1024)
var ta = new Uint8Array(sab)
ta[0] = 2

Atomics.or(ta, 0, 1) // returns 2, the old value
Atomics.load(ta, 0) // 3
```

##### Atomics.sub()

将指定位置上的数组元素与给定的值相减，并返回相减前该元素的值。

> Atomics.sub(typedArray, index, value)

```javascript
const sab = new SharedArrayBuffer(1024)
const ta = new Uint8Array(sab)
ta[0] = 48

Atomics.sub(ta, 0, 12) // returns 48, the old value
Atomics.load(ta, 0) // 36
```

##### Atomics.xor()

将指定位置上的数组元素与给定的值相异或，并返回异或操作前该元素的值。

> Atomics.xor(typedArray, index, value)

```javascript
const sab = new SharedArrayBuffer(1024)
const ta = new Uint8Array(sab)
ta[0] = 5

Atomics.xor(ta, 0, 1) // returns 5, the old value
Atomics.load(ta, 0) // 4
```
