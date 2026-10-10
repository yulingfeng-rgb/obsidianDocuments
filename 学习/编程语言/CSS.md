# CSS

> CSS 不是编程语言，是"样式语言"，用来定义网页长什么样。它是前端开发的三驾马车之一（HTML + CSS + JavaScript），负责美化网页。

**关键词**：选择器、盒模型、层叠、Flexbox、Grid、响应式

## 抽象类比

CSS 像房子的"装修风格"——HTML 决定了哪里是客厅、哪里是卧室，CSS 决定了墙面什么颜色、地板什么材质、灯光怎么打。

如果把网页比作一个人：HTML 是骨架，CSS 是皮肤、衣服、化妆，JavaScript 是肌肉和表情。

---

## 什么是 CSS

想象你要装修房子：
- **HTML**：房子的结构——哪里是墙、哪里是门
- **CSS**：装修风格——墙面颜色、地板材质、灯光效果
- **JavaScript**：智能家居——自动开关门、自动调节灯光

CSS 诞生于 1996 年，目标是"**把内容和样式分离**"。CSS 不是编程语言，没有逻辑、没有循环，只负责让网页变好看。

### 为什么 CSS 让人又爱又恨

CSS 入门极快——改个颜色、调个字号，5 分钟就会。但精通极难——"CSS 是前端最难的部分"不是开玩笑。

难在哪：
- **层叠优先级**：多个样式冲突时，谁生效？选择器权重、`!important`、继承……规则复杂
- **浏览器兼容**：同一个 CSS，Chrome 和 Safari 可能渲染不同
- **布局玄学**：Flexbox、Grid 很强大，但"为什么这个元素跑到了这里"是日常困惑

### CSS 的布局进化

CSS 布局经历了几个时代：
- **Table 时代**（1990s-2000s）：用表格布局，简单但代码臃肿
- **Float 时代**（2000s-2010s）：用浮动布局，但清除浮动是噩梦
- **Flexbox 时代**（2010s）：一维布局，终于能优雅地居中了
- **Grid 时代**（2017+）：二维布局，CSS 布局的终极方案

**现在新项目：Flexbox + Grid 就够了，不需要 Bootstrap 那种框架。**

### 响应式设计：一套代码适配所有屏幕

2010 年 Ethan Marcotte 提出"响应式设计"——用媒体查询让同一套 CSS 适配手机、平板、电脑。

核心思想：**不要为每个设备写一套 CSS，而是让 CSS 根据屏幕宽度自动调整。**

这改变了整个前端开发——从此不再需要 m.baidu.com（手机版）和 www.baidu.com（电脑版）两套代码。

### 常见误解

- **"CSS 很简单"**：入门简单，精通极难。布局、动画、响应式都是深坑
- **"用 Flexbox 就行了，不用学 Grid"**：Flexbox 适合一维布局，Grid 适合二维布局，两者互补
- **"CSS 不需要工程化"**：大项目的 CSS 也很复杂，需要 CSS Modules、Tailwind 等工具管理

---

## 核心概念

### 选择器：找到"谁"要变样式

选择器像点名——"穿红衣服的那位"、"坐在第一排的同学"。CSS 用选择器找到要改样式的元素：
- `p` 所有段落
- `.highlight` 所有 class 为 highlight 的元素
- `#header` id 为 header 的元素

### 盒模型：每个元素都是一个盒子

每个 HTML 元素都是一个快递盒子。由内到外：
- **内容**：盒子里装的东西
- **内边距（padding）**：盒子和内容之间的缓冲
- **边框（border）**：盒子的外壳
- **外边距（margin）**：盒子和其他盒子之间的距离

### 层叠：多个样式可以叠加

多个样式规则作用于同一个元素时，后面的覆盖前面的（除非优先级不同）。这就是"层叠"的含义。

---

## 设计哲学

CSS 诞生于 1996 年，解决的问题是：**HTML 原本只有结构，样式全靠标签属性硬塞，改个颜色要改几百处**。CSS 的核心理念是"**内容和样式分离**"——HTML 管结构，CSS 管外观，互不干扰。

### 解决的核心问题

- **样式复用**：一条规则能应用到成百上千个元素
- **批量修改**：改一处样式，整站跟着变
- **响应式适配**：一套样式根据屏幕大小自动调整

### 关键取舍

CSS 的设计取舍在于"**声明式 vs 命令式**"：
- 它是声明式的——你描述"想要什么样"，不描述"怎么做到" → 简单，但不精确控制过程
- 层叠和继承机制 → 灵活，但优先级冲突时很难调试（"为什么这个样式不生效"）
- 没有逻辑、没有循环 → 门槛低，但大型项目需要预处理器（Sass）或工具（Tailwind）

CSS 不追求"编程能力"，而追求"用最简单的方式描述视觉表现"。

### 类比

CSS 像一本"室内设计师的风格手册"——它不关心房子的结构（那是 HTML 的事），只规定"客厅墙刷米色、地板铺木纹、灯光用暖光"。你只要描述清楚想要的效果，浏览器（装修工人）负责实现。

---

## 引入方式

```html
<!-- 外部样式表（推荐） -->
<link rel="stylesheet" href="style.css">

<!-- 内部样式 -->
<style>
    p { color: blue; }
</style>

<!-- 行内样式（不推荐） -->
<p style="color: red;">红色段落</p>
```

---

## 选择器

