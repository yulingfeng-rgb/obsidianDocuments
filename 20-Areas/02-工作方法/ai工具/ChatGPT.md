# ChatGPT 使用指南

> ChatGPT 是 OpenAI 推出的对话式 AI 助手，开发者可以用它写代码、调 bug、读文档、做数据分析、做技术调研，几乎是日常开发的"瑞士军刀"。本文梳理 ChatGPT 的订阅方案、模型选择、核心用法、API 接入以及国内访问方案。

**关键词**：ChatGPT、OpenAI、对话式 AI、GPT 模型、API 调用

## 抽象类比

ChatGPT 就像口袋里装着的万能助手，或者说一把"瑞士军刀"。问它代码怎么写、bug 怎么调、文档怎么读、数据怎么分析，它都能搭上几句话。和单一用途的工具不同，它什么都懂一点，从写正则到讲架构到画图都能应付，是日常开发的"问路石"和"陪练"。但它不直接动手改你的代码——要真正改代码、跑命令，得请它的兄弟 Codex 或 Claude Code 出马。

## 适合人群 / 预备知识

- 想用 AI 提升开发效率的程序员
- 对订阅制 SaaS 产品有基本了解
- 部分章节涉及 API 调用，需要会使用命令行或写简单脚本

## 一、ChatGPT 是什么

ChatGPT 是 OpenAI 在 2022 年底推出的对话式 AI 产品，底层使用 GPT 系列大模型，可以理解自然语言、生成代码、分析数据、识别图片。

GPT 系列模型演进简述：

- GPT-3.5：2022 年首发，能力有限
- GPT-4：2023 年发布，多模态能力开始成熟
- GPT-4 Turbo：长上下文、知识更新
- GPT-4o：2024 年发布，多模态原生、速度更快
- GPT-4o mini：轻量版，便宜快速
- o1 / o3：推理系列模型，擅长数学、代码、逻辑
- GPT-5：最新一代旗舰模型

## 二、账号与订阅

ChatGPT 提供多个订阅档位，权益和能力差异如下：

- 免费版：可用 GPT-4o mini、有限次 GPT-4o；有使用次数限制；不能访问最新推理模型
- Plus（$20/月）：GPT-4o、o 系列模型更高用量；可创建 GPTs；可使用高级语音、深度搜索
- Pro（$200/月）：Plus 全部权益 + 几乎无限的使用额度；可优先访问最新模型
- Team（$25/人/月起）：Plus 权益 + 更高额度；团队共享工作区；数据默认不用于训练
- Enterprise：定制化方案；SSO、合规、安全审计；适合大型组织

## 三、模型选择

不同模型适用场景：

- GPT-4o：日常通用首选，速度快、多模态强
- GPT-4o mini：简单任务、批量处理，便宜快速
- GPT-5：最新旗舰，复杂推理、长文档、跨模态任务用
- o3：高强度推理任务，如数学证明、复杂算法、深度代码审查
- o3 mini：推理系列的轻量版，速度快

选择原则：先用 GPT-4o，搞不定再切 o3 或 GPT-5；简单任务用 mini 省额度。

## 四、核心使用方式

- 网页版：访问 `chat.openai.com`，最常用
- 手机 App：iOS / Android 均有官方 App，支持语音对话
- 桌面 App：Mac 和 Windows 都有，可与其他应用联动（如读取 IDE 上下文）
- API 调用：访问 `openai.com/platform`，将 ChatGPT 能力集成到自己的程序

## 五、开发者用法

- 代码生成与解释：写函数、写正则、解释一段陌生代码
- Debug 辅助：贴报错信息和上下文，让它分析可能原因
- 文档撰写：写 README、API 文档、注释、提交信息
- 学习新技术：让它讲清楚某个框架的概念、用法、最佳实践
- Code Review：贴代码让它指出潜在 bug、改进点
- 数据分析（Advanced Data Analysis）：上传 CSV/Excel，让它清洗数据、画图、做统计分析

## 六、实用技巧

### 6.1 提示词技巧

- 角色设定：开头先说"你是一个资深后端工程师"
- 分步骤：把复杂任务拆成多步让它一步步做
- 给例子：提供一两个示例输出，让它对齐格式
- 明确输出格式：要求"用 JSON 输出"、"用 Markdown 列表"、"不要超过 200 字"

### 6.2 Custom Instructions

