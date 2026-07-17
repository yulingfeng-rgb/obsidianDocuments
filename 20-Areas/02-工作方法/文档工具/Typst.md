# Typst

> Typst 是一个现代化的排版系统，定位为 LaTeX 的简化替代品，提供毫秒级编译、简洁语法和实时预览。本文介绍 Typst 的核心语法、与 LaTeX 的差异和使用技巧，帮助快速上手现代文档排版。

**关键词**：Typst、排版系统、实时预览、LaTeX 替代品、中文支持

## 抽象类比

Typst 就像一台**现代化排版引擎**。如果说 LaTeX 是"老牌印刷厂"——功能强大但操作繁琐，那么 Typst 就是"新一代速印机"——保留了专业排版的能力，但启动更快（毫秒级编译）、操作更直观（语法接近编程语言但不晦涩）、还能即时看到成品（实时预览）。同样的输入， Typst 能用更短的代码、更快的时间产出同样精美的文档。

---

## 一、介绍

### 1.1 什么是 Typst

Typst 是一个现代的排版系统，旨在提供比 LaTeX 更简单、更快速的文档编写体验。

**特点**：
- 编译速度快（毫秒级）
- 语法简洁直观
- 实时预览
- 错误信息友好
- 内置中文支持

**应用场景**：
- 学术论文
- 技术文档
- 报告
- 书籍
- 简历

### 1.2 与 LaTeX 对比

| 特性 | Typst | LaTeX |
|------|-------|-------|
| 编译速度 | 毫秒级 | 秒级 |
| 语法 | 简洁 | 复杂 |
| 学习曲线 | 平缓 | 陡峭 |
| 实时预览 | 原生支持 | 需要配置 |
| 错误提示 | 友好 | 晦涩 |
| 宏包生态 | 较少 | 丰富 |
| 中文支持 | 内置 | 需要配置 |

---

## 二、安装配置

### 2.1 Windows 环境

#### 方法 1：直接下载（推荐）
```
下载地址：https://typst.app/
安装方式：
1. 下载 typst-windows-x86_64.exe
2. 重命名为 typst.exe
3. 添加到系统 PATH
```

#### 方法 2：使用包管理器
```powershell
# 使用 winget
winget install typst

# 使用 scoop
scoop install typst
```

### 2.2 编辑器配置

#### VS Code + Tinymist
```
安装步骤：
1. 安装 VS Code
2. 安装扩展：Tinymist
3. 自动检测 Typst 编译器
4. 支持实时预览和语法高亮
```

#### Typst Web App
```
网址：https://typst.app/
特点：在线编辑，无需安装
```

### 2.3 验证安装

```bash
# 检查版本
typst --version

# 创建测试文档
typst init hello

# 编译
typst compile hello.typ
```

---

## 三、基本语法

### 3.1 文本格式

```typst
*粗体*
_斜体_
`代码`
~删除线~
```

### 3.2 标题

```typst
= 一级标题
== 二级标题
=== 三级标题
==== 四级标题
```

### 3.3 段落

```typst
第一段内容...

第二段内容...

// 强制换行
\
```

### 3.4 列表

#### 无序列表
```typst
- 项目 1
- 项目 2
  - 子项目
  - 子项目
- 项目 3
```

#### 有序列表
```typst
+ 第一项
+ 第二项
+ 第三项
```

#### 任务列表
```typst
- [x] 已完成
- [ ] 未完成
- [ ] 待办
```

### 3.5 链接与图片

```typst
// 链接
https://typst.app/
#link("https://typst.app/")[链接文本]

// 图片
#image("image.png")
#image("image.png", width: 80%)
#image("image.png", alt: "图片描述")
```

### 3.6 引用

```typst
> 这是一段引用
> 可以多行书写
```

### 3.7 代码块

```typst
```python
def hello():
    print("Hello, Typst!")
```
```

### 3.8 表格

```typst
#table(
  columns: 3,
  align: (center, center, center),
  table.header([列 1], [列 2], [列 3]),
  [数据], [数据], [数据],
  [数据], [数据], [数据],
)
```

### 3.9 注释

```typst
// 这是单行注释

/* 
这是多行注释
可以跨越多行
*/
```

---

## 四、文档结构

### 4.1 完整文档

