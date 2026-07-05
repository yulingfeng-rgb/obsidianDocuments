# Excalidraw 脚本和模板集合

> 提升 Excalidraw 使用效率的脚本和模板
> 最后更新：2026-06-25

---

## 一、Excalidraw Scripts 使用说明

### 如何安装脚本

1. 在 Obsidian 中创建文件夹：`Excalidraw/Scripts`
2. 将下面的脚本保存为 `.js` 文件
3. 在 Excalidraw 画布中：`命令面板 → Excalidraw: Execute Excalidraw Script`
4. 选择脚本执行

### 脚本目录结构

```
Excalidraw/
└── Scripts/
    ├── flowchart-template.js      # 流程图模板
    ├── mindmap-template.js        # 思维导图模板
    ├── architecture-template.js   # 架构图模板
    ├── uml-template.js            # UML 模板
    └── timeline-template.js       # 时间线模板
```

---

## 二、实用脚本

### 脚本 1：流程图模板生成器

**文件**：`flowchart-template.js`

**功能**：一键生成标准流程图模板

```javascript
// Excalidraw 流程图模板脚本
// 用法：在 Excalidraw 画布中执行此脚本

const ea = ExcalidrawAutomate;
ea.reset();

// 设置样式
ea.style.strokeColor = "#1e1e1e";
ea.style.backgroundColor = "transparent";
ea.style.strokeWidth = 2;
ea.style.fontSize = 16;

// 创建开始节点
ea.addText(400, 100, "开始", {
  width: 120,
  height: 50,
  backgroundColor: "#c8e6c9",
  strokeColor: "#2e7d32",
  textAlign: "center"
});

// 创建处理节点
ea.addText(400, 250, "处理步骤 1", {
  width: 150,
  height: 60,
  backgroundColor: "#bbdefb",
  strokeColor: "#1565c0",
  textAlign: "center"
});

ea.addText(400, 400, "处理步骤 2", {
  width: 150,
  height: 60,
  backgroundColor: "#bbdefb",
  strokeColor: "#1565c0",
  textAlign: "center"
});

// 创建决策节点
ea.addText(400, 550, "判断条件？", {
  width: 150,
  height: 80,
  shape: "diamond",
  backgroundColor: "#fff9c4",
  strokeColor: "#f9a825",
  textAlign: "center"
});

// 创建结束节点
ea.addText(400, 750, "结束", {
  width: 120,
  height: 50,
  backgroundColor: "#ffcdd2",
  strokeColor: "#c62828",
  textAlign: "center"
});

// 添加连接箭头
ea.addArrow(
  {x: 460, y: 150},
  {x: 460, y: 250},
  {strokeColor: "#1e1e1e", strokeWidth: 2}
);

ea.addArrow(
  {x: 460, y: 310},
  {x: 460, y: 400},
  {strokeColor: "#1e1e1e", strokeWidth: 2}
);

ea.addArrow(
  {x: 460, y: 460},
  {x: 460, y: 550},
  {strokeColor: "#1e1e1e", strokeWidth: 2}
);

// 是分支
ea.addArrow(
  {x: 460, y: 630},
  {x: 460, y: 750},
  {strokeColor: "#2e7d32", strokeWidth: 2}
);

ea.addText(480, 680, "是", {
  fontSize: 14,
  strokeColor: "#2e7d32"
});

// 否分支（循环回去）
ea.addArrow(
  {x: 550, y: 590},
  {x: 650, y: 590},
  {strokeColor: "#c62828", strokeWidth: 2}
);

ea.addArrow(
  {x: 650, y: 590},
  {x: 650, y: 280},
  {strokeColor: "#c62828", strokeWidth: 2}
);

ea.addArrow(
  {x: 650, y: 280},
  {x: 550, y: 280},
  {strokeColor: "#c62828", strokeWidth: 2}
);

ea.addText(570, 570, "否", {
  fontSize: 14,
  strokeColor: "#c62828"
});

await ea.create();
```

---

### 脚本 2：思维导图生成器

**文件**：`mindmap-template.js`

**功能**：一键生成中心辐射型思维导图

