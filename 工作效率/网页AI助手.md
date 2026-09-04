# 网页 AI 助手使用指南

> 网页 AI 助手指打开浏览器（或手机 App）就能直接对话的通用 AI 产品，如 ChatGPT、DeepSeek、豆包、Kimi、通义千问、Gemini、Claude 等。开发者可以用它写代码、调 bug、读文档、做数据分析、做技术调研，是日常开发的"瑞士军刀"。本文梳理主流产品对比、选型思路、通用提示词技巧、开发者用法与国内访问方案。

**关键词**：ChatGPT、DeepSeek、豆包、Kimi、对话式 AI、提示词

## 抽象类比

网页 AI 助手就像口袋里装着的万能顾问，或者说一把"瑞士军刀"。问它代码怎么写、bug 怎么调、文档怎么读、数据怎么分析，它都能搭上几句话。和单一用途的工具不同，它什么都懂一点，从写正则到讲架构到画图都能应付，是日常开发的"问路石"和"陪练"。但它不直接动手改你的代码——要真正改代码、跑命令，得请"本地 Agent 工具"出马（见《本地Agent工具》）。

## 适合人群 / 预备知识

- 想用 AI 提升开发、学习、办公效率的所有人
- 无需编程基础即可使用网页对话；部分章节涉及 API，需要会使用命令行或写简单脚本

## 一、主流产品一览

| 产品 | 厂商 | 特点 | 国内可用性 |
| --- | --- | --- | --- |
| ChatGPT | OpenAI | 综合能力标杆，GPT-5、o 系列推理、GPTs 生态 | 需科学上网 |
| DeepSeek | 深度求索 | 性价比极高、推理（R1）强、开源模型 | 直接使用 |
| 豆包 | 字节跳动 | 中文日常场景、免费额度大、语音自然 | 直接使用 |
| Kimi | 月之暗面 | 长文本阅读、多文件分析起家 | 直接使用 |
| 通义千问 | 阿里巴巴 | 生态完整（文档/音视频）、开源 Qwen 系列 | 直接使用 |
| 文心一言 | 百度 | 中文知识、免费 | 直接使用 |
| 智谱清言（GLM） | 智谱 AI | GLM 系列、Agent 能力 | 直接使用 |
| Gemini | Google | 多模态强、长上下文、与 Google 生态联动 | 需科学上网 |
| Claude | Anthropic | 写作与代码质量口碑好、Artifacts | 需科学上网 |

## 二、选型思路

- **日常中文问答、写作、办公**：豆包、DeepSeek、Kimi 都够用，选一个顺手的
- **数学、代码推理、性价比**：DeepSeek（R1 思考模式免费）
- **综合能力、复杂任务、生态插件**：ChatGPT
- **超长文档分析**：Kimi、Gemini（长上下文）
- **高质量写作、代码审查**：Claude
- **多模态（图/视频理解）、谷歌生态**：Gemini

原则：没有"最强"，只有"当前任务最合适"。可以并行保留 2-3 个，互相交叉验证重要结论。

## 三、通用使用方式

- **网页版**：最常用，各家官网直接对话
- **手机 App**：iOS / Android 均有，支持语音对话
- **桌面 App**：ChatGPT、Claude 等有官方桌面端，可与 IDE 联动
- **浏览器插件/侧边栏**：豆包、Kimi 等提供网页划词助手
- **文件上传**：普遍支持 PDF、Word、Excel、图片、代码文件
- **联网搜索**：回答时效性问题，各家基本都有
- **深度思考模式**：DeepSeek R1、o 系列、Gemini Thinking 等，适合难题，但更慢更贵

## 四、提示词技巧（各家通用）

- **角色设定**：开头先说"你是一个资深后端工程师"
- **分步骤**：把复杂任务拆成多步让它一步步做
- **给例子**：提供一两个示例输出，让它对齐格式
- **明确输出格式**：要求"用 JSON 输出"、"用 Markdown 列表"、"不要超过 200 字"
- **给足上下文**：贴完整报错、代码、背景，而不是一句话描述
- **追问与修正**：不满意就指出哪里不对，让它改，而不是重开对话
- **上下文管理**：一个对话一个主题，跑题多了开新对话；重要结论让它"总结成一段"方便复制
- **自定义指令 / 记忆**：ChatGPT 的 Custom Instructions、各家"记忆"功能，配置一次全局生效，避免重复自我介绍

