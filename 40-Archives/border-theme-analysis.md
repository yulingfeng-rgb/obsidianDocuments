# Border 主题分析

> 本文档解析 Border 主题的 CSS 架构、设计思路和关键技术

---

## 一、整体架构

Border 主题的 CSS 文件 (~9200 行) 分为两大区域：

| 区域 | 行数范围 | 内容 |
|------|----------|------|
| **配置声明区** | 1 ~ 3687 | `/* @settings */` 块，定义 Style Settings 插件的配置面板 |
| **样式实现区** | 3688 ~ 9188 | 实际 CSS 规则，包含颜色、布局、组件样式 |

### 设计哲学

Border 的核心理念是 **"一切皆可配置"**：
- 通过 CSS 变量 + `body` 类名切换实现功能开关
- 所有颜色从单一强调色 (accent color) 派生
- 支持亮/暗模式独立配置
- 提供预设 (presets) 和自定义两种使用方式

---

## 二、颜色系统

### 2.1 HSL 核心模型

Border 的颜色系统完全基于 **HSL 色彩空间**，通过一个强调色派生出整个界面色彩：

```css
/* 默认强调色 (蓝紫色) */
--accent-h: 232;      /* 色相 */
--accent-s: 96%;      /* 饱和度 */
--accent-l: 64%;      /* 亮度 */
```

**派生逻辑**：
- `--color-accent-1` = 色相-1, 饱和度×1.01, 亮度×1.075 (稍亮)
- `--color-accent-2` = 色相-3, 饱和度×1.02, 亮度×1.15 (更亮，用于 hover)
- `--color-accent-3` = 饱和度×0.4 (低饱和，用于边框/次要元素)

### 2.2 文本颜色

文本颜色同样从强调色色相派生，保持整体色调统一：

```css
/* 亮色模式 */
--text-normal: hsl(var(--accent-h), 6%, 12%);   /* 带色相倾向的黑色 */
--text-muted: hsl(var(--accent-h), 9%, 36%);    /* 带色相倾向的灰色 */
--text-faint: hsl(var(--accent-h), 12%, 64%);   /* 带色相倾向的浅灰 */

/* 暗色模式 */
--text-normal: hsl(var(--accent-h), 15%, 85%);  /* 带色相倾向的白色 */
--text-muted: hsl(var(--accent-h), 12%, 75%);
--text-faint: hsl(var(--accent-h), 9%, 50%);
```

**设计要点**：所有文本都带有微弱的主题色倾向，避免纯黑/纯白，让界面更和谐。

### 2.3 背景颜色

背景色通过 HSL 计算，与强调色保持微妙关联：

```css
/* 亮色模式 - 默认风格 */
--background-primary: #ffffff;                    /* 纯白 */
--background-secondary: hsl(accent-h, s/6, ...);  /* 极淡的主题色 */
--background-tertiary: hsl(accent-h, s/1.5, ...); /* 稍深的主题色 */

/* 暗色模式 - 默认风格 */
--background-primary: hsl(accent-h, s/5, ...);    /* 带色相的深灰 */
```

### 2.4 边框颜色

边框使用半透明的强调色，实现"融入但不突兀"的效果：

```css
--background-modifier-border: hsla(accent-h, 0.4*s, l, 0.2);      /* 20% 透明 */
--background-modifier-border-hover: hsla(accent-h, 0.4*s, l, 0.25); /* 25% 透明 */
--background-modifier-border-focus: hsla(accent-h, 0.4*s, l, 0.3);  /* 30% 透明 */
```

### 2.5 暗色模式变体

提供 4 种暗色风格，通过 body 类名切换：

| 类名 | 风格 | 特点 |
|------|------|------|
| `theme-dark-background-default` | 默认 | 中等亮度 |
| `theme-dark-background-brighter` | 较亮 | 适合 OLED 以外屏幕 |
| `theme-dark-background-darker` | 较暗 | 更深沉 |
| `theme-dark-background-black` | 纯黑 | AMOLED 风格，纯黑背景 |

---

## 三、布局系统

### 3.1 双布局模式

Border 提供两种布局：

**默认布局**：
- 工作区整体有圆角 + 阴影边框
- 通过 `::before` 伪元素创建全局阴影效果
- 面板之间用 border 分隔

```css
/* 默认布局阴影 */
--card-shadow-light: 0 0 0 1px var(--border-muted), 0 0 6px var(--border-muted);
--card-shadow-dark: 0 -1px 0 0 var(--border), 0 0 0 1px var(--border);
```

**卡片布局** (`card-layout-open`)：
- 每个面板独立圆角 + 阴影
- 面板之间有 16px 间距
- 底层背景可见，形成"卡片浮于背景"效果

```css
.workspace-tab-container {
    border-radius: 8px !important;
    overflow: hidden !important;
    box-shadow: var(--card-shadow);
}
```

### 3.2 面板背景独立控制

