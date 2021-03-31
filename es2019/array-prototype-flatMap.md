# Array.prototype.flat / Array.prototype.flatMap

`flat()`方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

`flat()`默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将 `flat()`方法的参数写成一个整数，表示想要拉平的层数，默认为 1。

```
[1, 2, [3, [4, 5]]].flat()
// [1, 2, 3, [4, 5]]

[1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]
```

如果不管有多少层嵌套，都要转成一维数组，可以用 `Infinity` 关键字作为参数。

```
[1, [2, [3]]].flat(Infinity)
// [1, 2, 3]
```

如果原数组有空位，`flat()`方法会跳过空位。

```
[1, 2, , 4, 5].flat()
// [1, 2, 4, 5]
```

`flatMap()`方法对原数组的每个成员执行一个函数（相当于执行 `Array.prototype.map()`），然后对返回值组成的数组执行 `flat()`方法，即 depth 值为 1。该方法返回一个新数组，不改变原数组。

```javascript
let new_array = arr.flatMap(function callback(currentValue[, index[, array]]) {
  // return element for new_array
}[, thisArg])
```

**currentValue**  
当前正在数组中处理的元素

**index** 可选  
可选的。数组中正在处理的当前元素的索引。

**array** 可选  
可选的。被调用的 `map` 数组

**thisArg** 可选  
可选的。执行 callback 函数时 使用的 `this` 值。
