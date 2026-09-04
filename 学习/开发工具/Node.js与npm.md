# Node.js 与 npm

> Node.js 让 JavaScript 能在浏览器之外运行，npm 是它的包管理器（全球最大的开源库仓库）。

**关键词**：JavaScript、服务端运行时、事件循环、npm、异步I/O

## 抽象类比

Node.js 像把 JavaScript 从浏览器"解放"出来——以前只能写网页交互，现在能写服务器、命令行工具、桌面应用。npm 是它的"应用商店"。

如果说 JavaScript 是"发动机"，Node.js 就是"底盘+轮子+方向盘"——把发动机装上车，就能上路了。

---

## 什么是 Node.js

Node.js 诞生于 2009 年，设计者是 Ryan Dahl。核心思路：**把 Chrome 浏览器的 V8 引擎拿出来，让它脱离浏览器独立运行**。

这样 JavaScript 就不只能操作网页了——它能读写文件、连数据库、起 HTTP 服务器。

Node.js 的核心特点：
- **单线程 + 事件循环**：一个线程处理所有请求，但不会阻塞
- **非阻塞 I/O**：读写操作异步执行，适合高并发场景
- **npm 生态**：全球最大的开源代码库，什么轮子都有

---

## 核心概念

### 事件循环：单线程也能并发

Node.js 是单线程的，但通过事件循环（Event Loop）处理异步操作。

像餐厅只有一个服务员，但他不站在桌边等你吃完——点完菜就去服务下一桌，菜好了再端上来。一个服务员能服务很多桌客人。

### 非阻塞 I/O：不等人的厨房

读文件、查数据库、发网络请求时，Node.js 不会卡住等结果，而是先去做别的事，结果回来了再处理。

像烧水的同时切菜——水开了再下面，而不是站在锅边傻等。

### 为什么 Node.js 能火

Node.js 火的原因不是"JavaScript 能写后端了"，而是**解决了 C10K 问题**（1 万台并发连接）。

传统的后端模型（如 Apache）：每个请求开一个线程，1 万并发就要 1 万个线程，内存直接爆。

Node.js 的模型：单线程 + 事件循环，1 万个连接只用 1 个线程。像餐厅只有 1 个服务员，但他效率极高——不站在桌边等你吃完，点完菜就去服务下一桌。

**Node.js 适合 I/O 密集型任务**（大量读写数据库、调用 API），不适合 CPU 密集型任务（计算、加密）。

### npm 生态

npm（Node Package Manager）是全球最大的开源代码库，有超过 200 万个包。

npm 的好处：几乎什么功能都有现成的包，不用从零写。
npm 的问题：包太多太杂，质量参差不齐，有些包可能不安全。

### 常见误解

- **"Node.js 是语言"**：不是，Node.js 是运行环境，JavaScript 才是语言
- **"Node.js 适合所有后端"**：适合 I/O 密集型，不适合 CPU 密集型（计算密集用 Go/Java/Rust）
- **"npm 的包都能用"**：要小心，有些包质量差或有安全漏洞，选包要看维护情况

---

## 设计哲学

### 把浏览器引擎搬到服务器

Node.js 的核心设计：**把 Chrome 的 V8 引擎拿出来，让它脱离浏览器独立运行**。

为什么这么设计？2009 年前后，JavaScript 只能在浏览器里跑，写不了服务器。Ryan Dahl 想让 JS 能写后端，最简单的方式就是复用已有的 JS 引擎（V8），而不是重新造一个。

类比：以前 JavaScript 像只能在某个品牌车上用的发动机。Node.js 把这个发动机拆下来，装上底盘、轮子、方向盘，变成一辆能上路的车。

### 单线程事件循环

Node.js 用单线程 + 事件循环处理并发，而不是传统的多线程模型。

为什么这么设计？
- **I/O 是瓶颈**：Web 应用大部分时间在等数据库、等网络、等文件。CPU 闲着，线程却在阻塞等待
- **线程开销大**：每个线程占内存（约 1MB），1 万并发就要 10GB 内存
- **无锁无竞争**：单线程不用加锁，代码简单

Node.js 的思路：**一个线程处理所有请求，I/O 操作异步化，回调通知结果**。

类比：传统模型像一个服务员服务一桌客人，客人点完菜服务员站在厨房等，10 桌客人要 10 个服务员。Node.js 像一个服务员服务所有桌，点完菜就去服务下一桌，菜好了厨房喊他来端。

### 关键取舍

