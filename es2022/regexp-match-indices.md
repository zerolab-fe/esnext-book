# 正则之`d`修饰符

正则匹配结果的开始位置和结束位置，目前获取并不是很方便。正则实例的 `exec()`方法，返回结果有一个 `index` 属性，可以获取整个匹配结果的开始位置，但是如果包含组匹配，每个组匹配的开始位置，很难拿到。

ES2022 中提供`d`修饰符，为 `exec()`方法的返回结果加上 `indices` 属性，在这个属性上面可以拿到匹配的开始位置和结束位置。

注意：出于性能原因，`indices` 只有在 `d` 指定了标志时才会添加到结果中。

```javascript
const text = 'zabbcdef';
const re = /ab/d;
const result = re.exec(text);

result.index; // 1
result.indices; // [ [1, 3] ]
```

上面例子中，`exec()`方法的返回结果 `result`，它的 index 属性是整个匹配结果`（ab）`的开始位置，而它的 `indices` 属性是一个数组，成员是每个匹配的开始位置和结束位置的数组。由于该例子的正则表达式没有组匹配，所以 `indices` 数组只有一个成员，表示整个匹配的开始位置是 `1`，结束位置是 `3`。

注意，开始位置包含在匹配结果之中，但是结束位置不包含在匹配结果之中。比如，匹配结果为 `ab`，分别是原始字符串的第 1 位和第 2 位，那么结束位置就是第 3 位。

如果正则表达式包含组匹配，那么 `indices` 属性对应的数组就会包含多个成员，提供每个组匹配的开始位置和结束位置。

```javascript
const text = 'zabbcdef';
const re = /ab+(cd)/d;
const result = re.exec(text);

result.indices; // [ [ 1, 6 ], [ 4, 6 ] ]
```

上面例子中，正则表达式包含一个组匹配，那么 `indices` 属性数组就有两个成员，第一个成员是整个匹配结果`（abbcd）`的开始位置和结束位置，第二个成员是组匹配`（cd）`的开始位置和结束位置。

下面是多个组匹配的例子。

```javascript
const text = 'zabbcdef';
const re = /ab+(cd(ef))/d;
const result = re.exec(text);

result.indices; // [ [1, 8], [4, 8], [6, 8] ]
```

上面例子中，正则表达式包含两个组匹配，所以 `indices` 属性数组就有三个成员。

如果正则表达式包含具名组匹配，`indices` 属性数组还会有一个 `groups` 属性。该属性是一个对象，可以从该对象获取具名组匹配的开始位置和结束位置。

```javascript
const text = 'zabbcdef';
const re = /ab+(?<Z>cd)/d;
const result = re.exec(text);

result.indices.groups; // { Z: [ 4, 6 ] }
```

上面例子中，`exec()`方法返回结果的 `indices.groups` 属性是一个对象，提供具名组匹配 Z 的开始位置和结束位置。

如果获取组匹配不成功，`indices` 属性数组的对应成员则为 `undefined`，`indices.groups` 属性对象的对应成员也是 `undefined`。

```javascript
const text = 'zabbcdef';
const re = /ab+(?<Z>ce)?/d;
const result = re.exec(text);

result.indices[1]; // undefined
result.indices.groups['Z']; // undefined
```

上面例子中，由于组匹配不成功，所以 `indices` 属性数组和 `indices.groups` 属性对象对应的组匹配成员都是 `undefined`。