```typst
#set page(paper: "a4", margin: 2.5cm)
#set text(font: "SimSun", size: 12pt)

#align(center)[
  = 文档标题
  *作者：张三*
  
  2026 年 7 月
]

#outline()

= 引言
内容...

= 方法
内容...

= 结论
内容...
```

### 4.2 页面设置

```typst
#set page(
  paper: "a4",
  margin: (
    top: 2.5cm,
    bottom: 2.5cm,
    left: 2.5cm,
    right: 2.5cm
  )
)
```

### 4.3 字体设置

```typst
#set text(
  font: "SimSun",
  size: 12pt,
  lang: "zh"
)
```

### 4.4 目录

```typst
#outline()
#outline(title: "目录", depth: 2)
```

### 4.5 页眉页脚

```typst
#set page(header: context [
  #if counter(page).get().first() > 1 [
    #align(center)[页眉内容]
  ]
])

#set page(footer: context [
  #align(center)[第 #counter(page).get().first() 页]
])
```

---

## 五、数学公式

### 5.1 行内公式

```typst
$E = m c^2$
```

### 5.2 块级公式

```typst
$ E = m c^2 $
```

### 5.3 多行公式

```typst
$ align(
  a &= b + c \
  d &= e + f \
  g &= h + i
) $
```

### 5.4 常用符号

```typst
$alpha$, $beta$, $gamma$        // 希腊字母
$sum_(i=1)^n x_i$               // 求和
$integral_0^1 f(x) dif x$       // 积分
$frac(a)(b)$                    // 分数
$sqrt(x)$                       // 平方根
$root(3, x)$                    // 立方根
$lim_(x -> oo) f(x)$            // 极限
$infinity$                      // 无穷
$leq$, $geq$, $neq$             // 比较运算符
$approx$                        // 约等于
```

### 5.5 矩阵

```typst
$ mat(
  1, 2, 3;
  4, 5, 6;
  7, 8, 9
) $
```

---

## 六、图表与布局

### 6.1 图片布局

```typst
// 居中图片
#align(center)[
  #image("image.png", width: 80%)
]

// 图片标题
#figure(
  image("image.png"),
  caption: [图片标题],
) <fig:example>
```

### 6.2 多列布局

```typst
#columns(2)[
  第一列内容...
][
  第二列内容...
]
```

### 6.3 网格布局

```typst
#grid(
  columns: (1fr, 1fr, 1fr),
  [单元格 1],
  [单元格 2],
  [单元格 3],
)
```

### 6.4 堆叠布局

```typst
#stack(
  dir: ttb,
  spacing: 1em,
  [内容 1],
  [内容 2],
  [内容 3],
)
```

---

## 七、样式定制

### 7.1 设置样式

```typst
#set heading(
  number: "1.1",
  above: 1.5em,
  below: 0.5em
)
```

### 7.2 自定义函数

```typst
#let my-box(content) = {
  block(
    fill: rgb("#f0f0f0"),
    inset: 10pt,
    radius: 5pt,
    content
  )
}

#my-box[这是框内内容]
```

### 7.3 主题颜色

```typst
#let primary = rgb("#1976d2")
#let secondary = rgb("#388e3c")

#set heading(fill: primary)
#set link(fill: secondary)
```

### 7.4 模板

```typst
// 创建模板文件 template.typ
#let template(body) = {
  set page(paper: "a4")
  set text(font: "SimSun")
  
  align(center)[
    = 标题
    *作者*
  ]
  
  body
}

// 使用模板
#import "template.typ": template
#show: template

= 引言
内容...
```

---

## 八、工具与资源

### 8.1 在线平台

| 平台            | 链接                        | 特点      |
| ------------- | ------------------------- | ------- |
| Typst App     | https://typst.app/        | 官方在线编辑器 |

### 8.2 包管理

```bash
# 安装包
typst install package-name

# 列出已安装包
typst list

# 更新包
typst update
```

---

## 九、编译命令

### 9.1 基本编译

```bash
# 编译为 PDF
typst compile document.typ

# 指定输出文件
typst compile document.typ output.pdf

# 监视模式（实时编译）
typst watch document.typ
```

### 9.2 参数传递

```bash
# 传递变量
typst compile --input name=value document.typ
```
