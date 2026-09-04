# WinGet 指南

> WinGet（Windows Package Manager）是微软官方的命令行包管理器，定位类似 Linux 的 `apt` 或 macOS 的 `brew`。本文覆盖原理、配置、高频命令、开发者实战场景与故障排除，适用于新电脑开发环境搭建、日常工具链更新、团队统一开发配置。

**关键词**：WinGet、包管理器、命令行、环境自动化、静默安装、export/import

## 抽象类比

WinGet 就像 Windows 的"应用超市收银台"：你说出商品编号（包 ID），它负责从官方货架取货、验货（哈希校验）、装袋（静默安装），全程不用你自己去官网找下载按钮、躲捆绑软件。配合 `export`/`import`，它还能把整台电脑的软件清单"存档"，在新电脑上"读档"一键复原。

## 第一章：背景与核心原理

### 1.1 什么是 WinGet

WinGet 让 Windows 开发者告别"去官网找下载按钮"的繁琐流程，直接在终端完成所有软件的生命周期管理：搜索、安装、更新、卸载、导出、导入。

### 1.2 核心工作机制（理解原理有助于排错）

WinGet 本身不搬运安装包，它只负责"调度"。其架构分为三层：

1. **客户端（winget.exe）**：你敲击命令的执行端
2. **仓库（Repository）**：默认使用微软维护的**社区仓库**（GitHub 上的 `microsoft/winget-pkgs`），存储海量软件的安装清单（Manifest），包含下载地址、文件哈希值、静默安装参数等
3. **执行器**：WinGet 根据清单下载对应的 `.exe` / `.msi` / `.msix` 安装包，然后调用 Windows 底层的安装程序实际执行

### 1.3 为什么开发者应该使用它

- **幂等性（Idempotence）**：配合 `export` / `import` 命令，可以一键还原整个开发工具栈
- **避开捆绑软件**：绕过官网混淆的下载按钮，确保下载官方纯净版
- **静默安装（Silent）**：适合编写自动化脚本、CI/CD 构建机初始化

## 第二章：基础环境与配置

### 2.1 确认是否已安装

WinGet 通常随 Windows 10 1809+ 和 Windows 11 预装。在 PowerShell 或 CMD 中输入：

```powershell
winget --version
```

若无显示，请前往 Microsoft Store 搜索"应用安装程序"进行更新或安装。

### 2.2 设置代理（国内开发环境必备）

如果你在公司内网或需要加速访问 GitHub，建议设置代理（以 `http://127.0.0.1:7890` 为例，按你本地代理的实际端口修改，如 Clash Verge 默认为 7897）：

```powershell
# 设置全局代理
winget settings --proxy http://127.0.0.1:7890

# 或者仅对当前命令生效
winget search python --header "Proxy: http://127.0.0.1:7890"
```

### 2.3 查看和重置软件源

```powershell
# 查看当前源
winget source list

# 重置默认源（如果出现 404 或连接错误）
winget source reset --force
```

## 第三章：开发者高频命令全解析

命令遵循 `winget [操作] [参数] [条件]` 的结构。

### 3.1 搜索（Search）

安装前务必用 ID 进行精确搜索，防止安装到非官方（Unofficial）的同名包。

```powershell
# 模糊搜索
winget search node

# 精确搜索并查看详情（推荐，找到的 ID 最准确）
winget search --exact OpenJS.NodeJS
```

### 3.2 安装（Install）—— 核心场景

```powershell
# 标准安装（使用官方 ID）
winget install --id Microsoft.VisualStudioCode

# 【开发者场景】一次安装多个 IDE/工具（空格分隔）
winget install --id Git.Git OpenJS.NodeJS Microsoft.PowerShell Microsoft.WindowsTerminal

# 【开发者场景】自动接受所有许可协议（用于自动化脚本）
winget install --id Docker.DockerDesktop --accept-package-agreements --accept-source-agreements

# 指定安装目录（仅部分支持 MSI 的软件有效，如 Node.js）
winget install --id OpenJS.NodeJS --location D:\DevTools\Node
```

### 3.3 更新（Update / Upgrade）—— 日常维护

```powershell
# 查看所有可更新的软件列表
winget upgrade

# 【开发者场景】更新所有开发工具（建议下班前运行）
winget upgrade --all

# 更新时排除某个特定软件（比如正在使用的 VS）
winget upgrade --all --exclude Microsoft.VisualStudio

# 【开发者场景】静默全量更新（适合后台运行，不弹窗打扰）
winget upgrade --all --silent --accept-package-agreements
```