```javascript
// Excalidraw 思维导图模板脚本

const ea = ExcalidrawAutomate;
ea.reset();

// 中心主题
ea.addText(500, 350, "中心主题", {
  width: 200,
  height: 80,
  shape: "ellipse",
  backgroundColor: "#fff59d",
  strokeColor: "#f57f17",
  strokeWidth: 3,
  fontSize: 24,
  textAlign: "center"
});

// 分支数据
const branches = [
  { x: 200, y: 150, text: "分支主题 1", color: "#e3f2fd" },
  { x: 500, y: 100, text: "分支主题 2", color: "#f3e5f5" },
  { x: 800, y: 150, text: "分支主题 3", color: "#e8f5e9" },
  { x: 200, y: 550, text: "分支主题 4", color: "#fff3e0" },
  { x: 500, y: 600, text: "分支主题 5", color: "#fce4ec" },
  { x: 800, y: 550, text: "分支主题 6", color: "#e0f2f1" }
];

// 创建分支
branches.forEach(branch => {
  // 分支节点
  ea.addText(branch.x, branch.y, branch.text, {
    width: 140,
    height: 50,
    backgroundColor: branch.color,
    strokeColor: "#1e1e1e",
    strokeWidth: 2,
    fontSize: 18,
    textAlign: "center"
  });
  
  // 连接线
  ea.addArrow(
    {x: 500, y: 390},
    {x: branch.x + 70, y: branch.y + 50},
    {strokeColor: "#1e1e1e", strokeWidth: 2}
  );
});

await ea.create();
```

---

### 脚本 3：系统架构图生成器

**文件**：`architecture-template.js`

**功能**：生成三层系统架构模板

```javascript
// Excalidraw 系统架构图模板

const ea = ExcalidrawAutomate;
ea.reset();

// 表示层
ea.addText(400, 100, "表示层 (Presentation)", {
  width: 600,
  height: 80,
  backgroundColor: "#e3f2fd",
  strokeColor: "#1565c0",
  strokeWidth: 3,
  fontSize: 20,
  textAlign: "center"
});

ea.addText(200, 220, "Web UI", {
  width: 120,
  height: 50,
  backgroundColor: "#bbdefb",
  strokeColor: "#1565c0",
  textAlign: "center"
});

ea.addText(400, 220, "Mobile App", {
  width: 120,
  height: 50,
  backgroundColor: "#bbdefb",
  strokeColor: "#1565c0",
  textAlign: "center"
});

ea.addText(600, 220, "API Gateway", {
  width: 120,
  height: 50,
  backgroundColor: "#bbdefb",
  strokeColor: "#1565c0",
  textAlign: "center"
});

// 业务逻辑层
ea.addText(400, 350, "业务逻辑层 (Business Logic)", {
  width: 600,
  height: 80,
  backgroundColor: "#e8f5e9",
  strokeColor: "#2e7d32",
  strokeWidth: 3,
  fontSize: 20,
  textAlign: "center"
});

ea.addText(200, 470, "服务 A", {
  width: 120,
  height: 50,
  backgroundColor: "#c8e6c9",
  strokeColor: "#2e7d32",
  textAlign: "center"
});

ea.addText(400, 470, "服务 B", {
  width: 120,
  height: 50,
  backgroundColor: "#c8e6c9",
  strokeColor: "#2e7d32",
  textAlign: "center"
});

ea.addText(600, 470, "服务 C", {
  width: 120,
  height: 50,
  backgroundColor: "#c8e6c9",
  strokeColor: "#2e7d32",
  textAlign: "center"
});

// 数据层
ea.addText(400, 600, "数据层 (Data)", {
  width: 600,
  height: 80,
  backgroundColor: "#fff3e0",
  strokeColor: "#e65100",
  strokeWidth: 3,
  fontSize: 20,
  textAlign: "center"
});

ea.addText(200, 720, "数据库", {
  width: 120,
  height: 50,
  shape: "cylinder",
  backgroundColor: "#ffe0b2",
  strokeColor: "#e65100",
  textAlign: "center"
});

ea.addText(400, 720, "缓存", {
  width: 120,
  height: 50,
  shape: "cylinder",
  backgroundColor: "#ffe0b2",
  strokeColor: "#e65100",
  textAlign: "center"
});

ea.addText(600, 720, "消息队列", {
  width: 120,
  height: 50,
  shape: "cylinder",
  backgroundColor: "#ffe0b2",
  strokeColor: "#e65100",
  textAlign: "center"
});

// 层间连接
for (let x of [260, 460, 660]) {
  ea.addArrow(
    {x: x, y: 270},
    {x: x, y: 350},
    {strokeColor: "#1e1e1e", strokeWidth: 2}
  );
  
  ea.addArrow(
    {x: x, y: 520},
    {x: x, y: 600},
    {strokeColor: "#1e1e1e", strokeWidth: 2}
  );
}

await ea.create();
```

