# HTML

> HTML 不是编程语言，是"标记语言"，用来定义网页的结构和内容。是前端开发三驾马车之一（HTML + CSS + JavaScript），是所有网页的基础。

**关键词**：标签、属性、嵌套、语义化、超链接、HTML5

## 抽象类比

HTML 像房子的"钢筋骨架"——决定了哪里是客厅、哪里是卧室、哪里放门放窗。没有 HTML，网页就不存在。

如果把网页比作一个人：HTML 是骨架，CSS 是皮肤和衣服，JavaScript 是肌肉和表情。

---

## 什么是 HTML

想象你要写一封信：
- **HTML**：信的结构——哪里是标题、哪里是正文、哪里是签名
- **CSS**：信的样式——字体多大、颜色什么、排版怎样
- **JavaScript**：信的功能——点击能回复、能转发、能动画

HTML 诞生于 1993 年，设计者是 Tim Berners-Lee。目标是"**让科学家能分享文档**"。

HTML 不是编程语言，没有逻辑、没有循环，只是"标记"内容是什么。

### 为什么 HTML 这么"简单"却能统治互联网

HTML 从 1993 年活到现在，经历了 HTML4 → XHTML → HTML5 的演变。它没有变复杂，反而越来越强：
- **HTML4**（1999）：只有基本标签
- **HTML5**（2014）：加了 `<video>`、`<canvas>`、`<audio>`、语义化标签……浏览器原生支持视频、画图、录音，不需要 Flash

HTML 能活这么久，不是因为强大，而是因为**简单到不可能被淘汰**。

### HTML 的语义化为什么重要

很多人写 HTML 全是 `<div>`——能跑，但搜索引擎和屏幕阅读器看不懂。

```html
<!-- 不好：全是 div，看不出结构 -->
<div class="header">...</div>
<div class="nav">...</div>
<div class="content">...</div>

<!-- 好：语义化，一看就懂 -->
<header>...</header>
<nav>...</nav>
<main>...</main>
```

语义化的好处：
- **SEO**：搜索引擎能更好理解页面结构
- **无障碍**：屏幕阅读器能正确朗读
- **可维护**：代码一看就知道哪里是头部、哪里是内容

### 常见误解

- **"HTML 不是编程语言，不重要"**：HTML 是 Web 的基础，不懂 HTML 学不了前端
- **"HTML 很简单，不用学"**：语义化、无障碍、SEO 优化，这些"高级话题"都需要理解 HTML
- **"HTML5 = 新标签"**：HTML5 还包括 API（Canvas、LocalStorage、Geolocation 等）

---

## 核心概念

### 标签：内容的"身份证"

标签像超市商品的分类标签——"这是水果"、"这是蔬菜"。HTML 用标签告诉浏览器"这是什么"：
- `<h1>` 这是大标题
- `<p>` 这是段落
- `<img>` 这是图片
- `<a>` 这是链接

### 嵌套：俄罗斯套娃

HTML 元素一层套一层。`<div>` 里套着 `<h1>` 和 `<p>`，`<p>` 里又套着 `<strong>`。

### 属性：标签的"附加信息"

属性像商品的详细说明——"颜色：红色"、"尺寸：大号"。`href`、`src`、`alt` 都是属性，给标签提供额外信息。

### 语义化：标签要有意义

`<header>` 就是头部，`<article>` 就是文章，`<nav>` 就是导航。语义化让搜索引擎、屏幕阅读器都能理解页面结构。

---

## 设计哲学

HTML 诞生于 1993 年（Tim Berners-Lee），最初的用途极其朴素：**让科学家能通过网络分享文档**。它的设计哲学是"**描述内容是什么，而不是怎么显示**"。

### 解决的核心问题

- **文档结构化**：用标签标记"这是标题、这是段落、这是链接"
- **跨平台共享**：任何设备、任何浏览器都能解析同一份 HTML
- **超链接互联**：`<a>` 标签让文档之间能互相跳转，这就是"万维网"的"网"

### 关键取舍

HTML 的哲学是"**简单到不可能被淘汰**"：
- 没有逻辑、没有循环 → 谁都能学，但只能描述结构
- 标签语义化 → 清晰，但要靠规范约束（很多人偷懒全用 `<div>`）
- 容错性强 → 浏览器会尽量"猜"你的意图，但这也导致代码容易乱

HTML 刻意不做任何"计算"，把表现交给 CSS、行为交给 JS。这种"各司其职"的分层设计，是它能活 30 年的原因。

### 类比

