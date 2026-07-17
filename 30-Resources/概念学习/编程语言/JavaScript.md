# JavaScript

> JavaScript 是网页的"灵魂"，让静态的网页动起来。它是唯一一门浏览器原生支持的编程语言，前端开发的核心，现在也能写后端（Node.js）。

**关键词**：动态类型、异步、DOM、事件驱动、ES6、单线程

## 抽象类比

JavaScript 像网页的"神经系统"——HTML 是骨架，CSS 是皮肤，JS 是让网页"动起来"的神经。没有 JS，网页就是一张静态图片。

如果说 HTML 是房子的结构，CSS 是装修风格，JavaScript 就是智能家居系统——让灯能自动开关、门能远程解锁。

---

## 什么是 JavaScript

想象你要做一个网站：
- **HTML**：搭骨架，决定哪里放标题、哪里放图片
- **CSS**：搞装修，决定颜色、字体、布局
- **JavaScript**：加灵魂，让按钮能点击、数据能加载、动画能播放

JavaScript 诞生于 1995 年，只用了 10 天就设计出来。最初只是"网页的小玩具"，没想到成了世界上最流行的语言之一。

JavaScript 和 Java 的关系：**就像"雷锋"和"雷峰塔"，除了名字像，没什么关系。**

### 为什么 JavaScript 这么火

JavaScript 火不是因为设计得多好，而是因为**垄断了浏览器**。

2005 年之前，网页就是静态文档。2005 年 Gmail 和 Google Maps 用 JS 做了"不刷新就能更新"的页面（Ajax），全世界震惊——原来网页可以像软件一样！

然后 Node.js（2009）让 JS 能写后端，React/Vue（2013/2014）让前端开发变得工程化，Electron 让 JS 能写桌面应用（VS Code 就是 Electron 写的）。

**一个语言，从前端到后端到桌面到手机（React Native），全覆盖了。**

### JS 的进化史

JS 经历了几个阶段：
- **1995-2005**：网页小玩具，只能做做表单验证
- **2005-2010**：Ajax 时代，网页能动态更新了
- **2010-2015**：Node.js 出现，JS 能写后端了
- **2015 至今**：ES6 大更新（let/const/箭头函数/Promise），现代 JS 诞生

2015 年的 ES6 是分水岭——之前的 JS 像"凑合用的工具"，之后的 JS 像"正经的编程语言"。

### 常见误解

- **"JS 和 Java 是一种语言"**：完全不是，只是当年为了蹭 Java 热度取了类似的名字
- **"JS 很简单"**：入门简单，但"this 指向"、"闭包"、"原型链"能让人学几年
- **"JS 性能差"**：V8 引擎（Chrome 的 JS 引擎）经过多年优化，性能已经接近 C

---

## 核心概念

### 动态类型：变量没有固定类型

- **静态类型**（Java）：变量像贴了标签的盒子，"这个盒子只能装书"
- **动态类型**（JS）：变量像没贴标签的盒子，"今天装书，明天装衣服，都行"

好处：写代码快，灵活。坏处：运行时才发现类型错误，大项目容易翻车。

### 异步编程：不等人的厨房

- **同步**：你点外卖，等外卖到了才能做下一件事
- **异步**：你点外卖，外卖还在路上时你可以先洗碗、切菜

JavaScript 是单线程的，但通过异步机制（回调、Promise、async/await），可以同时处理多件事而不卡住。

### DOM 操作：网页的遥控器

网页像一台电视，DOM 就是遥控器。JavaScript 通过 DOM 可以：
- 改变网页内容
- 响应用户点击
- 添加/删除元素
- 发送网络请求

---

## 设计哲学

JavaScript 只用了 10 天就设计出来（1995 年，Brendan Eich），初衷是：**给网页加一点交互，让表单能在浏览器里验证**。没人想到它会变成世界上最流行的语言。它的设计哲学不是"深思熟虑"，而是"先跑起来再说"。

### 解决的核心问题

- **浏览器唯一原生语言**：垄断了前端，没有竞争对手
- **动态灵活**：弱类型 + 解释执行，改完代码刷新就能看效果
- **事件驱动**：天生适合"用户点击 → 响应"这种交互场景

### 关键取舍

JS 因为"赶工"留下了很多历史包袱：
- 弱类型 + 自动类型转换 → 灵活，但 `[] + {}` 这种表达式结果很诡异
- 单线程 + 回调 → 简单，但容易陷入"回调地狱"（ES6 后用 Promise/async 修复）
- `this` 指向动态 → 灵活，但成了初学者最大的坑

JS 的哲学是"**实用主义**"——不追求设计完美，追求"能跑、能用、所有人都能上手"。ES6 之后它逐渐补齐了现代语言的短板。

### 类比

JavaScript 像一张"万能便利贴"——开始只是随手记事用的小纸条，结果被贴满了整个办公室，最后变成了支撑整个业务运转的系统。它不优雅，但它无处不在、什么都能粘。

---

## 运行

```bash
node script.js                # Node.js 运行
node                          # 交互式 REPL
```

浏览器中直接写在 HTML 里：

```html
<script>console.log("Hello!")</script>
```

---

## Hello World

```javascript
console.log("Hello, World!");
```

---

## 变量与类型

```javascript
let name = "张三"               // 可变
const age = 25                  // 不可变（推荐用 const）
var old = "旧语法"              // 旧写法，不推荐

let price = 9.99                // 数字（不区分整数和浮点）
let isAdult = true              // 布尔值
let nothing = null              // 空值
let notDefined                  // undefined（未赋值）
```

---

## 字符串

