# import() 函数

## 简介

`import` 命令被 JavaScript 引擎静态分析，先于模块内的其他语句执行，无法取代 `require()` 的动态加载功能，提案建议引入与 `require` 方法类似的 `import` 方法。

## 语法

> import(specifier)

上面代码中，`import` 函数的参数 `specifier`，指定所要加载的模块的位置。`import` 命令能够接受什么参数，`import()` 函数就能接受什么参数，两者区别主要是后者为动态加载。

**返回值**

`import()` 返回一个 Promise 对象。来看一个官方例子。

```html
<!DOCTYPE html>
<nav>
  <a href="books.html" data-entry-module="books">Books</a>
  <a href="movies.html" data-entry-module="movies">Movies</a>
  <a href="video-games.html" data-entry-module="video-games">Video Games</a>
</nav>

<main>Content will load here!</main>

<script>
  const main = document.querySelector('main')
  for (const link of document.querySelectorAll('nav > a')) {
    link.addEventListener('click', (e) => {
      e.preventDefault()

      import(`./section-modules/${link.dataset.entryModule}.js`)
        .then((module) => {
          module.loadPageInto(main)
        })
        .catch((err) => {
          main.textContent = err.message
        })
    })
  }
</script>
```

上面的代码，`import()`函数是运行时执行，类似于 Node 的 `require` 方法，区别主要是前者是异步加载，后者是同步加载。

## 实例

### 按需加载

```javascript
button.addEventListener('click', (event) => {
  import('./dialogBox.js')
    .then((dialogBox) => {
      dialogBox.open()
    })
    .catch((error) => {
      /* Error handling */
    })
})
```

上面代码中，`import()`方法放在 `click` 事件的监听函数之中，只有用户点击了按钮，才会加载这个模块。

### 条件加载

```javascript
if (condition) {
  import('moduleA').then(...);
} else {
  import('moduleB').then(...);
}
```

`import()`可以放在 `if` 代码块，根据不同的情况，加载不同的模块。

### 动态的模块路径

```javascript
import(f())
.then(...);
```

上面代码中，根据函数 f 的返回结果，加载不同的模块。

### 在 async 函数中使用

```javascript
async function main() {
  const myModule = await import('./myModule.js')
  const { export1, export2 } = await import('./myModule.js')
  const [module1, module2, module3] = await Promise.all([
    import('./module1.js'),
    import('./module2.js'),
    import('./module3.js'),
  ])
}
```