HTML 像一份"乐高积木的说明书"——它告诉你每块积木是什么（标题、图片、链接），以及它们怎么拼在一起，但不管积木涂什么颜色（CSS）会不会动（JS）。说明书越简单明了，越能流传下去。

---

## 基本结构

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>页面标题</title>
</head>
<body>
    <h1>大标题</h1>
    <p>段落内容</p>
</body>
</html>
```

---

## 常用标签

### 文本

```html
<h1>大标题</h1>                   <!-- h1~h6 -->
<p>段落</p>
<strong>加粗</strong>
<em>斜体</em>
<br>                              <!-- 换行 -->
<hr>                              <!-- 水平线 -->
<blockquote>引用内容</blockquote>
<code>行内代码</code>
<pre>预格式化文本</pre>
```

### 链接与图片

```html
<a href="https://example.com">点击访问</a>
<a href="https://example.com" target="_blank">新窗口打开</a>
<img src="photo.jpg" alt="照片描述" width="300">
```

### 列表

```html
<!-- 无序列表 -->
<ul>
    <li>苹果</li>
    <li>香蕉</li>
</ul>

<!-- 有序列表 -->
<ol>
    <li>第一步</li>
    <li>第二步</li>
</ol>
```

---

## 表单

```html
<form action="/submit" method="POST">
    <label for="name">姓名：</label>
    <input type="text" id="name" name="name" placeholder="请输入姓名">

    <label for="email">邮箱：</label>
    <input type="email" id="email" name="email">

    <label for="pwd">密码：</label>
    <input type="password" id="pwd" name="pwd">

    <label>
        <input type="checkbox" name="agree"> 同意条款
    </label>

    <select name="city">
        <option value="beijing">北京</option>
        <option value="shanghai">上海</option>
    </select>

    <textarea name="bio" rows="4" cols="30">个人简介</textarea>

    <button type="submit">提交</button>
</form>
```

---

## 表格

```html
<table>
    <thead>
        <tr>
            <th>姓名</th>
            <th>年龄</th>
            <th>城市</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>张三</td>
            <td>25</td>
            <td>北京</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>30</td>
            <td>上海</td>
        </tr>
    </tbody>
</table>
```

---

## 语义化标签

```html
<header>页面头部</header>
<nav>导航栏</nav>
<main>
    <article>
        <h2>文章标题</h2>
        <section>章节内容</section>
    </article>
    <aside>侧边栏</aside>
</main>
<footer>页面底部</footer>
```

语义化标签让页面结构清晰，有利于 SEO 和无障碍访问。

---

## 定位速查

| 维度 | HTML |
| :--- | :--- |
| **是什么** | 超文本标记语言（不是编程语言） |
| **作用** | 定义网页的结构和内容 |
| **核心概念** | 标签、属性、嵌套、语义化 |
| **搭配** | CSS（样式）、JavaScript（行为） |

---

## 面试速答

### Q: HTML 和 HTML5 有什么区别？
A: HTML5 是 HTML 的最新版本，新增了语义化标签（header/nav/article）、多媒体标签（video/audio/canvas）、表单类型（email/date）、以及一批 API（LocalStorage、Geolocation、WebSocket）。它让浏览器原生支持视频和画图，淘汰了 Flash。

### Q: 为什么要用语义化标签？
A: 语义化标签（header/nav/main/article）让结构一目了然，带来三个好处：SEO（搜索引擎能理解页面结构）、无障碍（屏幕阅读器能正确朗读）、可维护性（代码一看就知道哪里是头部、哪里是内容）。全用 div 虽然能跑，但机器看不懂。

### Q: 行内元素和块级元素有什么区别？
A: 块级元素（div/p/h1）独占一行，可以设宽高和边距；行内元素（span/a/strong）不换行，宽高由内容决定，设上下边距无效。还有行内块（inline-block），既能并排又能设宽高。不过用 Flexbox 后这个区别不太重要了。

### Q: HTML 的 DOCTYPE 有什么用？
A: `<!DOCTYPE html>` 告诉浏览器用"标准模式"渲染页面。不写或写错的话，浏览器会进入"怪异模式"（quirks mode），按老式浏览器的非标准方式渲染，导致布局错乱。HTML5 只需写 `<!DOCTYPE html>`，很简单。

### Q: meta 标签有什么作用？
A: meta 标签提供页面的元信息，不显示在页面上。常见用途：声明字符编码（`charset="UTF-8"`）、设置视口（移动端适配）、SEO 描述（keywords/description）、社交媒体卡片（Open Graph）。是页面"看不见但很重要"的配置。


