# LaTeX

> LaTeX 是一个高质量的排版系统，特别适用于生成复杂的技术和科学文档。本文介绍 LaTeX 的核心概念、常用语法和使用技巧，帮助快速上手论文与技术文档的撰写。

**关键词**：LaTeX、排版系统、数学公式、学术论文、参考文献管理

## 抽象类比

LaTeX 就像一座**自动化印刷厂**。你只需把内容（文字、公式、图表）按"稿纸规范"交给它，它就会像排字工人一样自动处理字号、行距、对齐、目录、编号和参考文献，最后交付一本印刷级的成品。与 Word"所见即所得"不同，LaTeX 是"所写即所得"——你专注于内容，它专注于排版。

---

## 一、介绍

### 1.1 什么是 LaTeX

LaTeX 是一个高质量的排版系统，特别适用于生成复杂的技术和科学文档。

**特点**：
- 专业的排版质量
- 强大的数学公式支持
- 自动化参考文献管理
- 跨平台兼容

**应用场景**：
- 学术论文
- 毕业论文
- 技术报告
- 书籍出版
- 演示文稿

### 1.2 与 Word 对比

| 特性 | LaTeX | Word |
|------|-------|------|
| 排版质量 | 专业级 | 普通 |
| 数学公式 | 强大 | 有限 |
| 学习曲线 | 较陡 | 平缓 |
| 协作编辑 | 困难 | 容易 |
| 版本控制 | 友好 | 不友好 |
| 实时预览 | 需要编译 | 所见即所得 |

---

## 二、安装配置

### 2.1 Windows 环境

#### TeX Live（推荐）
```
下载地址：https://tug.org/texlive/
安装方式：
1. 下载 安装程序
2. 运行安装程序
3. 选择"完整安装"
4. 安装时间：1-2 小时
```

### 2.2 编辑器配置

#### VS Code + LaTeX Workshop
```
安装步骤：
1. 安装 VS Code
2. 安装扩展：LaTeX Workshop
3. 配置 settings.json（网络搜索）
```

#### TeXstudio
```
下载地址：https://www.texstudio.org/
特点：专为 LaTeX 设计，功能完整
配置：
1. Default Compiler: XeLaTeX
2. 设置中文支持
```

### 2.3 中文支持

**使用 XeLaTeX + ctex 宏包**：

---

## 三、基本语法

### 3.1 文档结构

```latex
\documentclass[options]{class}
% 宏包导入
\usepackage{package1}
\usepackage{package2}

% 文档信息
\title{文档标题}
\author{作者}
\date{日期}

\begin{document}
\maketitle  % 生成标题
\tableofcontents  % 生成目录

% 正文内容
\section{引言}
内容...

\section{方法}
内容...

\end{document}
```

### 3.2 文档类

```latex
\documentclass{article}      % 文章
\documentclass{report}       % 报告
\documentclass{book}         % 书籍
\documentclass{beamer}       % 演示文稿
\documentclass{ctexart}      % 中文文章
```

### 3.3 文本格式

```latex
\textbf{粗体}
\textit{斜体}
\underline{下划线}
\texttt{等宽字体}
\emph{强调}
```

### 3.4 字体大小

```latex
{\tiny 极小}
{\scriptsize 很小}
{\footnotesize 小}
{\small 较小}
{\normalsize 正常}
{\large 较大}
{\Large 大}
{\LARGE 很大}
{\huge 巨大}
{\Huge 极大}
```

### 3.5 列表

#### 无序列表
```latex
\begin{itemize}
  \item 项目 1
  \item 项目 2
  \begin{itemize}
    \item 子项目
  \end{itemize}
\end{itemize}
```

#### 有序列表
```latex
\begin{enumerate}
  \item 第一项
  \item 第二项
  \item 第三项
\end{enumerate}
```

### 3.6 环境

```latex
\begin{center}
居中内容
\end{center}

\begin{flushleft}
左对齐内容
\end{flushleft}

\begin{flushright}
右对齐内容
\end{flushright}
```

### 3.7 注释

```latex
% 这是单行注释

\iffalse
这是多行注释
可以跨越多行
\fi
```

---

## 四、文档结构

### 4.1 章节

```latex
\part{部分}           % 部分（book/report）
\chapter{章}          % 章（book/report）
\section{节}          % 节
\subsection{小节}     % 小节
\subsubsection{子小节} % 子小节
\paragraph{段落}      % 段落
\subparagraph{子段落} % 子段落
```

### 4.2 目录

