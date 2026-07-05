# Pandoc

> 通用文档转换工具

---

## 一、介绍

### 1.1 什么是 Pandoc

Pandoc 是一个通用的文档转换工具，可以在几乎任何格式的标记文档之间转换。

**应用场景**：
- Markdown 转 PDF
- Markdown 转 Word
- HTML 转 Markdown
- LaTeX 转 HTML
- 批量文档转换

### 1.2 支持的格式

| 输入格式 | 输出格式 |
|---------|---------|
| Markdown | PDF |
| HTML | Word (docx) |
| LaTeX | HTML |
| reStructuredText | ePub |
| AsciiDoc | LaTeX |
| DocBook | ODT |
| Textile | Beamer |
| Org-mode | Slides |

---

## 二、安装配置

### 2.1 Windows 环境

#### 直接下载
```
下载地址：https://github.com/jgm/pandoc/releases
安装方式：
1. 下载 pandoc-x.x.x-windows-x86_64.zip
2. 解压到任意目录
3. 添加到系统 PATH
```

### 2.2 验证安装

```bash
# 检查版本
pandoc --version

# 查看支持的格式
pandoc --list-input-formats
pandoc --list-output-formats
```

### 2.3 配置 PDF 引擎

**推荐：TeX Live 或 MiKTeX**

---

## 三、基本用法

### 3.1 基本语法

```bash
pandoc input.md -o output.pdf
```

### 3.2 常用选项

```bash
# 指定输入格式
pandoc -f markdown -t html input.md -o output.html

# 指定输出格式
pandoc input.md -o output.docx

# 添加目录
pandoc --toc input.md -o output.pdf

# 指定模板
pandoc --template=template.html input.md -o output.html

# 添加元数据
pandoc --metadata title="文档标题" input.md -o output.pdf
```

### 3.3 元数据文件

```yaml
---
title: 文档标题
author: 作者
date: 2026-07-03
---

# 正文内容
```

---

## 四、格式转换

### 4.1 Markdown 转 PDF

```bash
# 基本转换
pandoc input.md -o output.pdf

# 指定 PDF 引擎
pandoc --pdf-engine=xelatex input.md -o output.pdf

# 使用 typst
pandoc --pdf-engine=typst input.md -o output.pdf

# 添加目录
pandoc --toc --toc-depth=3 input.md -o output.pdf

# 添加页码
pandoc -V pagestyle=number input.md -o output.pdf
```

### 4.2 Markdown 转 Word

```bash
# 基本转换
pandoc input.md -o output.docx

# 使用参考文档（保持样式）
pandoc --reference-doc=template.docx input.md -o output.docx

# 添加目录
pandoc --toc input.md -o output.docx
```

### 4.3 Markdown 转 HTML

```bash
# 基本转换
pandoc input.md -o output.html

# 独立 HTML（包含 CSS）
pandoc --standalone --css=style.css input.md -o output.html

# 嵌入 CSS
pandoc --standalone --css=style.css --embed-resources input.md -o output.html

# 添加目录
pandoc --toc --toc-depth=3 input.md -o output.html
```

### 4.4 Markdown 转 LaTeX

```bash
# 基本转换
pandoc input.md -o output.tex

# 指定文档类
pandoc -V documentclass=article input.md -o output.tex

# 使用 ctex（中文）
pandoc -V documentclass=ctexart input.md -o output.tex

# 添加 bibliography
pandoc --bibliography=references.bib input.md -o output.tex
```

### 4.5 HTML 转 Markdown

```bash
# 基本转换
pandoc input.html -t markdown -o output.md

# GitHub Flavored Markdown
pandoc input.html -t gfm -o output.md
```

### 4.6 LaTeX 转 Markdown

```bash
# 基本转换
pandoc input.tex -t markdown -o output.md
```

### 4.7 批量转换

```bash
# Windows PowerShell
Get-ChildItem *.md | ForEach-Object {
    pandoc $_.FullName -o ($_.BaseName + ".pdf")
}

# Linux/Mac
for file in *.md; do
    pandoc "$file" -o "${file%.md}.pdf"
done
```

---

## 五、高级功能

### 5.1 扩展语法

```bash
# 启用所有扩展
pandoc --standalone --smart input.md -o output.pdf

# 启用特定扩展
pandoc -f markdown+footnotes+citations input.md -o output.pdf
```

### 5.2 代码高亮

```bash
# 使用默认高亮
pandoc --highlight-style=tango input.md -o output.pdf

# 列出可用样式
pandoc --list-highlight-styles

# 自定义高亮
pandoc --highlight-style=custom.theme input.md -o output.pdf
```

### 5.3 数学公式

```bash
# MathJax（HTML）
pandoc --mathjax input.md -o output.html

# KaTeX
pandoc --katex input.md -o output.html

# MathML
pandoc --mathml input.md -o output.html
```

### 5.4 参考文献

```bash
# BibTeX
pandoc --bibliography=references.bib --csl=ieee.csl input.md -o output.pdf

# 列出可用 CSL
pandoc --list-csl-styles
```

### 5.5 过滤器

```bash
# 使用 Lua 过滤器
pandoc --lua-filter=filter.lua input.md -o output.pdf

# 使用 citeproc
pandoc --citeproc input.md -o output.pdf
```

---

## 六、模板与样式

### 6.1 自定义模板

```bash
# 生成默认模板
pandoc -D html > template.html

# 使用模板
pandoc --template=template.html input.md -o output.html
```

### 6.2 模板变量

```yaml
---
title: 文档标题
author: 作者
date: 2026-07-03
abstract: 摘要内容
---
```

### 6.3 CSS 样式

```css
/* style.css */
body {
  font-family: "SimSun", serif;
  font-size: 12pt;
  line-height: 1.6;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  color: #1976d2;
  border-bottom: 2px solid #1976d2;
  padding-bottom: 10px;
}

code {
  background-color: #f5f5f5;
  padding: 2px 5px;
  border-radius: 3px;
}
```
