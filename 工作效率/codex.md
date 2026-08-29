# Codex 使用指南

> Codex 是 OpenAI 推出的 AI 编程代理，基于 GPT-5-codex 模型构建，提供 CLI 终端版和桌面版（现已整合为 ChatGPT 桌面版的 Codex 功能）。它不是"终端里的 ChatGPT"，而是一个能读取代码库、修改文件、执行 shell 命令、跑测试、通过 MCP 连接外部工具的完整代理。代码留在本地，只把必要上下文发到 OpenAI API，因此既快又安全。本文前半部分面向 Windows 环境开发者，系统讲解 Codex CLI 的安装、登录、模式选择与国内中转方案；后半部分聚焦 ChatGPT 桌面版的实战配置与竞赛开发工作流，涵盖轮次开发规范、AGENTS.md 标准文件族、多会话管理、GPT 网页端与 Codex 协作模式。学完后，你能独立用 Codex 解释陌生代码库、修复 bug、跑测试、做 code review，并能为竞赛项目设计"方案设计 + AI 实现"的完整开发流程。

**关键词**：Codex、ChatGPT 桌面版、AI 编程代理、轮次开发、AGENTS.md、竞赛开发

## 抽象类比

Codex 就像坐在你旁边的资深程序员。你把项目代码、报错信息、需求描述丢给它，它能自己读代码、改文件、跑命令、跑测试，再给你一份带着 diff 的修改方案。和"终端里调 ChatGPT"不同，它不只是回答问题，而是真正动手干活——读你的代码库、写回磁盘、执行 shell 命令，把"问 AI"变成"使唤 AI"。

## 适合人群 / 预备知识

- 想用 AI 提升编码效率的后端/前端/全栈开发者
- 熟悉命令行基础操作（cd、git、npm 等命令）
- 有一个 OpenAI 账号（ChatGPT 订阅用户或 API Key 用户）
- 本机已安装 Node.js 22+ 或可用 PowerShell 脚本安装
- 了解 Git 基本工作流（status、diff、commit）
- 有可访问的代码仓库用于练习

## 一、Codex 是什么

Codex 是一个 agentic coding product（代理式编程产品），用 Rust 编写，速度快。它和"终端里调用 ChatGPT API"有本质区别：

- 能直接读取整个代码库的上下文
- 能修改文件并写入磁盘
- 能执行 shell 命令、跑测试、跑 lint
- 能通过 MCP（Model Context Protocol）连接外部工具
- 本地执行 + 云端推理的混合架构

### 四个入口

- Codex CLI：终端工具，最适合新手理解工作循环，本文重点
- IDE extension：集成在 VS Code/Cursor/Windsurf 编辑器内
- Codex app：桌面端，支持多线程、worktree、浏览器预览、Git diff
- Codex cloud：远程并行任务，适合长任务批量执行

### 混合架构

```mermaid
flowchart LR
    A[本地终端 Codex CLI] -->|读取代码库| B[本地文件系统]
    A -->|执行命令| C[本地 Shell/沙箱]
    A -->|发送必要上下文| D[OpenAI API 云端推理]
    D -->|返回修改方案| A
    A -->|写回文件| B
```

代码、文件、命令执行都发生在本地，只有推理请求发送到 OpenAI 服务器，既保护代码隐私，又利用了云端模型能力。

## 二、安装

### 系统要求

- 操作系统：macOS 12+、Ubuntu 20.04+/Debian 10+、Windows 11（需 WSL2 或原生 PowerShell）
- Node.js 22+（推荐 LTS 版本）
- Git 2.23+（可选但强烈推荐）
- 内存：4GB 最低，8GB+ 推荐

### 各系统安装命令

npm 方式（跨平台，推荐 Windows 用户）：

```bash
npm install -g @openai/codex
```

Homebrew 方式（macOS）：

```bash
brew install codex
```

