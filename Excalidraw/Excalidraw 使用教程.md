# Excalidraw 插件使用教程

> Obsidian 手绘风格绘图插件

---

## 一、插件简介

Excalidraw 是 Obsidian 中最受欢迎的绘图插件之一，提供：
- 手绘风格的 diagrams
- 无限画布
- 丰富的图形元素
- 双向链接支持
- LaTeX 公式
- 版本控制

---

## 二、快速开始

### 1. 创建第一个绘图

**方法 1**：命令面板
```
Ctrl/Cmd + P → Excalidraw: New Excalidraw Drawing
```

**方法 2**：工具栏
```
点击左侧工具栏的 Excalidraw 图标
```

**方法 3**：快捷键
```
Alt + N（可自定义）
```

### 2. 基础操作

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 选择 | V | 选择和移动元素 |
| 矩形 | R | 绘制矩形 |
| 椭圆 | O | 绘制椭圆 |
| 菱形 | D | 绘制菱形 |
| 直线 | L | 绘制直线 |
| 箭头 | A | 绘制箭头 |
| 自由绘制 | P | 手绘 |
| 文字 | T | 添加文字 |
| 撤销 | Ctrl+Z | 撤销上一步 |
| 重做 | Ctrl+Y | 重做 |
| 复制 | Ctrl+C | 复制选中元素 |
| 粘贴 | Ctrl+V | 粘贴 |
| 删除 | Delete | 删除选中元素 |
| 全选 | Ctrl+A | 全选所有元素 |

---

## 三、核心功能

### 1. 图形工具

#### 基础图形
- **矩形** (R)：方框、容器
- **椭圆** (O)：圆形、椭圆
- **菱形** (D)：决策节点
- **三角形**：特殊标记

#### 线条
- **直线** (L)：连接线
- **箭头** (A)：方向指示
- **曲线**：流畅的连接
- **自由绘制** (P)：手写、标注

#### 文字
- **文字工具** (T)：添加文本
- **双击图形**：在图形内添加文字
- **LaTeX 公式**：`$$E = mc^2$$`

### 2. 样式设置

#### 线条样式
- **实线**：默认
- **虚线**：辅助线、边界
- **手绘**：手绘风格

#### 填充
- **无填充**：透明
- **半透明**：强调
- **实色填充**：背景

#### 颜色
- **描边颜色**：边框颜色
- **背景颜色**：填充颜色
- **文字颜色**：字体颜色

#### 粗细
- **细线**：1px
- **中线**：2px
- **粗线**：4px

### 3. 高级功能

#### 分组
```
选中多个元素 → Ctrl+G → 分组
选中分组 → Ctrl+Shift+G → 取消分组
```

#### 图层
```
选中元素 → Ctrl+] → 上移一层
选中元素 → Ctrl+[ → 下移一层
```

#### 对齐
```
选中多个元素 → 右键 → 对齐方式
- 左对齐
- 居中对齐
- 右对齐
- 顶部对齐
- 垂直居中
- 底部对齐
```

#### 约束
```
绘制时按住 Shift：
- 矩形：正方形
- 椭圆：正圆
- 直线：45° 倍数
```

---

## 四、Obsidian 集成

### 1. 双向链接

**在 Excalidraw 中链接笔记**：
```
[[笔记名称]]
```

**从笔记链接到 Excalidraw**：
```
[[绘图名称.excalidraw]]
```

### 2. 嵌入绘图

**在笔记中嵌入**：
```markdown
![[绘图名称.excalidraw]]
```

### 3. 模板系统

**使用模板**：
```
命令面板 → Excalidraw: Insert Template
```

**常用模板**：
- 流程图模板
- 思维导图模板
- 架构图模板
- UML 模板

### 4. 导出功能

**导出格式**：
- PNG 图片
- SVG 矢量图
- Markdown 嵌入
- PDF

**导出方法**：
```
工具栏 → 导出按钮 → 选择格式
```

---

## 五、效率技巧

### 1. 快捷键大全

#### 绘图快捷键
| 快捷键 | 功能 |
|--------|------|
| V | 选择工具 |
| R | 矩形 |
| O | 椭圆 |
| D | 菱形 |
| L | 直线 |
| A | 箭头 |
| P | 自由绘制 |
| T | 文字 |
| E | 橡皮擦 |
| K | 裁剪 |