**【特别说明】关于更新是否安装在原位置**：执行 `winget upgrade` 时，WinGet 只是调用软件自带的安装程序。绝大多数主流开发者工具（如 VS Code、Git、Node.js）在更新时会自动覆盖原位置，因此路径不会改变。只有极少数便携版软件或编写不规范的老旧安装包才会重置路径。最稳妥的做法是：安装时尽量使用默认路径（`C:\Program Files`），这能最大程度避免更新后路径漂移的问题。

### 3.4 卸载（Uninstall）

```powershell
# 卸载普通软件
winget uninstall --id Google.Chrome

# 强制卸载（清理残留）
winget uninstall --id Google.Chrome --force
```

### 3.5 查看详情（Show）

```powershell
# 查看版本号、发行方、支持的静默参数
winget show Microsoft.PowerToys
```

## 第四章：开发者场景实战（必看）

### 场景 1：新电脑"一键恢复"开发环境（导出/导入）

这是 WinGet 对程序员最有价值的功能，适合重装系统或入职新公司配置新电脑。

第一步（旧电脑）导出清单：

```powershell
# 导出当前所有已安装软件到 JSON 文件
winget export --output D:\Backup\myDevSetup.json
```

第二步（新电脑）一键安装：

```powershell
# 执行批量安装（加上 --ignore-versions 可忽略版本差异，装最新的）
winget import --import-file D:\Backup\myDevSetup.json --accept-package-agreements
```

### 场景 2：锁定关键工具版本（防止自动更新破坏兼容性）

对于某些老项目，可能需要锁定特定的 JDK 或 Node 版本，防止 `--all` 误升。

```powershell
# 将 Node.js 18 版本固定（锁定后 upgrade --all 将自动跳过它）
winget pin add --id OpenJS.NodeJS --version 18.20.0

# 查看当前锁定的软件列表
winget pin list

# 解除锁定
winget pin remove --id OpenJS.NodeJS
```

### 场景 3：为团队编写初始化脚本（setup.ps1）

新人或构建机只需双击运行此脚本即可准备好所有依赖。

```powershell
# setup.ps1 示例
Write-Host "Setting up Dev Environment..." -ForegroundColor Green

# 安装核心工具链
winget install --id Git.Git --silent --accept-package-agreements
winget install --id OpenJS.NodeJS --silent --accept-package-agreements
winget install --id Microsoft.VisualStudioCode --silent --accept-package-agreements
winget install --id Microsoft.PowerShell --silent --accept-package-agreements
winget install --id Microsoft.WindowsTerminal --silent --accept-package-agreements

Write-Host "All done! Please restart your terminal." -ForegroundColor Yellow
```

## 第五章：进阶与故障排除

### 5.1 常用开发者包 ID 速查表

| 软件 | 官方 ID | 备注 |
| --- | --- | --- |
| Visual Studio Code | `Microsoft.VisualStudioCode` | 程序员必备 |
| Git | `Git.Git` | 版本控制 |
| Node.js | `OpenJS.NodeJS` | JS 运行时 |
| Python | `Python.Python` | 默认安装 3.x 版本 |
| Docker Desktop | `Docker.DockerDesktop` | 容器化 |
| PowerShell 7 | `Microsoft.PowerShell` | 跨平台 Shell |
| Windows Terminal | `Microsoft.WindowsTerminal` | 现代化终端 |
| PowerToys | `Microsoft.PowerToys` | 开发辅助工具 |
| Oh My Posh | `JanDeDobbeleer.OhMyPosh` | 终端美化 |
| Visual Studio 2022 社区版 | `Microsoft.VisualStudio.2022.Community` | 含专业版/企业版 |
| Wireshark | `WiresharkFoundation.Wireshark` | 网络抓包 |
| Postman | `Postman.Postman` | API 调试 |
| IntelliJ IDEA 社区版 | `JetBrains.IntelliJIDEA.Community` | Java IDE |

### 5.2 常见报错解决

- **`0x8D15000F`（清单冲突）**：软件已通过其他方式（如手动 MSI）安装了更高版本。加上 `--force` 参数强制覆盖
- **无法找到包**：搜不到时，先执行 `winget source update` 更新本地缓存索引再尝试
- **管理员权限不足**：WinGet 默认要求管理员权限安装大部分软件。右键终端选择"以管理员身份运行"即可。若使用 PowerShell 脚本，可在头部加上 `#Requires -RunAsAdministrator`

### 5.3 图形化前端（可选）

若偶尔记不住命令，可安装 UniGetUI（原名 WingetUI），方便在界面上点选操作。

```powershell
winget install --id UnigetUI.UnigetUI
```

## 结语

WinGet 极大简化了 Windows 开发者工具的维护成本。将 `winget export` 的 JSON 文件保存在云盘或 GitHub 私有仓库中，再配合 `winget upgrade --all` 每周执行一次，即可让开发环境始终保持最新且高度一致。遇到更新路径问题，优先相信官方默认路径，并关注对应软件的 GitHub Release 说明。