官方脚本方式（macOS/Linux）：

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | CODEX_NON_INTERACTIVE=1 sh
```

Windows PowerShell 方式：

```powershell
$env:CODEX_NON_INTERACTIVE=1; irm https://chatgpt.com/codex/install.ps1 | iex
```

### 验证与更新

```bash
codex --version        # 或 codex -V
codex doctor           # 检查环境是否完整，推荐首次安装后立即运行
npm install -g @openai/codex@latest   # 更新（无自带更新命令）
```

## 三、登录认证

Codex 像一个坐在你旁边的资深开发者，但它干活前需要先"出示工牌"。登录认证就是这个工牌，有两种方式。

### 方式一：ChatGPT 登录（默认）

适合 ChatGPT Plus/Pro/Team/Edu/Enterprise 用户（Edu 可免费体验）。

1. 终端运行 `codex login`
2. 浏览器自动打开 OAuth 授权页面
3. 授权后凭据自动写入 `~/.codex/auth.json`
4. 验证状态：`codex login status`

### 方式二：API Key 登录

适合 CI/CD、脚本自动化、无订阅用户。

临时设置（macOS/Linux）：`export OPENAI_API_KEY="sk-xxx"`

临时设置（Windows PowerShell）：`$env:OPENAI_API_KEY="sk-xxx"`

永久设置（Windows）：`setx OPENAI_API_KEY "sk-xxx"`

永久写入配置文件，编辑 `~/.codex/auth.json`：

```json
{"OPENAI_API_KEY": "sk-xxx"}
```

远程/无浏览器环境使用：`codex login --device-auth`

### 两种方式切换与凭据保护

```bash
codex --config preferred_auth_method="apikey"   # 切到 API Key
codex --config preferred_auth_method="chatgpt"  # 切回 ChatGPT
codex logout                                    # 登出
```

- 凭据文件 `auth.json` 像密码一样保护，绝对不能提交到 Git 仓库
- 建议把 `~/.codex/` 加入全局 gitignore 或项目 .gitignore

## 四、三种工作模式

模式决定 Codex 在干活时有多少自主权。新手从 suggest 开始，逐步放开权限。

### suggest 模式（默认，建议新手使用）

- 权限：只读文件，提议编辑和 shell 命令
- 操作：每一步都需要手动批准
- 适用：安全探索、代码审查、学习陌生代码库
- 启动：`codex`（默认就是 suggest）

### auto-edit 模式（自动编辑）

- 权限：可读取和写入文件
- 操作：执行 shell 命令前仍需批准
- 适用：重构、批量重命名、重复性编辑任务
- 启动：`codex --auto-edit` 或 `codex -a`

### full-auto 模式（完全自动）

- 权限：在沙盒中自主读取、写入、执行命令
- 操作：基本不需要人工干预
- 适用：修复构建错误、原型开发、长任务
- 启动：`codex --full-auto` 或 `codex -f`

### 模式选择决策

```mermaid
flowchart TD
    A[启动 Codex] --> B{任务类型?}
    B -->|先理解代码| C[suggest 模式]
    B -->|批量编辑文件| D[auto-edit 模式]
    B -->|长任务自主执行| E[full-auto 模式]
    C --> F[逐步批准, 安全第一]
    D --> G[文件自动改, 命令需批准]
    E --> H[沙箱内全自动, 注意审查]