---

### 脚本 4：UML 类图生成器

**文件**：`uml-template.js`

**功能**：生成 UML 类图模板

```javascript
// Excalidraw UML 类图模板

const ea = ExcalidrawAutomate;
ea.reset();

// 类 1
ea.addText(200, 100, "ClassA", {
  width: 200,
  height: 30,
  backgroundColor: "#e3f2fd",
  strokeColor: "#1565c0",
  strokeWidth: 2,
  fontSize: 18,
  textAlign: "center",
  fontWeight: "bold"
});

ea.addText(200, 130, "- field1: string\n- field2: int", {
  width: 200,
  height: 50,
  backgroundColor: "#e3f2fd",
  strokeColor: "#1565c0",
  fontSize: 14,
  textAlign: "left"
});

ea.addText(200, 180, "+ method1(): void\n+ method2(): int", {
  width: 200,
  height: 50,
  backgroundColor: "#e3f2fd",
  strokeColor: "#1565c0",
  fontSize: 14,
  textAlign: "left"
});

// 类 2
ea.addText(600, 100, "ClassB", {
  width: 200,
  height: 30,
  backgroundColor: "#e8f5e9",
  strokeColor: "#2e7d32",
  strokeWidth: 2,
  fontSize: 18,
  textAlign: "center",
  fontWeight: "bold"
});

ea.addText(600, 130, "- field3: boolean", {
  width: 200,
  height: 30,
  backgroundColor: "#e8f5e9",
  strokeColor: "#2e7d32",
  fontSize: 14,
  textAlign: "left"
});

ea.addText(600, 160, "+ method3(): string", {
  width: 200,
  height: 30,
  backgroundColor: "#e8f5e9",
  strokeColor: "#2e7d32",
  fontSize: 14,
  textAlign: "left"
});

// 继承箭头（空心三角）
ea.addArrow(
  {x: 600, y: 140},
  {x: 400, y: 140},
  {
    strokeColor: "#1e1e1e",
    strokeWidth: 2,
    endArrow: "triangle_outline"
  }
);

ea.addText(500, 120, "继承", {
  fontSize: 14,
  strokeColor: "#1e1e1e"
});

// 接口
ea.addText(400, 300, "<<Interface>>", {
  width: 200,
  height: 30,
  backgroundColor: "#fff3e0",
  strokeColor: "#e65100",
  strokeWidth: 2,
  fontSize: 16,
  textAlign: "center",
  fontStyle: "italic"
});

ea.addText(400, 330, "+ interfaceMethod(): void", {
  width: 200,
  height: 30,
  backgroundColor: "#fff3e0",
  strokeColor: "#e65100",
  fontSize: 14,
  textAlign: "left"
});

// 实现箭头（虚线）
ea.addArrow(
  {x: 300, y: 210},
  {x: 400, y: 300},
  {
    strokeColor: "#1e1e1e",
    strokeWidth: 2,
    strokeStyle: "dashed",
    endArrow: "triangle_outline"
  }
);

await ea.create();
```

---

### 脚本 5：时间线生成器

**文件**：`timeline-template.js`

**功能**：生成水平时间线

