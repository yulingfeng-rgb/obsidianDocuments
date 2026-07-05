# Mermaid 教程

> 代码生成图表工具，适合 Markdown 文档

---

## 目录

- [介绍](#一介绍)
- [安装配置](#二安装配置)
- [基本语法](#三基本语法)
- [图表类型](#四图表类型)
- [高级功能](#五高级功能)
- [工具与资源](#六工具与资源)

---

## 一、介绍

### 1.1 什么是 Mermaid

Mermaid 是一个基于 JavaScript 的图表绘制工具，通过文本描述生成图表。

**特点**：
- 代码生成图表
- 版本控制友好
- Markdown 嵌入
- 自动化生成

**应用场景**：
- Markdown 文档嵌入
- Git 版本控制
- Docs as Code 工作流
- 自动化生成
- 团队协作文档

### 1.2 与其他工具对比

| 工具 | 优势 | 劣势 |
|------|------|------|
| Mermaid | 代码生成、版本控制、Git 友好 | 定制性有限 |
| Figma | 协作强、美观 | 需要网络、手动绘制 |
| draw.io | 免费、功能强 | 版本控制弱 |
| Excalidraw | 手绘风格 | 功能简单 |

---

## 二、安装配置

### 2.1 在线使用

```
网址：https://mermaid.live/
特点：无需安装，在线编辑和预览
```

### 2.2 VS Code 配置

```
安装扩展：
- Markdown Preview Mermaid Support
- Mermaid Markdown Syntax Highlighting

配置 settings.json：
{
  "markdown.preview.mermaid.theme": "default"
}
```

### 2.3 Obsidian 配置

```
Obsidian 原生支持 Mermaid
直接在 Markdown 中使用即可
```

---

## 三、基本语法

### 3.1 代码块

````markdown
```mermaid
图表代码
```
````

### 3.2 方向设置

```mermaid
graph TD  % 从上到下
graph LR  % 从左到右
graph RL  % 从右到左
graph BT  % 从下到上
```

### 3.3 节点定义

```mermaid
graph TD
    A[矩形节点]
    B(圆角节点)
    C{菱形节点}
    D((圆形节点))
    E>旗帜节点]
```

### 3.4 连接方式

```mermaid
graph TD
    A --> B      % 箭头
    A --- B      % 无箭头
    A -.-> B     % 虚线箭头
    A -.-> B     % 虚线无箭头
    A ==> B      % 粗箭头
    A === B      % 粗无箭头
```

### 3.5 文本标签

```mermaid
graph TD
    A -->|标签| B
    A --> B
    A -->|多行
    标签| C
```

---

## 四、图表类型

### 4.1 流程图

```mermaid
graph TD
    A[开始] --> B[步骤1]
    B --> C{判断}
    C -->|是| D[步骤2]
    C -->|否| E[步骤3]
    D --> F[结束]
    E --> F
```

### 4.2 时序图

```mermaid
sequenceDiagram
    participant A
    participant B
    A->>B: 消息
    B-->>A: 回复
```

### 4.3 状态图

```mermaid
stateDiagram-v2
    [*] --> 状态1
    状态1 --> 状态2
    状态2 --> [*]
```

### 4.4 类图

```mermaid
classDiagram
    class Animal {
        +String name
        +makeSound()
    }
    class Dog {
        +fetch()
    }
    Animal <|-- Dog
```

### 4.5 甘特图

```mermaid
gantt
    title 项目计划
    dateFormat  YYYY-MM-DD
    section 阶段1
    任务1 :a1, 2026-01-01, 30d
    任务2 :after a1, 20d
```

### 4.6 饼图

```mermaid
pie title 数据分布
    "A" : 40
    "B" : 30
    "C" : 20
    "D" : 10
```

### 4.7 Git 图

```mermaid
gitGraph
    commit
    branch develop
    checkout develop
    commit
    checkout main
    merge develop
    commit
```

### 4.8 ER 图

```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ LINE-ITEM : contains
```

---

## 五、高级功能

### 5.1 子图

```mermaid
graph TD
    subgraph 子系统1
        A1 --> A2
    end
    subgraph 子系统2
        B1 --> B2
    end
    A2 --> B1
```

### 5.2 样式设置

```mermaid
graph TD
    A --> B
    style A fill:#f9f,stroke:#333
    style B fill:#bbf,stroke:#f66
```

### 5.3 点击事件

```mermaid
graph TD
    A[点击我] --> B
    click A callback "提示"
```

---

## 六、工具与资源

### 6.1 在线工具

| 工具 | 链接 | 说明 |
|------|------|------|
| Mermaid Live | https://mermaid.live/ | 在线编辑器 |
| GitHub | https://github.com/mermaid-js/mermaid | 官方仓库 |

### 6.2 学习资源

| 资源 | 链接 |
|------|------|
| 官方文档 | https://mermaid-js.github.io/mermaid/ |
| 语法参考 | https://mermaid-js.github.io/mermaid/#/ |