```

### 新手推荐组合

```bash
codex --sandbox workspace-write --ask-for-approval on-request
```

官方推荐的安全起步组合：允许写工作区，但每一步都先问你。

## 五、沙箱与权限系统

Codex 用两层安全机制防止误操作，就像给一个能干但偶尔冒失的助手套上"安全围栏"。

### 第一层：sandbox（技术上能做什么）

- read-only：只能读文件，不能写
- workspace-write：可以写当前工作目录内的文件
- full-access：可以读写任意路径，包括系统文件

### 第二层：approval policy（什么时候停下来问）

- on-request：每次需要操作时都问
- on-failure：先尝试，失败后再问
- never：从不打断（危险，仅限可信任务）

### 推荐配置阶梯

- 第一周：`--sandbox read-only`（只看不改）
- 第二周：`--sandbox workspace-write --ask-for-approval on-request`
- 熟悉后：`--sandbox workspace-write --ask-for-approval on-failure`
- 永远不要一开始就给 `full-access`

### 安全建议

- 在 Git 仓库内运行，便于随时回滚
- 重要操作前 `git status` 确认工作区干净
- full-auto 模式务必配合 workspace-write 沙箱
- 不熟悉的命令让 Codex 解释后再批准

## 六、常用命令速查

### 交互式会话

- `codex`：启动交互式会话（默认 suggest 模式）
- `codex "修复 README 里的拼写错误"`：直接给定任务进入交互
- `codex --full-auto "跑一遍测试"`：以全自动模式启动

### 非交互式（脚本/CI 友好）

- `codex exec "任务描述"`：非交互式执行（别名 `codex e`）
- `codex exec --json "任务"`：JSON 输出，便于脚本解析
- `codex exec --full-auto "任务"`：全自动非交互执行
- `codex apply <patch>`：把代理生成的 diff 应用到工作树（别名 `codex a`）

### 会话与登录管理

- `codex resume`：恢复之前的会话；`codex resume --last` 恢复最近一次
- `codex login`：ChatGPT OAuth 登录；`codex login --device-auth` 设备授权
- `codex login status`：查看登录状态；`codex logout`：登出

### MCP 与工具

- `codex mcp list`：列出已配置的 MCP 服务器
- `codex mcp add <name>`：添加 MCP 服务器
- `codex completion zsh`：生成 shell 补全（也支持 bash、fish）
- `codex doctor`：检查环境配置

## 七、交互式会话内置命令

进入 `codex` 交互会话后，用斜杠命令控制会话：

- `/status`：查看当前配置、所用模型、Token 消耗情况
- `/model`：切换模型（gpt-5 / gpt-5-codex / o4-mini）或调整推理强度
- `/approvals`：实时调整批准策略
- `/init`：在当前目录生成 AGENTS.md 项目规则文件
- `/help`：查看所有可用命令
- `/quit` 或 Ctrl+C 两次：退出会话

`/status` 是排查问题的第一步，Token 消耗异常或模型不对时先看这里。

## 八、配置体系

Codex 配置有五层优先级，从高到低覆盖。就像 CSS 的层叠规则，越具体的越优先。

### 五层优先级

```mermaid
flowchart TD
    A[1. CLI 参数 & --config 覆盖] --> B[2. Profile 配置]
    B --> C[3. 项目配置 .codex/config.toml]
    C --> D[4. 用户配置 ~/.codex/config.toml]
    D --> E[5. 系统配置 /etc/codex/config.toml]
    E --> F[6. 内置默认值]