- **I/O 密集 vs CPU 密集**：单线程适合 I/O 密集型（网络请求、数据库），CPU 密集任务会阻塞整个线程。Node.js 明确选择 I/O 场景
- **简单 vs 性能**：单线程代码简单（无锁无竞争），但用不满多核 CPU。通过集群（cluster）或多实例弥补
- **npm 生态 vs 质量管控**：npm 包数量最多（200 万+），但质量参差不齐。选择开放而非管控

---

## 安装

- **官网下载**：[nodejs.org](https://nodejs.org)（推荐 LTS 版本）

```bash
node --version                  # 查看 Node 版本
npm --version                   # 查看 npm 版本
```

---

## 运行 JavaScript

```bash
node app.js                     # 运行 JS 文件
node                            # 进入交互式 REPL（输入 .exit 退出）
```

---

## npm 基础命令

npm 像手机的应用商店，管理项目需要的第三方库。

```bash
npm init -y                     # 初始化项目（生成 package.json）

npm install express             # 安装依赖
npm install -g nodemon          # 全局安装
npm install                     # 根据 package.json 安装所有依赖
npm uninstall express           # 卸载依赖

npm list                        # 查看已安装的包
npm update                      # 更新所有包
npm outdated                    # 查看过期的包
```

---

## package.json

项目的"身份证"和"购物清单"，记录项目信息和依赖。

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  }
}
```

```bash
npm start                       # 运行 start 脚本
npm run dev                     # 运行 dev 脚本
npm test                        # 运行 test 脚本
```

---

## 模块化

### CommonJS（传统方式）

```javascript
// math.js
function add(a, b) { return a + b; }
module.exports = { add };

// app.js
const { add } = require('./math');
console.log(add(1, 2));
```

### ES Module（现代方式）

```javascript
// math.js
export function add(a, b) { return a + b; }

// app.js
import { add } from './math.js';
console.log(add(1, 2));
```

---

## 异步编程

```javascript
// 回调（老式）
fs.readFile('file.txt', (err, data) => {
    if (err) throw err;
    console.log(data);
});

// Promise
fs.promises.readFile('file.txt')
    .then(data => console.log(data))
    .catch(err => console.error(err));

// async/await（推荐）
async function readFile() {
    try {
        const data = await fs.promises.readFile('file.txt', 'utf-8');
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}
```

---

## 写一个简单的 Web 服务器

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello World!\n');
});

server.listen(3000, () => {
    console.log('服务器运行在 http://localhost:3000');
});
```

---

## 全局工具

```bash
npx create-react-app my-app     # 创建 React 项目
npx vite my-app                 # 创建 Vite 项目
npm install -g typescript       # 全局安装 TypeScript
tsc --init                      # 初始化 TypeScript 配置
```

---

## npm vs yarn vs pnpm

| 维度 | npm | yarn | pnpm |
| :--- | :--- | :--- | :--- |
| **速度** | 慢 | 快 | 最快 |
| **磁盘占用** | 大 | 中 | 小（硬链接） |
| **命令** | `npm install` | `yarn add` | `pnpm add` |
| **推荐** | 默认选择 | 中等项目 | 大项目、多项目 |

---

## 面试速答

### Q: Node.js 为什么是单线程还能处理高并发？
A: Node.js 用事件循环（Event Loop）和异步 I/O，单线程不阻塞。I/O 操作（读文件、查数据库）交给底层处理，完成后回调通知。像服务员点完菜不等厨房，去服务下一桌。适合 I/O 密集型场景。

### Q: Node.js 适合什么场景？不适合什么？
A: 适合 I/O 密集型：Web API、实时聊天、爬虫、命令行工具。不适合 CPU 密集型：图像处理、加密计算、大数据处理。CPU 密集任务会阻塞单线程，导致整个服务卡住。

### Q: npm 中 dependencies 和 devDependencies 的区别？
A: dependencies 是生产环境需要的包（如 express、react），安装时用 --save。devDependencies 是开发时才需要的包（如测试工具 jest、构建工具 webpack），用 --save-dev。生产部署时可以只装 dependencies，减小体积。

### Q: CommonJS 和 ES Module 的区别？
A: CommonJS 用 require/module.exports，同步加载，是 Node.js 传统方式。ES Module 用 import/export，异步加载，是现代标准。ES Module 支持静态分析（tree shaking），是未来方向。

### Q: package.json 中的 ^ 和 ~ 有什么区别？
A: `^1.2.3` 表示允许 1.x.x 的版本更新（不升大版本），`~1.2.3` 表示只允许 1.2.x 的更新（不升小版本）。`^` 更宽松，`~` 更保守。还有 `1.2.3`（精确版本，不更新）和 `*`（任意版本，不推荐）。生产环境通常用 `^` 兼顾安全和新特性。