#### 编辑快捷键
| 快捷键 | 功能 |
|--------|------|
| Ctrl+Z | 撤销 |
| Ctrl+Y | 重做 |
| Ctrl+C | 复制 |
| Ctrl+V | 粘贴 |
| Ctrl+D | 复制并粘贴 |
| Ctrl+G | 分组 |
| Ctrl+Shift+G | 取消分组 |
| Delete | 删除 |
| Ctrl+A | 全选 |
| Space+拖拽 | 平移画布 |
| 滚轮 | 缩放 |

#### 视图快捷键
| 快捷键 | 功能 |
|--------|------|
| Ctrl+0 | 重置缩放 |
| Ctrl++ | 放大 |
| Ctrl+- | 缩小 |
| H | 切换帮助 |
| Ctrl+S | 保存 |

### 2. 高效工作流

#### 流程图绘制
```
1. 用 R 绘制节点（矩形）
2. 用 A 绘制箭头连接
3. 双击节点添加文字
4. 用 Ctrl+G 分组相关元素
5. 调整颜色区分层级
```

#### 架构图绘制
```
1. 用 R 绘制系统边界
2. 用不同颜色区分模块
3. 用 T 添加模块名称
4. 用 A 标注接口关系
5. 用 [[链接]] 关联笔记
```

#### 思维导图
```
1. 中心节点（大椭圆）
2. 分支（箭头或直线）
3. 子节点（矩形）
4. 用颜色区分主题
5. 逐步展开
```

### 3. 实用技巧

**快速复制样式**：
```
1. 选中已设置样式的元素
2. Ctrl+C 复制
3. 选中新元素
4. Ctrl+Shift+V 粘贴样式
```

**精确对齐**：
```
1. 选中多个元素
2. 右键 → 对齐
3. 选择对齐方式
4. 自动对齐
```

**快速缩放**：
```
- 双击画布空白：重置视图
- Space+拖拽：平移
- 滚轮：缩放
- Ctrl+0：适应屏幕
```

---

## 六、推荐脚本和模板

### 1. 自动化脚本

#### 脚本 1：快速创建流程图
```javascript
// 文件名：flowchart-template.js
// 用途：一键创建流程图模板

const canvas = await ea.getExcalidrawAPI();
const elements = [];

// 创建起始节点
elements.push({
  type: 'rectangle',
  x: 100,
  y: 100,
  width: 150,
  height: 60,
  backgroundColor: '#e3f2fd',
  strokeColor: '#1976d2',
  text: '开始'
});

// 创建处理节点
elements.push({
  type: 'rectangle',
  x: 100,
  y: 250,
  width: 150,
  height: 60,
  backgroundColor: '#fff3e0',
  strokeColor: '#f57c00',
  text: '处理'
});

// 创建决策节点
elements.push({
  type: 'diamond',
  x: 100,
  y: 400,
  width: 150,
  height: 80,
  backgroundColor: '#e8f5e9',
  strokeColor: '#388e3c',
  text: '判断'
});

// 添加连接箭头
elements.push({
  type: 'arrow',
  startX: 175,
  startY: 160,
  endX: 175,
  endY: 250
});

elements.push({
  type: 'arrow',
  startX: 175,
  startY: 310,
  endX: 175,
  endY: 400
});

await canvas.addAllElements(elements);
```

#### 脚本 2：快速创建思维导图
```javascript
// 文件名：mindmap-template.js
// 用途：一键创建思维导图中心

const canvas = await ea.getExcalidrawAPI();
const elements = [];

// 中心主题
elements.push({
  type: 'ellipse',
  x: 400,
  y: 300,
  width: 200,
  height: 100,
  backgroundColor: '#ffeb3b',
  strokeColor: '#f57f17',
  strokeWidth: 3,
  text: '中心主题',
  fontSize: 24
});

// 6 个分支
const branches = [
  { x: 150, y: 150, text: '分支 1' },
  { x: 400, y: 100, text: '分支 2' },
  { x: 650, y: 150, text: '分支 3' },
  { x: 150, y: 450, text: '分支 4' },
  { x: 400, y: 500, text: '分支 5' },
  { x: 650, y: 450, text: '分支 6' }
];

branches.forEach(branch => {
  elements.push({
    type: 'rectangle',
    x: branch.x,
    y: branch.y,
    width: 120,
    height: 50,
    backgroundColor: '#e1f5fe',
    strokeColor: '#0288d1',
    text: branch.text
  });
  
  elements.push({
    type: 'arrow',
    startX: 400,
    startY: 300,
    endX: branch.x + 60,
    endY: branch.y + 25
  });
});

await canvas.addAllElements(elements);
```