```

### config.toml 示例

用户配置文件 `~/.codex/config.toml`：

```toml
model = "gpt-5-codex"
model_provider = "openai"
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```

项目配置文件 `<项目根>/.codex/config.toml` 可覆盖用户配置（如 `model = "gpt-5"`）。

### AGENTS.md 项目规则

AGENTS.md 是给 Codex 的"项目说明书"，提供持久化上下文。建议放项目根目录，内容包括：

- 编码规范（命名、缩进、注释风格）
- 依赖说明（包管理器、版本要求）
- 测试命令（`npm test`、`pytest` 等）
- 项目结构（主要目录职责）
- 已知坑点和注意事项

`codex /init` 会自动扫描项目并生成 AGENTS.md 初稿，然后手动完善。

## 九、模型选择

Codex 默认使用 gpt-5.3-codex，但不同任务适合不同模型。

### 三个模型对比

- gpt-5.3-codex
  - 定位：代码生成专用，默认模型
  - 优势：专为编程优化，速度快
  - 何时用：日常编码、修 bug、写测试
- gpt-5
  - 定位：复杂推理
  - 优势：分析能力强，理解复杂逻辑
  - 何时用：代码审查、架构分析、难题排查
- o4-mini
  - 定位：轻量任务
  - 优势：成本低、响应快
  - 何时用：简单修改、批量重复任务、控制成本

### 切换模型与推理强度

交互式会话内用 `/model` 切换，或在 config.toml 中设置 `model = "gpt-5"`。`/model` 还能调整推理强度（reasoning effort）：

- low：快速响应，简单任务
- medium：默认平衡
- high：复杂任务，多想一会儿

## 十、第一次使用完整流程

第一次用 Codex，建议按这个流程走，避免上来就翻车。

### 准备工作

1. 进入一个 Git 仓库：`cd your-project`
2. 确认工作区干净：`git status`
3. 如有未提交改动，先 commit 或 stash

### 启动并理解项目

4. 启动 Codex：`codex`
5. 让它先解释项目结构，用这个 prompt：

```
先阅读这个仓库，解释主要目录、启动命令和测试命令。不要修改文件。
```

6. 仔细阅读它的解释，对照实际代码验证

### 给小任务试水

7. 给一个小的修改任务，比如修文档拼写、加注释
8. 让它运行测试或 lint：

```
运行项目测试，报告结果。如果有失败，先分析原因再修复。
```

9. 用 `git diff` 审查所有改动
10. 满意后 `git add` + `git commit`

### 逐步放大任务

11. 尝试稍微复杂的任务：重构函数、提取组件
12. 全程保持 suggest 或 auto-edit 模式
13. 不要一开始就让它"重构整个项目"

## 十一、国内使用方案

国内直连 OpenAI API 不稳定，需要科学上网或使用网关中转。下面是中转方案配置步骤。

### 配置步骤

1. 登出当前账号：`codex logout`
2. 在中转服务（如 agentrouter.org）注册并获取 API Key
3. 设置环境变量：`setx AGENT_ROUTER_TOKEN "你的中转 API Key"`
4. 创建 `~/.codex/config.toml`，配置中转地址：

```toml
model = "gpt-5"
model_provider = "openai-chat-completions"