```css
/* 元素选择器 */
p { color: blue; }

/* 类选择器 */
.highlight { background: yellow; }

/* ID 选择器 */
#header { font-size: 24px; }

/* 后代选择器 */
.container p { margin: 10px; }

/* 子选择器（直接子元素） */
.container > p { color: red; }

/* 伪类 */
a:hover { color: red; }           /* 鼠标悬停 */
li:first-child { font-weight: bold; }
input:focus { border-color: blue; }

/* 伪元素 */
p::first-line { font-weight: bold; }
p::before { content: "→ "; }
```

---

## 盒模型

```css
.box {
    width: 200px;                 /* 内容宽度 */
    padding: 20px;                /* 内边距 */
    border: 1px solid #333;       /* 边框 */
    margin: 10px;                 /* 外边距 */
}

/* 推荐：让 width 包含 padding 和 border */
* { box-sizing: border-box; }
```

---

## 常用属性

```css
/* 文字 */
font-size: 16px;
font-weight: bold;
font-family: "Microsoft YaHei", sans-serif;
color: #333;
text-align: center;
line-height: 1.5;

/* 背景 */
background-color: #f0f0f0;
background-image: url("bg.jpg");

/* 边框 */
border: 1px solid #ccc;
border-radius: 8px;               /* 圆角 */

/* 显示 */
display: block;                   /* 块级 */
display: inline;                  /* 行内 */
display: none;                    /* 隐藏 */
visibility: hidden;               /* 隐藏但占位 */
```

---

## 布局

### Flexbox（一维布局）

```css
.container {
    display: flex;
    justify-content: center;      /* 主轴对齐 */
    align-items: center;          /* 交叉轴对齐 */
    gap: 10px;                    /* 间距 */
}

/* 常用 justify-content 值 */
/* flex-start | flex-end | center | space-between | space-around */
```

### Grid（二维布局）

```css
.grid {
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;  /* 三列，中间列两倍宽 */
    grid-gap: 10px;
}
```

---

## 定位

```css
/* 相对定位（相对自身原位置偏移） */
.box { position: relative; top: 10px; left: 20px; }

/* 绝对定位（相对最近的定位祖先偏移） */
.box { position: absolute; top: 0; right: 0; }

/* 固定定位（相对浏览器窗口） */
.navbar { position: fixed; top: 0; width: 100%; }
```

---

## 响应式设计

```css
/* 媒体查询：屏幕宽度小于 768px 时 */
@media (max-width: 768px) {
    .container {
        flex-direction: column;
    }
}

/* 常用断点 */
/* 手机：< 768px */
/* 平板：768px ~ 1024px */
/* 桌面：> 1024px */
```

---

## 过渡与动画

```css
/* 过渡 */
.button {
    transition: background-color 0.3s ease;
}
.button:hover {
    background-color: blue;
}

/* 动画 */
@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

.box {
    animation: fadeIn 1s ease-in;
}
```

---

## 定位速查

| 维度 | CSS |
| :--- | :--- |
| **是什么** | 层叠样式表（不是编程语言） |
| **作用** | 定义网页的视觉样式 |
| **核心概念** | 选择器、盒模型、布局（Flex/Grid）、响应式 |
| **搭配** | HTML（结构）、JavaScript（行为） |

---

## 面试速答

### Q: CSS 盒模型是什么？
A: 每个 HTML 元素都是一个盒子，由内容（content）、内边距（padding）、边框（border）、外边距（margin）组成。默认 width 只包含内容，加 `box-sizing: border-box` 后 width 包含 padding 和 border，布局更直观。

### Q: Flexbox 和 Grid 有什么区别？
A: Flexbox 是一维布局（一次控制一行或一列），适合导航栏、按钮组、居中对齐；Grid 是二维布局（同时控制行和列），适合整体页面布局。两者互补，不是替代关系。现代项目通常 Grid 做整体布局，Flexbox 做局部排列。

### Q: CSS 选择器优先级怎么算？
A: 优先级从高到低：`!important` > 内联样式 > ID 选择器 > 类/伪类/属性选择器 > 元素/伪元素选择器。同优先级时后写的覆盖先写的。实际开发尽量避免用 `!important` 和内联样式，靠选择器层级管理优先级。

### Q: 如何实现垂直居中？
A: 最简单的方法：父元素 `display: flex; align-items: center;`。其他方法：Grid 的 `place-items: center`、绝对定位 + transform、line-height（单行文本）。Flexbox 是现代首选。

### Q: 响应式设计怎么做？
A: 用媒体查询（`@media`）根据屏幕宽度应用不同样式。核心是"移动优先"——先写手机样式，再用 `min-width` 逐步增强大屏。配合相对单位（rem、%、vw）和弹性布局，让页面自动适配各种屏幕。


---

## 工程实践路线

- **最小项目**：完成一个响应式组件页面，先写清输入、输出和失败行为，再逐步增加功能。
- **刻意练习**：重点验证盒模型、层叠、选择器优先级、Flex/Grid 与可访问状态，不要只记语法片段。
- **质量门槛**：至少具备格式化、静态检查、单元测试和可重复构建；提交前检查依赖、日志与敏感信息。
- **重点避坑**：滥用 `!important`、固定像素、选择器过深；用 DevTools 检查计算样式。

判断“学会”不以看完教程为准，而以能独立定位报错、解释语言机制、写出测试并说明该语言不适合什么场景为准。