```javascript
// Excalidraw 时间线模板

const ea = ExcalidrawAutomate;
ea.reset();

// 时间轴主线
ea.addLine(
  {x: 100, y: 300},
  {x: 900, y: 300},
  {strokeColor: "#1e1e1e", strokeWidth: 3}
);

// 时间节点
const events = [
  { x: 150, text: "2024-01", desc: "项目启动" },
  { x: 300, text: "2024-03", desc: "需求完成" },
  { x: 450, text: "2024-06", desc: "开发中期" },
  { x: 600, text: "2024-09", desc: "测试阶段" },
  { x: 750, text: "2024-12", desc: "项目上线" }
];

events.forEach((event, index) => {
  // 时间节点圆点
  ea.addEllipse(
    event.x - 10,
    290,
    20,
    20,
    {
      backgroundColor: "#ff7043",
      strokeColor: "#d84315",
      strokeWidth: 2
    }
  );
  
  // 时间标签
  ea.addText(event.x - 40, 250, event.text, {
    width: 80,
    height: 30,
    fontSize: 14,
    textAlign: "center",
    fontWeight: "bold"
  });
  
  // 描述文字
  ea.addText(event.x - 50, 330, event.desc, {
    width: 100,
    height: 40,
    fontSize: 12,
    textAlign: "center"
  });
});

// 箭头
ea.addArrow(
  {x: 900, y: 300},
  {x: 920, y: 300},
  {strokeColor: "#1e1e1e", strokeWidth: 3}
);

await ea.create();
```

---

## 三、Markdown 模板

### 模板 1：会议记录绘图模板

**用途**：快速绘制会议讨论的架构图

```markdown
---
excalidraw-plugin: parsed
tags: [excalidraw]
---

# 会议架构图

## 会议信息
- **日期**：2026-06-25
- **主题**：系统架构讨论
- **参与人**：团队

## 绘图说明
1. 使用矩形表示模块
2. 使用箭头表示依赖关系
3. 用颜色区分层级
   - 蓝色：核心模块
   - 绿色：外部依赖
   - 橙色：数据层

## 相关链接
- [[项目文档]]
- [[技术方案]]
```

### 模板 2：学习笔记绘图模板

**用途**：绘制知识导图

```markdown
---
excalidraw-plugin: parsed
tags: [excalidraw, 学习笔记]
---

# 知识导图

## 主题
[填写主题]

## 核心概念
1. 概念 A
2. 概念 B
3. 概念 C

## 关联笔记
- [[相关笔记 1]]
- [[相关笔记 2]]

## 总结
[学习总结]
```

---

## 四、效率提升技巧

### 1. 使用模板库

**创建个人模板文件夹**：
```
Excalidraw/Templates/
├── flowchart.md
├── mindmap.md
├── architecture.md
└── uml.md
```

**使用方法**：
```
命令面板 → Excalidraw: Insert Template → 选择模板
```

### 2. 自定义样式预设

**保存常用样式**：
```javascript
// 保存到脚本中
const presets = {
  primary: {
    backgroundColor: "#e3f2fd",
    strokeColor: "#1565c0",
    strokeWidth: 2
  },
  success: {
    backgroundColor: "#e8f5e9",
    strokeColor: "#2e7d32",
    strokeWidth: 2
  },
  warning: {
    backgroundColor: "#fff3e0",
    strokeColor: "#e65100",
    strokeWidth: 2
  }
};
```

### 3. 批量操作技巧

**快速复制多个元素**：
```
1. 选中元素
2. Ctrl+C 复制
3. Ctrl+V 粘贴
4. 拖拽到新位置
5. 重复 Ctrl+V
```

**统一修改样式**：
```
1. 选中所有元素（Ctrl+A 或框选）
2. 修改样式（颜色、粗细）
3. 所有选中元素同时更新
```

### 4. 快捷键工作流

**高效绘图流程**：
```
R（矩形）→ 绘制 → T（文字）→ 双击添加文字
→ A（箭头）→ 连接 → V（选择）→ 调整位置
→ Ctrl+S（保存）
```

---

## 五、常见问题解决

### Q1: 脚本执行失败？
**A**: 确保使用最新版本的 Excalidraw 插件

### Q2: 如何自定义脚本？
**A**: 修改脚本中的坐标、颜色、文字等参数

### Q3: 如何分享脚本？
**A**: 将 `.js` 文件分享给其他人

### Q4: 脚本能自动化吗？
**A**: 可以结合 Templater 插件实现自动执行

---

*持续更新中...*
*最后更新：2026-06-25*