[model_providers.openai-chat-completions]
name = "OpenAI Chat Completions"
base_url = "https://agentrouter.org/v1"
env_key = "AGENT_ROUTER_TOKEN"
```

5. 创建 `~/.codex/auth.json`：`{"OPENAI_API_KEY": "你的中转 API Key"}`
6. 验证：`codex doctor` 和 `codex "测试一下连接"`

### 注意事项

- 中转服务的稳定性和延迟会影响使用体验
- 部分中转服务可能不支持所有模型，先用 `codex doctor` 测试
- 切换回官方服务时，记得改回 config.toml 的 base_url
- 不同中转服务计费方式不同，注意成本控制

## 十二、实战技巧

10 条高手都在用的 Codex 技巧：

1. 用 AGENTS.md 固化项目规则，避免每次重复说明编码规范
2. 大任务拆小步骤，每步用 git commit 锁定，便于回滚
3. 让 Codex 先解释再修改：`先解释这段代码的作用，再提议修改方案`
4. 用 `codex exec --json` 接入 CI/CD，自动跑 code review
5. full-auto 模式前先 `git stash` 保护未提交工作
6. 复杂 bug 让 Codex 先列假设再验证：`列出 3 个可能的根因，逐一验证`
7. 用 `codex resume --last` 恢复中断的会话，省去重新解释上下文
8. 模型切换：日常用 gpt-5-codex，审查架构切 gpt-5，省钱用 o4-mini
9. 让 Codex 写测试再让它跑测试，形成闭环验证
10. 用 MCP 连接外部工具（如数据库、API 文档），扩展 Codex 能力边界

## 面试速答

- 安装失败：检查 Node.js 版本是否 22+，用 `node -v` 验证
- 登录卡住：尝试 `codex login --device-auth`，或检查网络代理
- Token 消耗过快：用 `/status` 查看消耗，小任务切 o4-mini
- 修改了不该改的文件：用 sandbox 限制为 workspace-write，立即 `git checkout`
- full-auto 跑飞了：Ctrl+C 中断，`git reset --hard` 回滚到安全点
- 国内连不上：配置中转服务，参考第十一节
- 命令执行失败：先 `codex doctor` 检查环境，再检查沙箱权限
- 模型回答不对题：检查 AGENTS.md 是否提供足够上下文，prompt 是否清晰
- 凭据泄露风险：把 `~/.codex/auth.json` 加入 .gitignore，绝不提交
- 多线程改同一文件冲突：用 worktree 或串行执行，避免并行冲突

## 十四、学习路径建议

从入门到进阶的阶梯式学习路径：

### 第一阶段：入门（1-2 天）

- 安装并登录 Codex
- 用 suggest 模式解释一个熟悉的小项目
- 完成一次小修改（修拼写、加注释）
- 学会 `/status` 和 `/model` 命令

### 第二阶段：熟练（3-5 天）

- 切换到 auto-edit 模式做批量编辑
- 让 Codex 跑测试和 lint，形成闭环
- 创建第一个 AGENTS.md
- 学会 `codex resume` 恢复会话

### 第三阶段：进阶（1-2 周）

- 尝试 full-auto 模式跑长任务
- 配置 config.toml 定制工作流
- 用 `codex exec --json` 接入脚本或 CI
- 学习 MCP 扩展外部工具

### 第四阶段：精通（持续）

- 多模型组合策略（codex 干活、gpt-5 审查、o4-mini 省钱）
- 编写自动化 code review 流水线
- 团队共享 AGENTS.md 规范
- 探索 Codex app 和 cloud 的多线程能力

### 学习心法

- 真正的价值来自"清晰任务 + 可验证命令 + Git 审查"三件套
- 不要追求一次让 Codex 干完所有事，分步骤反而更可控
- 把 Codex 当成需要明确指令的实习生，而不是全自动机器人

## 十五、参考资源

- 官方文档：developers.openai.com/codex/cli
- GitHub 仓库：github.com/openai/codex
- Codex CLI npm 包：npmjs.com/package/@openai/codex
- OpenAI 开发者平台：developers.openai.com
- AGENTS.md 规范：见官方文档 Configuration 章节
- MCP 协议说明：modelcontextprotocol.io
- 社区示例配置：GitHub 仓库的 examples 目录

## 十六、ChatGPT 桌面版（Codex App）实战配置

Codex 产品线现已整合：终端用户用 Codex CLI，桌面用户用 ChatGPT 桌面版内置的 Codex 功能（下文统称"Codex 桌面版"）。两者共享模型与代理能力，但桌面版提供图形化会话管理、边栏 diff 查看、可视化批准流程，更适合竞赛这类需要反复审查和多人协作的场景。本章聚焦桌面版的特有配置，CLI 用户可跳过。

### 16.1 工作目录：必须打开已有目录

Codex 桌面版新建项目时，默认会在 C 盘创建工作目录（类似 `C:\Users\你\codex-projects\xxx`）。这会带来三个麻烦：

- C 盘空间被占满，影响系统运行
- 项目散落在用户目录深处，不便用其他工具（VS Code、git GUI）访问
- 与你已有的竞赛仓库隔离，导致代码同步困难

**正确做法**：新建项目时选择"打开已有目录"，指向你本机已有的竞赛仓库（如 `e:\竞赛项目\xxx`）。这样 Codex 直接在你的仓库里干活，git 历史、依赖、配置都在原位。

### 16.2 沙盒模式 vs 本地模式

这是桌面版最重要的一个选择，项目开始时务必确认：

- **沙盒模式（Sandbox）**：Codex 在隔离环境里运行，不能访问电脑真实系统、不能装系统级依赖、网络受限。优点是绝对安全，误操作不会搞坏电脑；缺点是很多依赖装不上，跑不了真实硬件相关任务
- **本地模式（Local）**：Codex 直接接入电脑真实环境，能装包、能跑 QEMU、能调用真实工具链。优点是能力强，缺点是误操作可能影响系统

竞赛开发通常需要跑 QEMU、调用交叉编译工具链、连接开发板，**建议用本地模式**，但务必在 Git 仓库内运行，便于回滚。桌面版在首次进入项目时会提示当前模式，仔细确认。

### 16.3 记忆功能：开启会话间记忆互通

ChatGPT 桌面版的"个性化设置"里有"记忆"开关。开启后：

- 不同会话之间能共享关键上下文（项目技术栈、已确认的设计决策、依赖版本）
- ChatGPT 会自动记录你提到过的关键内容，下次会话直接调用
- 避免每个新会话都要重新解释项目背景

**竞赛场景特别重要**：竞赛周期长、会话多，记忆功能能让你不必每次都重复"这是 StarryOS 内核比赛，用 RK3588，交叉编译工具链是…"这类背景。

### 16.4 批准模式：默认请求批准，可选"类似指令不再询问"

Codex 执行具体指令（写文件、跑命令）时会请求你批准。两个层次：

- **默认请求批准**：每次指令都问，最安全，推荐竞赛初期使用
- **类似指令不再询问**：你批准一次后，Codex 记住这类指令后续不再问，提升流畅度

建议先严格（每步都批准），熟悉后对"读文件、跑测试、git status"这类安全操作放开，对"写文件、git push、装包"始终保持批准。

### 16.5 多会话管理：一会话一主题

每个 Codex 项目可以创建多个会话（类似浏览器多标签页）。这是管理复杂项目的核心机制：

- **一个会话只处理一个主题**：比如"会话 A 修 bootloader bug"、"会话 B 写驱动测试"、"会话 C 优化内存管理"
- **会话内保持一致性和指向性**：提问要明确指向当前主题，不要在一个会话里东问一句西问一句
- **Codex 真的会去测没说的范围**：如果你问"测试一下启动流程"，它可能顺便测了关机流程、重启流程，白白消耗用量。明确说"只测冷启动到 shell 出现这一段，不要测重启"

为什么要这样？Codex 的代理行为很"勤快"，你不划定边界，它就会自作主张扩大测试和修改范围，既浪费用量又可能搞乱代码。

### 16.6 边栏查看改动：多次对话，一次修改

桌面版右侧边栏显示 Codex 对文件做出的改动，但**只显示"最新一次对话"的改动，不是整个会话的累计改动**。这影响你的审查策略：

- **推荐模式**：多次对话，一次修改。即先多次和 Codex 讨论方案、分析可行性、确认设计，最后一次对话让它动手改文件。这样边栏显示的就是这次完整的修改，便于人工审查和回溯
- **反面模式**：每次对话都让它改一点，边栏只能看到最后一次的小改动，要回溯整个会话的累计改动很痛苦

如果不得不分多次修改，用 `git diff` 配合审查累计改动，每段对话后及时 commit 锁定。

### 16.7 指令使用顺序与依赖存储

Codex 有一套指令使用的优先顺序（比如优先用 `cargo test`，没有就用 `make test`，再没有就用 `pytest`）。但如果你电脑没装相关依赖，对应指令会失效，Codex 会自动切换到下一个能达到相同效果的指令——这会消耗额外用量且结果不可控。

**解决方法**：在项目记忆或 AGENTS.md 里明确存储当前开发环境存在的工具和依赖，比如：

```markdown
## 环境与依赖
- 操作系统：Windows 11 + WSL2 (Ubuntu 22.04)
- 交叉编译：aarch64-linux-gnu-gcc 12.3
- 模拟器：QEMU 8.0（支持 ARM64）
- Python：3.10（仅用于脚本，不用 pytest）
- 包管理：apt（WSL 内）、scoop（Windows 侧）
- 已安装：make、cmake、git
- 未安装：cargo、go、rustc
```

Codex 读取后就知道直接用可用工具，不会盲目尝试未安装的。

## 十七、轮次开发规范与竞赛工作流

这是竞赛这类长周期、高复杂度项目最关键的工作方法。本节讲清楚"轮次开发"是什么、为什么、怎么落地。

### 17.1 为什么要轮次开发

竞赛项目通常几十个模块、上万行代码，一次性丢给 AI 让它"全做完"必然翻车：要么上下文超限、要么改了这里坏那里、要么无法定位 bug。轮次开发的思路是**用 git 把完整方案切成多个可独立验证的轮次，每轮做完就测试、测试通过就冻结提交，再开下一轮**。

类比：就像盖楼。你不能一次性把地基、主体、装修全干完再验收——任何一个环节出问题都会连累后面所有工作。正确做法是打完地基验收一次、主体封顶验收一次、装修完再验收一次，每层验收通过才往上盖。git commit 就是每层的"验收章"。

### 17.2 轮次开发流水线

```mermaid
flowchart LR
    A[方案设计] --> B[拆分轮次]
    B --> C[轮次N]
    C --> D[实现]
    D --> E[测试]
    E --> F{通过?}
    F -->|是| G[git commit 冻结]
    F -->|否| H[修复]
    H --> E
    G --> I[下一轮]
    I --> D
    G --> J[...]
    J --> K[全部完成]
