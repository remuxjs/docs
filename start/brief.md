# 简介

### 什么是 remux？

remux 是一个非常方便的RPC库/框架，可以将本地函数和远程函数写在同一个文件中，通过`@remux`标记进行区分。

下面是一个基本示例，也可以打开[stackblitz](https://stackblitz.com/github/remuxjs/example?file=src/main.js)进行体验：

```javascript
// @remux server
function serverFunction() {
  console.log('Ouch!')
}

// @remux browser
{
  // put browser code here
  const btn = document.createElement('button')
  btn.innerText = 'Hit me'
  btn.addEventListener('click', () => serverFunction())
  document.body.appendChild(btn)
}
```

上面的示例中，`@remux browser`部分的代码创建了一个按钮，并将点击事件注册为一个调用`serverFunction`的闭包，当用户点击按钮时，服务器会打印一行`Ouch!`。

这个示例展示了remux的两个核心功能，**无缝远程调用**和**区分多端代码**，用`@remux`注释的函数会被视为远程函数，而代码块则会被视为只在特定的端运行。至于`@remux`后面分别会跟着的`server`和`browser`，这两个名字是随意起的，remux本身并不太关心开发者起什么名字，只要在编译时传入即可。

### 实现原理

remux提供了一个[babel插件](https://github.com/remuxjs/babel-plugin)用于转译源码。

它会将所有被`@remux`标记的函数封装为对`_remuxInvoke`函数的调用，同时也会在源码的开始位置添加一个`import { _remuxInvoke } from '@remux/lib'`，当然这里的`@remux/lib`可以通过参数改为其他库，只是默认是使用[@remux/lib](https://github.com/remuxjs/lib)。

此外，对于所有使用`@remux`标记的代码块、变量声明和`import`语句，如果不是本端的则会被移除。通常并不需要用于标记变量声明，因为构建时的tree-shaking会确保没有使用的变量会被移除，所以如果存在一些服务器的密钥等敏感信息，请确保构建时打开了tree-shaking，不然就加标记来确保客户端代码中不会包含敏感信息。