### 2. 常用模板

#### 模板 1：系统架构图
```markdown
用途：绘制软件系统架构
元素：
- 矩形：模块/组件
- 椭圆：外部系统
- 菱形：数据库
- 箭头：依赖关系
颜色方案：
- 蓝色：核心模块
- 绿色：外部依赖
- 橙色：数据层
```

#### 模板 2：流程图
```markdown
用途：业务流程、算法流程
元素：
- 椭圆：开始/结束
- 矩形：处理步骤
- 菱形：判断节点
- 箭头：流程方向
颜色方案：
- 绿色：开始
- 蓝色：处理
- 黄色：判断
- 红色：结束
```

#### 模板 3：UML 类图
```markdown
用途：面向对象设计
元素：
- 矩形：类（分三层）
  - 类名
  - 属性
  - 方法
- 箭头：关系
  - 继承（空心三角）
  - 实现（虚线三角）
  - 关联（实线）
  - 依赖（虚线）
```

#### 模板 4：时间线
```markdown
用途：项目计划、历史事件
元素：
- 直线：时间轴
- 圆点：时间节点
- 矩形：事件描述
- 箭头：时间方向
布局：
- 水平或垂直
- 等间距分布
```

---

## 七、插件设置优化

### 1. 推荐设置

**打开设置**：
```
设置 → 第三方插件 → Excalidraw
```

**基础设置**：
- [x] 自动保存
- [x] 嵌入时显示工具栏
- [x] 使用 Obsidian 主题色
- [x] 启用 LaTeX 支持

**高级设置**：
- 默认画布大小：2000x2000
- 默认字体：系统默认
- 网格间距：20px
- 导出质量：高质量

### 2. 快捷键自定义

**在 Obsidian 中设置**：
```
设置 → 快捷键 → 搜索 Excalidraw
```

**推荐自定义**：
- 新建绘图：Alt+N
- 插入模板：Alt+T
- 导出图片：Alt+E
- 切换黑暗模式：Alt+D

---

## 八、常见问题

### Q1: 如何插入 Obsidian 笔记链接？
**A**: 使用文字工具 (T)，输入 `[[笔记名称]]`

### Q2: 如何导出为图片？
**A**: 工具栏 → 导出按钮 → 选择 PNG/SVG

### Q3: 如何在笔记中嵌入绘图？
**A**: 使用 `![[绘图名称.excalidraw]]`

### Q4: 如何对齐元素？
**A**: 选中多个元素 → 右键 → 对齐方式

### Q5: 如何复制样式？
**A**: 复制元素 → 选中目标 → Ctrl+Shift+V 粘贴样式

### Q6: 画布太大卡顿怎么办？
**A**: 
- 减少元素数量
- 分组复杂图形
- 使用简化模式
- 定期清理未使用元素

---

## 九、学习资源

### 官方资源
- Excalidraw 官网：https://excalidraw.com/
- Obsidian 插件页：https://obsidian.md/plugins?id=obsidian-excalidraw-plugin
- GitHub：https://github.com/zsviczian/obsidian-excalidraw-plugin

### 教程推荐
- 官方文档
- B 站教程搜索"Obsidian Excalidraw"
- YouTube 教程

### 社区
- Obsidian 论坛
- GitHub Issues
- Discord 社区

---

## 十、最佳实践

### 1. 组织管理
- 使用文件夹分类绘图
- 命名规范：`类型-主题-日期`
- 定期清理未使用绘图

### 2. 版本控制
- 使用 Obsidian Git 备份
- 重要绘图导出 PNG 备份
- 保留历史版本

### 3. 复用策略
- 创建个人模板库
- 常用元素保存为模板
- 建立样式规范

### 4. 协作分享
- 导出 SVG 分享
- 嵌入笔记中展示
- 使用链接关联相关笔记

---