```

核心规则：

- **每轮有明确的入口和出口**：入口是上一轮的 commit，出口是本轮测试通过后的 commit
- **每轮只做一件事**：不要在"写驱动"的轮次里顺手改"内存管理"，改了就破坏了可回溯性
- **测试不通过不提交**：提交的必须是可运行的状态，哪怕功能不完整

### 17.3 单轮次工作流：六步法

每个轮次内部遵循固定的六步：

1. **设计**：在 GPT 网页端（或 Codex 会话里）讲清楚本轮要做什么、改哪些文件、预期效果
2. **可行性分析**：让 Codex 针对项目现状和具体需求分析可行性，指出潜在风险。这步往往比直接让 LLM 给方案更准——因为 Codex 能读到真实代码
3. **开发计划**：让 Codex 根据可行性反馈制定具体开发计划（改哪些文件、顺序、测试点）
4. **实现**：按计划让 Codex 动手改代码
5. **测试**：让 Codex 跑测试，人工审查 diff
6. **提交**：`git add` + `git commit`，写清楚本轮做了什么，然后开下一轮

### 17.4 GPT 网页端 + Codex 的分工

这是核心：怎么用 GPT 网页端设计方案，让 Codex 实现。

```mermaid
flowchart TD
    subgraph GPT网页端
        A[需求分析] --> B[方案设计]
        B --> C[制作提示词]
        C --> D[编写标准文件族]
    end
    subgraph Codex桌面版
        E[读取标准文件族] --> F[可行性分析]
        F --> G[开发计划]
        G --> H[实现+测试]
        H --> I[git提交]
    end
    D --> E