在设置里配置"自定义指令"，告诉 ChatGPT 你的身份、偏好、回答风格，所有新对话都会自动带上，避免每次重复说明。

### 6.3 创建 GPTs

Plus 及以上用户可以创建定制化 GPT，绑定特定的知识库、提示词、API，相当于自己的小助手。比如"我的代码 reviewer"、"SQL 生成器"。

### 6.4 使用 @ 引用

在对话框输入 @ 可以引用上传过的文件、图片，让 ChatGPT 基于这些内容回答，避免重复粘贴。

### 6.5 对话分支管理

点击某条消息可以选择"以此分支继续"，方便对比不同思路，不必每次开新对话。

## 七、常用插件与工具

- 代码解释器（Code Interpreter）：让 ChatGPT 在沙箱里执行 Python 代码、处理文件
- 联网搜索（Web Search）：自动检索最新网页内容，回答时效性问题
- DALL-E：根据文字描述生成图片
- 文件上传：支持 PDF、Word、Excel、图片、代码文件
- 高级语音模式：实时语音对话，适合边走边聊

## 八、API 使用入门

### 8.1 获取 API Key

- 访问 `openai.com/platform` 登录
- 进入 API Keys 页面，创建新 Key（`sk-` 开头）
- 注意保管，Key 只显示一次

### 8.2 调用方式

简单的 curl 示例：

```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-mini",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

### 8.3 计费

API 按使用量计费，单位是 token：

- 输入和输出分别计价
- 不同模型价格差距很大，GPT-4o mini 比 GPT-4o 便宜几十倍
- 在 Billing 页面设置用量上限，避免意外爆账

### 8.4 与 Codex / Claude Code 的关系

- Codex CLI：OpenAI 自家的命令行编程工具，类似 Claude Code，可调用 OpenAI 模型
- Claude Code：Anthropic 出品，命令行编程工具
- 三者关系：ChatGPT 是对话产品，Codex / Claude Code 是命令行编程工具，定位不同但能力有重叠

## 九、国内访问方案

- 科学上网：最直接的方式，但需要稳定的网络环境
- 中转 API：第三方服务商提供 OpenAI 接口的中转，配置 `OPENAI_BASE_URL` 指向中转地址
- 注意事项：
  - 中转 API 有数据安全风险，不要传敏感代码
  - 价格可能比官方便宜也可能更贵，自行甄别
  - 账号注册需要海外手机号或邮箱
  - 不要在多家服务商之间频繁切换 Key，容易触发风控

## 十、与其他 AI 工具的配合

不同工具有不同定位，建议组合使用：

- ChatGPT：通用对话、技术调研、文档撰写、学习新知识
- Codex / Claude Code：在 IDE 或命令行里改代码、跑测试、跨文件重构
- GitHub Copilot：在 IDE 里实时补全代码
- Cursor / Windsurf：AI 原生 IDE，集成度更高

何时用 ChatGPT vs Codex vs Claude Code：

- 想"问问题、查资料、要思路" → ChatGPT
- 想"自动改代码、跑命令、提交 PR" → Codex 或 Claude Code
- 项目代码库大、需要深度上下文 → Claude Code / Codex

## 十一、常见问题

- Q：免费版能用 GPT-4o 吗？
  A：能，但有次数限制，超量后自动降级到 GPT-4o mini。

- Q：Plus 和 Pro 主要差别？
  A：Pro 提供几乎无限的使用额度，并优先访问最新模型，价格贵 10 倍。

- Q：API 和 ChatGPT 订阅是同一个账号吗？
  A：登录是同一套 OpenAI 账号，但 API 计费和订阅计费独立，需要分别充值。

- Q：API 调用报错 "Rate limit"？
  A：是用量超限，可以等一会儿再试，或在账户里提升 tier。

- Q：GPT-5 一定要订阅才能用吗？
  A：免费用户有极少次体验额度，重度使用需要 Plus 及以上订阅。

- Q：中转 API 安全吗？
  A：第三方中转有风险，敏感代码不要走中转，重要的项目建议用官方 API 或本地模型。

## 十二、参考资源链接

- ChatGPT 官网：`chat.openai.com`
- OpenAI 开发者平台：`openai.com/platform`
- OpenAI API 文档：`platform.openai.com/docs`
- OpenAI 定价：`openai.com/pricing`
- Codex CLI 仓库：`github.com/openai/codex`
- GPT 最佳实践：`platform.openai.com/docs/guides/prompt-engineering`