## 五、开发者常用场景

- **代码生成与解释**：写函数、写正则、解释一段陌生代码
- **Debug 辅助**：贴报错信息和上下文，让它分析可能原因
- **文档撰写**：写 README、API 文档、注释、提交信息
- **学习新技术**：让它讲清楚某个框架的概念、用法、最佳实践
- **Code Review**：贴代码让它指出潜在 bug、改进点
- **数据分析**：上传 CSV/Excel，让它清洗数据、画图、做统计分析（如 ChatGPT 的 Code Interpreter）
- **方案设计**：先在网页端把方案讨论清楚，再交给本地 Agent 工具实现（分工见《本地Agent工具》）

## 六、API 接入（进阶）

### 6.1 通用模式

各家 API 设计高度相似，主流都提供 **OpenAI 兼容端点**，学会一家等于学会全部：

1. 到开放平台注册账号、创建 API Key（`sk-` 开头）
2. 设置 `BASE_URL`（指向哪家）和 `API_KEY`
3. 调用 `chat/completions` 接口，传 `model` 和 `messages`

以 OpenAI 为例：

```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-mini",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

DeepSeek 平台（`platform.deepseek.com`）端点为 `api.deepseek.com`，模型 `deepseek-chat` / `deepseek-reasoner`；智谱、通义、硅基流动等同理，只换 URL、Key 和模型名。

### 6.2 计费

- 按使用量计费，单位是 token，输入和输出分别计价
- 不同模型价格差距很大（如 GPT-4o mini 比 GPT-4o 便宜几十倍；DeepSeek 约 ¥1/百万 tokens）
- 在 Billing 页面设置用量上限，避免意外爆账
- 订阅（网页会员）和 API 计费是两套独立体系，需分别充值

## 七、国内访问与账号

- **直接可用**：DeepSeek、豆包、Kimi、通义千问、文心一言、智谱清言——手机号注册即用
- **需科学上网**：ChatGPT、Gemini、Claude——账号注册需要海外手机号或邮箱
- **中转 API**：第三方服务商提供 OpenAI 接口中转，配置 `OPENAI_BASE_URL` 指向中转地址。注意事项：
  - 中转 API 有数据安全风险，不要传敏感代码
  - 价格可能比官方便宜也可能更贵，自行甄别
  - 不要在多家服务商之间频繁切换 Key，容易触发风控

## 八、与本地 Agent 工具的配合

不同形态定位不同，建议组合使用：

- **网页 AI 助手**：问问题、查资料、要思路、方案设计、文档撰写
- **本地 Agent 工具**（Claude Code、Codex、Cursor、Trae 等）：在 IDE 或命令行里改代码、跑测试、跨文件重构
- **IDE 补全类**（GitHub Copilot）：写代码时实时补全

简单判断：想"问"→ 网页助手；想"让它动手做"→ 本地 Agent。

## 常见问题速答

- Q：免费版够用吗？
  A：日常问答够用；重度使用（高级模型、更多次数）各家都需要订阅，国内产品免费额度普遍更大。

- Q：回答对不对怎么判断？
  A：AI 会一本正经地编造（幻觉），重要结论一定要交叉验证：换个模型再问、查官方文档、跑代码验证。

- Q：API 和网页订阅是同一个账号吗？
  A：同一套账号体系，但 API 计费和订阅计费独立，需分别充值。

- Q：API 调用报错 "Rate limit"？
  A：用量超限，等一会儿再试，或在账户里提升 tier。

- Q：中转 API 安全吗？
  A：第三方中转有风险，敏感代码不要走中转；重要项目建议用官方 API 或本地部署开源模型（如 Qwen、DeepSeek 开源版）。

- Q：数据会被拿去训练吗？
  A：看产品设置，ChatGPT 可在设置里关闭"用于模型训练"；企业版（Team/Enterprise）默认不训练。敏感信息一律脱敏。

## 参考资源链接

- ChatGPT 官网：`chat.openai.com`
- DeepSeek 开放平台：`platform.deepseek.com`
- 豆包：`doubao.com`
- Kimi：`kimi.moonshot.cn`
- 通义千问：`tongyi.aliyun.com`
- OpenAI API 文档：`platform.openai.com/docs`
- Prompt 工程指南：`platform.openai.com/docs/guides/prompt-engineering`
