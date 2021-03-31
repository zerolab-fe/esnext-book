# import.meta

## 简介

开发者使用一个模块时，有时需要知道模板本身的一些信息（比如模块的路径）。ES2020 为 import 命令添加了一个元属性 `import.meta`，返回当前模块的元信息。

## 语法

`import.meta` 只能在模块内部使用，如果在模块外部使用会报错。

这个属性返回一个对象，该对象的各种属性就是当前运行的脚本的元信息。具体包含哪些属性，标准没有规定，由各个运行环境自行决定。一般来说，`import.meta` 至少会有下面两个属性。

### import.meta.url

`import.meta.url` 返回当前模块的 URL 路径。举例来说，当前模块主文件的路径是 `https://foo.com/main.js`，`import.meta.url` 就返回这个路径。如果模块里面还有一个数据文件 `data.txt`，那么就可以用下面的代码，获取这个数据文件的路径。

```javascript
new URL('data.txt', import.meta.url)
```

注意，Node.js 环境中，`import.meta.url` 返回的总是本地路径，即是 `file:URL` 协议的字符串，比如 `file:///home/user/foo.js`。

### import.meta.scriptElement

`import.meta.scriptElement` 是浏览器特有的元属性，返回加载模块的那个`<script>`元素，相当于 `document.currentScript` 属性。

```javascript
// HTML 代码为
// <script type="module" src="my-module.js" data-foo="abc"></script>

// my-module.js 内部执行下面的代码
import.meta.scriptElement.dataset.foo
// "abc"
```