```

**GPT 网页端负责"想"**：

- 梳理竞赛需求，产出整体方案文档
- 把方案拆成多个轮次，每个轮次有明确目标
- 为每个轮次制作清晰的提示词（prompt）
- 编写标准文件族（AGENTS.md、设计文档、测试规范）

**Codex 负责"做"**：

- 读取 GPT 网页端产出的标准文件族
- 针对真实代码做可行性分析（这步 Codex 比 GPT 网页端准，因为它能读代码）
- 根据可行性反馈调整开发计划
- 动手实现、跑测试、提交 git

**关键衔接**：把 GPT 网页端产出的提示词和文件族保存到项目目录（如 `docs/prompts/`、`AGENTS.md`），Codex 会自动读取。

### 17.5 复杂任务的轮次拆分

面对复杂任务，拆轮次的原则：

- **按模块拆**：驱动、内存、文件系统各一轮
- **按层次拆**：先底层（HAL），再中层（内核服务），再上层（应用）
- **按风险拆**：高风险改动单独一轮，便于失败后回滚
- **每会话一轮**：一个 Codex 会话只处理一个轮次，避免上下文污染

### 17.6 并行处理多项目

网络条件允许时，可以同时对不同项目的会话提出指令，Codex 会并行处理。比如：

- 项目 A 的会话在跑测试（耗时）
- 同时在项目 B 的会话里让它写代码
- 两个会话互不干扰，提升整体效率

注意：同一项目内的多个会话不要并行改同一批文件，会冲突。

## 十八、AGENTS.md 标准文件族与实战建议

### 18.1 AGENTS.md：项目的"宪法"

AGENTS.md 是给 Codex 的项目规则文件，存在项目根目录，Codex 每次启动会自动读取。它解决"每次都要重复说明项目规则"的痛点。

竞赛项目的 AGENTS.md 应包含：

- **项目启动**：怎么编译、怎么跑 QEMU、怎么烧录到开发板
- **测试方式**：用什么测试框架、跑哪些测试、预期输出
- **目录禁区**：哪些目录绝对不能碰（如 `参考仓库/`、`第三方库/`）
- **修改记录**：每次改动后要记录什么（改了哪个模块、为什么改、影响范围）
- **提交流程**：是否每个轮次都提交、commit message 格式、是否推到远程
- **编码规范**：命名、缩进、注释风格

可以让 Codex 用 `/init` 自动生成初稿，然后手动完善。也可以让 Codex 在项目进行中持续更新。

### 18.2 标准文件族：不止 AGENTS.md

完整的标准文件族是一组文件，各自承担不同职责：

- **AGENTS.md**：项目宪法，整体规则和约束
- **design.md**：整体设计方案，架构和模块划分
- **prompts/轮次N.md**：每个轮次的提示词，由 GPT 网页端产出
- **skills.md**：固定流程的参考（见下节）
- **env.md**：环境与依赖清单（见 16.7）

把这套文件族放在项目根目录的 `docs/` 或 `.codex/` 下，Codex 会按需查找。

### 18.3 skills.md：固化重复流程

轮次开发中如果流程较为固定（比如"每次加新驱动都要走改 Kconfig、改 Makefile、写驱动文件、加测试"这四步），可以让 Codex 总结成一个 `skills.md` 文件：

```markdown
# 添加新驱动的标准流程
1. 修改 Kconfig，添加新选项
2. 修改 Makefile，添加编译规则
3. 在 drivers/xxx/ 下创建驱动文件
4. 在 tests/ 下添加对应测试
5. 跑 make && make test 验证
```

后续每次加驱动，Codex 会参考 skills.md 直接按流程走，不用每次重新思考步骤，提升效率和一致性。

### 18.4 项目记忆：存储关键决策

除了文件族，Codex 桌面版的"项目记忆"功能可以存储关键内容：

- 已确认的技术选型和原因
- 已踩过的坑和解决方案
- 团队成员分工
- 里程碑和进度

这些记忆跨会话共享，避免重复决策。

### 18.5 实战避坑十条

1. **带完整方案再开发**：空手套白狼让 Codex 自己想方案，会消耗大量用量且质量差。先用 GPT 网页端想清楚，再让 Codex 实现
2. **先分析可行性再动手**：`针对当前代码分析这个需求的可行性，指出风险和依赖`——这步比直接让它写代码更省时
3. **一会话一主题**：不要在一个会话里混问多个主题，上下文会乱
4. **明确划定测试范围**：`只测 X，不要测 Y`，避免 Codex 自作主张扩大范围
5. **多次对话一次修改**：先讨论充分再让它动手，边栏 diff 才好审查
6. **每轮 commit 锁定**：测试通过立即提交，不要攒着
7. **沙盒模式确认**：项目开始时确认是沙盒还是本地模式，竞赛建议本地
8. **工作目录用已有**：绝不默认 C 盘，指向已有仓库
9. **依赖清单写清楚**：AGENTS.md 里列明已装和未装工具，避免指令无效
10. **并行只跨项目**：同项目内不并行改同文件，跨项目可并行跑会话