三个面板 (左/中/右) 可独立设置背景：
- primary (默认，跟随主背景)
- secondary (跟随次要背景)
- transparent (透明，仅卡片布局)
- customize (自定义颜色)
- customize CSS (自定义 CSS 背景，支持渐变+模糊)

---

## 四、关键技术

### 4.1 CSS 变量级联

Border 大量使用 CSS 变量级联实现主题切换：

```
body (全局变量)
  └─ .theme-light / .theme-dark (模式变量)
       └─ .theme-light-background-adapt / .theme-dark-background-darker (风格变量)
            └─ .mod-left-split-background-secondary-light (面板变量)
```

### 4.2 类名切换机制

功能开关通过 `body` 上的类名实现：

```css
/* 配置声明 */
body.tab-autohide .workspace-tab-header-container { ... }
body.card-layout-open-light .workspace-tab-container { ... }
body.border-focus-mode .cm-active { ... }
```

### 4.3 条件隐藏设置项

根据当前选择自动隐藏无关设置：

```css
/* 未启用自定义代码块主题时，隐藏相关颜色设置 */
body:not(.codeblock-style-customize) .setting-item[data-id=code-background-light] {
    display: none;
}
```

### 4.4 动画系统

```css
--anim-speed: 1;  /* 可通过设置调节 */
--anim-duration-fast: calc(150ms / var(--anim-speed));
--anim-duration-moderate: calc(300ms / var(--anim-speed));

/* 弹窗缩放动画 */
@keyframes scale-up {
    0% { transform: scale(0.75); opacity: 0; }
    100% { transform: scale(1); opacity: 1; }
}
```

支持 `prefers-reduced-motion` 媒体查询，尊重用户系统设置。

### 4.5 滚动条

```css
/* 细滚动条，悬停时才显示 */
::-webkit-scrollbar { width: 11px; }
::-webkit-scrollbar-thumb {
    border: 3px solid transparent;  /* 透明边框实现视觉细度 */
    border-radius: 20px;
}
::-webkit-scrollbar-track { background: transparent; }
:not(.is-mobile) :hover::-webkit-scrollbar-thumb {
    background-color: rgba(0,0,0,0.1);  /* 悬停时才可见 */
}
```

---

## 五、Style Settings 配置体系

Border 的配置面板分为 6 大类：

| 配置类 | 功能 |
|--------|------|
| **Components** | 自动隐藏、文件列表、大纲、新标签页、图谱、白板、动效 |
| **Appearance (light)** | 亮色模式：卡片布局、色彩、背景、前景、卡片、激活态、UI 控件 |
| **Appearance (dark)** | 暗色模式：同上，额外支持 darker/brighter/black 风格 |
| **Editor** | 焦点指示器、专注模式、背景图案、标题 H1-H6、段落、文本、链接、列表、标签、表格、图片、代码块、引用、Callout、嵌入、复选框 |
| **Mobile** | 平板卡片布局、手机侧边抽屉全屏 |
| **Plugin** | DB Folder、Projects、Checklist 插件适配 |

### 配置项类型

| 类型 | 用途 |
|------|------|
| `class-toggle` | 开关功能，添加/移除 body 类名 |
| `class-select` | 多选一，添加对应类名 |
| `variable-color` | 自定义颜色，生成 CSS 变量 |
| `variable-themed-color` | 分亮/暗模式的颜色 |
| `variable-text` | 自定义 CSS 值 (如 border-radius) |
| `variable-number` | 数字值 (如 font-size) |
| `variable-number-slider` | 滑块数字 (0-1) |

---

## 六、设计亮点总结

1. **统一色调**：所有颜色从单一 HSL 强调色派生，保证视觉和谐
2. **半透明边框**：用 `hsla` 低透明度做边框，融入背景而不突兀
3. **微色相偏移**：文本/背景都带微弱主题色倾向，避免死板的纯黑白
4. **卡片/默认双布局**：一套代码支持两种完全不同的 UI 风格
5. **面板独立背景**：左/中/右面板可独立定制背景，支持毛玻璃效果
6. **渐进式自定义**：从预设 → 调色 → 自定义 CSS，满足不同层次用户
7. **性能友好**：动画支持 `prefers-reduced-motion`，滚动条悬停显示减少渲染压力

---

## 七、对 CSS Snippet 开发的启示

基于 Border 的设计逻辑，编写 CSS 片段时应注意：

1. **使用 CSS 变量**：`var(--interactive-accent)` 等，自动跟随主题色
2. **使用 `color-mix()`**：`color-mix(in srgb, var(--accent) 10%, transparent)` 创建半透明效果
3. **圆角统一**：Border 默认 8px 卡片圆角，4-6px 控件圆角
4. **阴影层次**：使用多层阴影模拟真实光影 (3 层 shadow-s, 4 层 shadow-l)
5. **亮暗模式适配**：用 `.theme-light` / `.theme-dark` 分别处理
6. **避免硬编码颜色**：始终使用 `var(--text-normal)` 等变量
