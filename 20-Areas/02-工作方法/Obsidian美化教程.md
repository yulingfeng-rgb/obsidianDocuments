# Obsidian 美化教程

> 本教程面向已完成字体安装和插件安装的 Obsidian 用户，通过 4 步配置（Style Settings、Iconize、Templater、CSS Snippet）实现界面美化。配套插件详细设置见 [Obsidian 插件设置手册](Obsidian插件设置手册.md)。

**关键词**：Obsidian、界面美化、Style Settings、CSS Snippet、主题定制

## 抽象类比

Obsidian 美化就像**装修一套毛坯房**。Markdown 笔记是房屋结构，插件是家具家电，而美化就是给墙面刷漆、换灯具、铺地板——让住起来更舒服、看着更顺眼。本教程带你走完"软装"全流程：调色板（Style Settings）→ 装饰图标（Iconize）→ 模板工艺（Templater）→ 自定义墙纸（CSS Snippet）。

---

## 总览：4 步完成美化

```mermaid
flowchart LR
    A[Step 1<br/>配置 Style Settings] --> B[Step 2<br/>配置 Iconize]
    B --> C[Step 3<br/>配置 Templater]
    C --> D[Step 4<br/>写 CSS Snippet]
    
    style A fill:#e1f5ff
    style D fill:#e8f5e9
```

**预计耗时**：15-20 分钟

---

## Step 1：配置 Style Settings（5 分钟）

### 打开方式

`设置` → 左侧滑到最下面 → `Style Settings`

### 必开项（3 个最关键的开关）

| 位置 | 英文选项 | 中文含义 | 操作 |
|------|----------|---------|------|
| Components → Auto hide | Auto hide tab bar | 自动隐藏标签栏 | ✅ 开 |
| Components → Auto hide | Auto hide bottom status bar | 自动隐藏状态栏 | ✅ 开 |
| Components → File explorer | Bold folder font | 加粗文件夹字体 | ✅ 开 |
| Components → File explorer | Enable colorful folder icon | 多彩文件夹图标 | ✅ 开 |

### 卡片布局（Border 的核心特色）

| 位置 | 英文选项 | 中文含义 | 操作 |
|------|----------|---------|------|
| Appearance (light) | Enable card layout | 启用卡片布局（亮色） | ✅ 开 |
| Appearance (dark) | Enable card layout | 启用卡片布局（暗色） | ✅ 开 |

### 编辑器美化

| 位置 | 英文选项 | 中文含义 | 操作 |
|------|----------|---------|------|
| Editor | Hover line indicator | 悬停行指示器 | ✅ 开 |
| Editor | Table → Enable table style | 表格美化 | ✅ 开 |
| Editor | Image → Image border radius | 图片圆角 | `8px` |

> 完整的中英对照和所有选项说明，见 [插件设置手册 → Style Settings](Obsidian插件设置手册.md#一、style-settings：border-主题配置)

### 保存配置

点击右上角 **Export** → 保存到 `40-Archives/style-settings-backup.json`

### 检查效果

- ✅ 面板变成卡片浮于背景
- ✅ 不用的标签栏/状态栏自动隐藏
- ✅ 文件夹名字加粗，图标有颜色
- ✅ 表格有圆角和斑马纹

---

## Step 2：配置 Iconize（3 分钟）

### 安装图标包

`设置` → `Iconize` → `Icon packs` → `Add icon pack` → 选 `Lucide`

### 给文件夹加图标

**右键** 文件夹 → `Change Icon` → 搜索关键词：

| 文件夹 | 搜索关键词 |
|--------|-----------|
| `00-Inbox` | `inbox` |
| `10-Projects` | `rocket` |
| `20-Areas` | `book-open` |
| `30-Resources` | `package` |
| `40-Archives` | `archive` |

### 检查效果

文件树中每个文件夹前面都有图标，一目了然。

> 完整设置见 [插件设置手册 → Iconize](Obsidian插件设置手册.md#二、iconize：文件夹图标)

---

## Step 3：配置 Templater（5 分钟）

### 基础设置

`设置` → `Templater`

| 设置项 | 设置值 |
|--------|--------|
| Template folder location | `20-Areas/Templates` |
| Trigger Templater on new file creation | ✅ 开 |
| Automatic jump to cursor | ✅ 开 |

### 创建模板文件夹

在 `20-Areas` 下创建 `Templates` 文件夹

### 创建模板

在 `Templates` 文件夹下创建 3 个模板文件，内容见 [插件设置手册 → Templater](Obsidian插件设置手册.md#三、templater：模板引擎)：

| 模板文件 | 用途 |
|---------|------|
| `daily.md` | 每日笔记 |
| `concept.md` | 概念学习 |
| `project.md` | 项目笔记 |

### 配置文件夹自动模板

`设置` → `Templater` → `Folder Templates` → 开启 → 添加：

| 文件夹 | 模板 |
|--------|------|
| `30-Resources/概念学习` | `20-Areas/Templates/concept.md` |
| `10-Projects` | `20-Areas/Templates/project.md` |

### 检查效果

在 `概念学习` 文件夹下新建笔记 → 自动套用概念学习模板。

---

## Step 4：创建 CSS Snippet（3 分钟）

### 打开 snippets 文件夹

`设置` → `外观` → `CSS 代码片段` → 点击文件夹图标

### 创建微调文件

新建文件 `polish.css`，写入：

```css
/* 阅读模式行距优化 */
.markdown-preview-view { line-height: 1.9; }
.markdown-preview-view p { margin-bottom: 1.2em; }

/* 分割线渐变 */
.markdown-preview-view hr {
    border: none; height: 2px;
    background: linear-gradient(to right, transparent, var(--interactive-accent), transparent);
    margin: 2em 0;
}

/* 图片圆角 */
.markdown-preview-view img {
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    margin: 1em auto; display: block;
}

/* 标签圆角 */
.tag { border-radius: 12px; padding: 2px 8px; font-size: 0.85em; }

/* 高亮圆角 */
.markdown-preview-view mark { border-radius: 3px; padding: 1px 3px; }
```

### 启用

回到 `设置` → `外观` → `CSS 代码片段` → 点刷新 → 开启 `polish`

### 检查效果

打开任意笔记 → `Ctrl+E` 切换阅读模式：
- ✅ 行距更宽松
- ✅ 分割线变成渐变色
- ✅ 图片有圆角和阴影
- ✅ 标签变成圆角徽章

---

## 完成检查

- [ ] Style Settings：卡片布局已开启，自动隐藏已设置
- [ ] Iconize：5 个文件夹都有图标
- [ ] Templater：模板文件夹已建，自动模板已配
- [ ] CSS Snippet：polish.css 已启用
- [ ] 导出 Style Settings 配置备份

---

## 下一步

- **学习 Dataview**：见 [插件设置手册 → Dataview](Obsidian插件设置手册.md#五、dataview：数据查询)
- **学习 Excalidraw**：见 [插件设置手册 → Excalidraw](Obsidian插件设置手册.md#四、excalidraw：手绘图)
- **按需安装更多插件**：见 [Obsidian 美化指南](Obsidian美化指南.md)
- **Fork Border 主题**：见 [Border 主题开发计划](file:///e:/obsidianDocument/40-Archives/border-modify-plan.md)
