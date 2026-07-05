# Node.js 与 npm 入门

> 小白先读：Node.js 让 JavaScript 能跑在服务器上，不再局限于浏览器。npm 是它的包管理器，有上百万个现成的库可以用。
>
> 学这篇时先抓 3 个问题：Node.js 和浏览器里的 JS 有什么区别？npm 是什么？为什么要用 Node.js？

## 先用一句生活话理解

Node.js 像给 JavaScript 装上了"腿"——以前只能在浏览器里跑，现在能跑到服务器上、命令行里。

以前 JavaScript 像只能在厨房做的菜（浏览器），Node.js 让它能送到任何地方（服务器、命令行、桌面应用）。

## 什么是 Node.js？

想象你要开餐厅：
- **浏览器 JS**：只能在顾客面前做菜（前端交互）
- **Node.js**：有了中央厨房，可以备料、做菜、送外卖（后端服务）

**简单说**：Node.js 是 **JavaScript 的运行环境**，让 JS 能跑在浏览器之外。

---

## Node.js 的设计哲学

Node.js 诞生于 2009 年，核心特点：
- **事件驱动**：不阻塞，异步处理
- **单线程**：一个线程处理所有请求
- **非阻塞 I/O**：等待网络请求时不闲着

适合场景：高并发、I/O 密集型（Web 服务器、API）。

---

## 核心概念

### 1. npm：包管理器

**类比**：npm 像应用商店，里面有上百万个现成的库，拿来就能用。

```bash
npm init              # 初始化项目
npm install express   # 安装依赖
npm install -g nodemon # 全局安装
```

**package.json**：项目的"身份证"，记录依赖和脚本。

---

### 2. 模块化：代码分块

**类比**：模块化像乐高积木，把代码拆成小块，各自独立。

```javascript
// math.js
module.exports.add = (a, b) => a + b

// app.js
const { add } = require('./math')
```

---

### 3. 异步编程：不等待

**类比**：异步像点外卖——下单后不用等，可以做其他事，外卖到了再处理。

```javascript
// 异步读取文件
fs.readFile('file.txt', (err, data) => {
  console.log(data)
})
```

---

## Node.js 的应用场景

| 场景 | 例子 | 为什么用 Node.js |
| :--- | :--- | :--- |
| **Web 后端** | Express、Koa | 高并发、异步 |
| **命令行工具** | webpack、vue-cli | 开发快 |
| **桌面应用** | VS Code、Discord | Electron 框架 |
| **实时应用** | 聊天室、游戏 | WebSocket 支持好 |

---

## 总结

### 三句话讲清楚 Node.js

1. Node.js 让 JavaScript 能跑在浏览器之外
2. npm 是包管理器，有上百万个现成的库
3. 事件驱动、异步非阻塞，适合高并发场景

### 核心类比

> Node.js 像给 JavaScript 装上了"腿"——以前只能在浏览器里跑，现在能跑到服务器上。

### 关键词

**Node.js、npm、JavaScript、异步、事件驱动、包管理**