```javascript
const s = "Hello, JavaScript!"
s.toUpperCase()                 // 转大写
s.replace("JavaScript", "World") // 替换
s.split(", ")                   // 分割 → ["Hello", "JavaScript!"]
s.includes("Hello")             // 是否包含
s.length                        // 长度

// 模板字符串（反引号）
const name = "张三"
const greeting = `你好，${name}！`  // 字符串插值
```

---

## 数组

```javascript
const arr = [1, 2, 3, 4, 5]
arr.push(6)                     // 追加
arr.pop()                       // 弹出最后一个
arr.map(x => x * 2)            // [2, 4, 6, 8, 10]
arr.filter(x => x > 3)         // [4, 5]
arr.reduce((sum, x) => sum + x, 0)  // 求和
arr.length                      // 长度

// 解构
const [a, b, ...rest] = arr
```

---

## 对象

```javascript
const user = {
    name: "张三",
    age: 25,
    greet() {
        console.log(`你好，我叫${this.name}`)
    }
}

user.name                       // "张三"
user["age"]                     // 25
user.greet()

// 解构
const { name, age } = user
```

---

## 控制流

```javascript
// if-else
if (age >= 18) {
    console.log("成年")
} else if (age >= 12) {
    console.log("青少年")
} else {
    console.log("儿童")
}

// for 循环
for (let i = 0; i < 10; i++) {
    console.log(i)
}

// for...of（遍历值）
for (const item of arr) {
    console.log(item)
}

// for...in（遍历键）
for (const key in user) {
    console.log(key, user[key])
}

// switch
switch (grade) {
    case "A": console.log("优秀"); break
    case "B": console.log("良好"); break
    default: console.log("其他")
}
```

---

## 函数

```javascript
// 函数声明
function add(a, b) {
    return a + b
}

// 箭头函数（最常用）
const add = (a, b) => a + b

// 默认参数
const greet = (name, greeting = "你好") => `${greeting}，${name}！`

// 剩余参数
const total = (...args) => args.reduce((sum, n) => sum + n, 0)

// 回调函数
arr.forEach(item => console.log(item))
setTimeout(() => console.log("延迟执行"), 1000)
```

---

## 异步编程

```javascript
// Promise
fetch("https://api.example.com/data")
    .then(res => res.json())
    .then(data => console.log(data))
    .catch(err => console.error(err))

// async/await（推荐）
async function fetchData() {
    try {
        const res = await fetch("https://api.example.com/data")
        const data = await res.json()
        console.log(data)
    } catch (err) {
        console.error(err)
    }
}
```

---

## DOM 操作

```javascript
// 获取元素
const el = document.getElementById("app")
const el2 = document.querySelector(".container")
const els = document.querySelectorAll("p")

// 修改
el.textContent = "新内容"
el.innerHTML = "<b>加粗</b>"
el.style.color = "red"
el.classList.add("active")

// 事件
el.addEventListener("click", (e) => {
    console.log("被点击了")
})

// 创建元素
const div = document.createElement("div")
div.textContent = "新元素"
document.body.appendChild(div)
```

---

## 定位速查

| 维度 | JavaScript |
| :--- | :--- |
| **类型检查** | 动态类型 |
| **运行方式** | 浏览器 / Node.js |
| **核心特色** | 异步、事件驱动、DOM 操作 |
| **适合场景** | 前端、后端（Node.js）、桌面（Electron） |
| **代表框架** | React、Vue、Express |

---

## 面试速答

### Q: JavaScript 的 this 指向怎么确定？
A: this 在函数调用时确定，不是定义时确定。普通函数的 this 指向调用者（谁调用就指向谁）；箭头函数没有自己的 this，继承外层作用域的 this；构造函数里 this 指向新创建的对象。这是 JS 最容易出错的点之一。

### Q: var、let、const 有什么区别？
A: var 有变量提升（hoisting），作用域是函数级，可以重复声明；let 是块级作用域，不提升，不能重复声明；const 也是块级作用域，且不能重新赋值（但对象内容可改）。现代 JS 推荐默认用 const，需要改时用 let，不用 var。

### Q: 什么是闭包？有什么用？
A: 闭包是"函数 + 它能访问的外层变量"的组合。内部函数可以访问外部函数的变量，即使外部函数已经返回。用途：创建私有变量（模块模式）、实现柯里化、保存状态。闭包会让变量不被回收，注意内存泄漏。

### Q: Promise 和 async/await 有什么关系？
A: async/await 是 Promise 的语法糖。async 函数返回 Promise，await 等待 Promise 完成。它让异步代码写起来像同步代码，避免 Promise 的 .then 链。本质还是基于 Promise，只是更好读。

### Q: JavaScript 是单线程怎么处理并发？
A: JS 主线程是单线程，但有事件循环（Event Loop）。I/O 操作（网络请求、定时器）交给浏览器/Node 的底层多线程处理，完成后回调进入任务队列。主线程空闲时从队列取回调执行。所以 JS 能"同时"处理多个 I/O 而不阻塞。

---

## 如何给别人讲

1. **一句话定位**：JavaScript 是网页的"灵魂"，唯一一门浏览器原生支持的语言，像"网页的神经系统"
2. **核心卖点**：垄断浏览器、全栈通吃（前端/后端 Node.js/桌面 Electron/手机 React Native）
3. **设计取舍**：10 天赶工设计，灵活但留下 this、闭包、类型转换等历史包袱，靠 ES6 逐渐补齐
4. **谁在用**：所有网页、VS Code、Slack 桌面端、几乎所有互联网公司前端
5. **面试一句话**：JS 是单线程事件驱动的语言，靠异步机制（Promise/async）处理并发，能全栈开发