```latex
\tableofcontents  % 目录
\listoffigures    % 图表目录
\listoftables     % 表格目录
```

### 4.3 页面设置

```latex
\usepackage{geometry}
\geometry{
  a4paper,           % A4 纸张
  left=2.5cm,        % 左边距
  right=2.5cm,       % 右边距
  top=2.5cm,         % 上边距
  bottom=2.5cm       % 下边距
}
```

### 4.4 页眉页脚

```latex
\usepackage{fancyhdr}
\pagestyle{fancy}
\fancyhead[L]{左页眉}
\fancyhead[R]{右页眉}
\fancyfoot[C]{\thepage}
```

---

## 五、数学公式

### 5.1 行内公式

```latex
$E = mc^2$
```

### 5.2 块级公式

```latex
\begin{equation}
  E = mc^2
  \label{eq:einstein}
\end{equation}
```

### 5.3 多行公式

```latex
\begin{align}
  a &= b + c \\
  d &= e + f \\
  g &= h + i
\end{align}
```

### 5.4 常用符号

```latex
\alpha, \beta, \gamma      % 希腊字母
\sum, \prod, \int          % 求和、连乘、积分
\frac{a}{b}                % 分数
\sqrt{x}                   % 平方根
\sqrt[3]{x}                % 立方根
\lim_{x \to \infty}        % 极限
\infty                     % 无穷
\leq, \geq, \neq           % 小于等于、大于等于、不等于
\approx                    % 约等于
```

### 5.5 矩阵

```latex
\begin{pmatrix}
  1 & 2 & 3 \\
  4 & 5 & 6 \\
  7 & 8 & 9
\end{pmatrix}
```

---

## 六、图表插入

### 6.1 图片

```latex
\usepackage{graphicx}

\begin{figure}[htbp]
  \centering
  \includegraphics[width=0.8\textwidth]{image.png}
  \caption{图片标题}
  \label{fig:example}
\end{figure}
```

### 6.2 表格

```latex
\begin{table}[htbp]
  \centering
  \caption{表格标题}
  \label{tab:example}
  \begin{tabular}{|c|c|c|}
    \hline
    列 1 & 列 2 & 列 3 \\
    \hline
    数据 & 数据 & 数据 \\
    数据 & 数据 & 数据 \\
    \hline
  \end{tabular}
\end{table}
```

### 6.3 浮动体参数

```latex
[h]  - here，当前位置
[t]  - top，页面顶部
[b]  - bottom，页面底部
[p]  - page，单独一页
[htbp] - 按顺序尝试
```

---

## 七、参考文献

### 7.1 BibTeX 方式

#### 创建 .bib 文件
```bibtex
@article{einstein1905,
  title={On the electrodynamics of moving bodies},
  author={Einstein, A.},
  journal={Annalen der Physik},
  year={1905}
}

@book{knuth1984,
  title={The TeXbook},
  author={Knuth, Donald E.},
  year={1984},
  publisher={Addison-Wesley}
}
```

#### 在文档中引用
```latex
\documentclass{article}
\usepackage{natbib}

\begin{document}

引用文献 \cite{einstein1905}。
引用书籍 \citep{knuth1984}。

\bibliographystyle{plain}
\bibliography{references}

\end{document}
```

### 7.2 手动方式

```latex
\begin{thebibliography}{99}
\bibitem{ref1} 作者. 标题. 期刊, 年份.
\bibitem{ref2} 作者. 书名. 出版社, 年份.
\end{thebibliography}
```

---

## 八、工具与资源

### 8.1 在线平台

| 平台 | 链接 | 特点 |
|------|------|------|
| Overleaf | https://www.overleaf.com/ | 在线协作编辑 |

### 8.2 常用宏包

| 宏包 | 用途 |
|------|------|
| amsmath | 数学公式增强 |
| graphicx | 图片插入 |
| hyperref | 超链接支持 |
| geometry | 页面布局 |
| fancyhdr | 页眉页脚 |
| natbib | 参考文献 |
| booktabs | 三线表 |
| listings | 代码高亮 |

---

## 九、编译命令

### 9.1 基本编译

```bash
# PDFLaTeX（英文文档）
pdflatex document.tex

# XeLaTeX（中文文档）
xelatex document.tex

# 生成参考文献
bibtex document

# 编译两次（确保引用正确）
xelatex document.tex
xelatex document.tex
```

### 9.2 清理临时文件

```bash
# 删除辅助文件
rm *.aux *.log *.toc *.lof *.lot *.bbl *.blg
```
