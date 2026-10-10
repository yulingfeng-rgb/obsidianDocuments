# uv：现代 Python 项目与环境管理指南

> 适用场景：Python 学习、日常开发、课程项目、后端/脚本/AI工程，以及解释现代 Python 工程化工具链。
> 本文以 Windows 为主要示例，同时尽量解释 uv 背后的原理，而不是只给命令。

---

## 一、uv 是什么

`uv` 是 Astral 开发的现代 Python 包与项目管理工具，核心实现语言是**Rust**。

最容易理解的方法，是把传统 Python 开发中经常分散给多个工具的职责放在一起看：

| 传统工具/概念 | 主要职责 | uv 中对应能力 |
| --- | --- | --- |
| Python 安装器 / pyenv | 管理 Python 版本 | `uv python ...` |
| `venv` / virtualenv | 创建虚拟环境 | uv 自动维护项目 `.venv`，也可 `uv venv` |
| `pip` | 安装 Python 包 | `uv add` / `uv pip ...` |
| requirements.txt | 描述部分依赖 | `pyproject.toml` |
| pip-tools 等 | 锁定完整依赖树 | `uv.lock` |
| 手工激活环境后运行 | 选择正确解释器和依赖 | `uv run ...` |
| pipx | 隔离安装 CLI 工具 | `uv tool ...` / `uvx` |

因此，uv **不是一种新的 Python，也不是 Python 解释器**。你写的仍然是普通 Python，变化的是 Python 项目的**工程管理方式**。

### 为什么会出现 uv

传统 Python 工程并不是不能用，而是工具链容易碎片化：

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install requests
pip install pytest
pip freeze > requirements.txt
python main.py
```

如果还要管理多个 Python 版本、锁定依赖、安装 CLI 工具，又可能引入 pyenv、virtualenv、pip-tools、pipx、Poetry、Conda 等。问题不在于这些工具差，而在于开发者需要理解并维护多个工具之间的边界。uv 的思路是：

> 尽量提供一个统一、高性能、可复现的 Python 工程工具链。

```powershell
uv init
uv add requests
uv add pytest --dev
uv run python main.py
```

依赖信息写入项目配置，解析结果进入锁文件，项目虚拟环境由 uv 管理。

### 为什么 uv 用 Rust

uv 的核心实现语言是 **Rust**，而不是 Python。这并不意味着"Rust 一定比 Python 好"，而是 uv 属于一种非常适合系统语言实现的工具：

- 大量文件系统访问、网络下载、压缩包处理、哈希计算、依赖解析、并发任务、进程启动、跨平台二进制分发

Rust 的特点包括：

1. **原生编译**：可以生成独立的原生可执行程序。
2. **高性能**：适合依赖解析、缓存、文件操作等高频任务。
3. **内存安全**：通过所有权、借用等机制在编译期规避大量内存安全问题。
4. **并发能力强**：适合并行下载、解析和文件处理。
5. **部署方便**：用户安装 uv 时不需要"先有一个 Python 才能运行 uv"。

最后一点尤其重要。如果一个 Python 环境管理器本身完全依赖某个已经正确安装的 Python，那么会出现一种"先有鸡还是先有蛋"的问题。uv 作为独立原生程序，可以直接下载 CPython、创建虚拟环境、安装项目依赖、运行 Python 项目，使 Python 本身也可以成为 uv 管理的资源。

---

## 二、四个核心概念

刚接触 uv 时，最容易把以下四种东西混为一谈：① uv 工具本身 ② CPython ③ 项目虚拟环境 ④ uv 缓存。必须分清。

### 2.1 uv 本体

Windows 上通常就是 `uv.exe`、`uvx.exe`，它们是工具程序。例如自定义安装到 `D:\dev\python\uv\`。这里不是项目依赖仓库，也不是 Python 环境。

### 2.2 uv 管理的 CPython

```powershell
uv python install 3.13
```

uv 可以下载并管理 CPython。例如自定义到 `D:\dev\python\uv-python\`，可能看到：

```text
cpython-3.13.15-windows-x86_64-none
cpython-3.13-windows-x86_64-none
```

这不一定表示重复安装了两份 Python，其中一个通常是真正的具体版本目录，另一个可能是 uv 维护的版本入口/链接。**原则：不要手工整理 uv 的 Python 管理目录。** 使用 `uv python list` / `uv python find 3.13` / `uv python dir` 来查询真实状态。

### 2.3 项目 `.venv`

假设项目目录 `D:\dev\projects\project-a\`，执行 `uv init` / `uv python pin 3.13` / `uv add numpy` 后，项目可能形成：

```text
project-a\
├── .venv\
├── .python-version
├── pyproject.toml
├── uv.lock
└── main.py
```

这里的 `.venv` 才是 **project-a 自己的 Python 虚拟环境**。NumPy 真正可导入的安装位置通常类似 `project-a\.venv\Lib\site-packages\numpy\`，而不是 `D:\dev\python\uv-python\numpy`。

### 2.4 uv cache

uv 还会维护共享缓存。查看：`uv cache dir`。缓存的作用可以类比为"仓库"：

```text
第一次项目 A 需要某个包：
网络 → uv cache → project-a\.venv

以后项目 B 也需要相同内容时：
uv cache → project-b\.venv
```

> cache 是共享缓存，不是项目运行环境。清理缓存：`uv cache clean`，但日常通常没有必要频繁清理。

### 仓库类比

可以把整个体系想成"中央仓库 + 各项目独立工作间"：

```text
                    uv
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
   Python 仓库     包缓存仓库    项目管理
        │            │            │
   CPython 3.13   下载缓存      project A
   CPython 3.14                 project B
```

每个项目都有自己的工作间（`.venv`），A 安装 NumPy 不意味着 B 自动拥有 NumPy。这就是**环境隔离**。

---

## 三、Python 版本管理

### 安装与查看

```powershell
uv python install 3.13          # 安装
uv python install 3.12 3.13 3.14  # 管理多个版本
uv python list                  # 查看已安装版本
uv python dir                   # 查看 Python 管理目录
uv python find 3.13             # 查找指定解释器
```

### 为项目固定 Python

进入项目执行 `uv python pin 3.13`，项目通常会产生 `.python-version` 文件，表示"这个项目希望使用 Python 3.13"。于是 project A → Python 3.13、project B → Python 3.12、project C → Python 3.14 可以同时存在。

---

## 四、项目结构与依赖管理

### 为什么项目需要虚拟环境

假设两个项目：A 需要 package-x 1.x，B 需要 package-x 2.x。如果所有包都安装到全局 Python，就可能产生版本冲突。使用虚拟环境：

```text
CPython 3.13
   │
   ├── project-A\.venv
   │      └── package-x 1.x
   │
   └── project-B\.venv
          └── package-x 2.x
```

两个项目可以同时正常存在。所以现代 Python 开发的基本原则之一就是：

> 项目依赖尽量不要污染全局 Python。

### 创建第一个 uv 项目

```powershell
cd D:\dev\projects
uv init hello-python
cd hello-python
uv python pin 3.13
uv add requests
uv run python main.py
```

首次需要时，uv 会准备项目环境。

### `pyproject.toml` 是什么

现代 Python 项目越来越倾向于使用 `pyproject.toml` 作为项目的核心配置文件之一。它可以描述：项目名称、版本、Python 版本要求、直接依赖、开发依赖/依赖组、构建系统、其他工具配置。例如：

```toml
[project]
name = "demo"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "requests>=2.32",
]
```

执行 `uv add requests` 时，uv 会帮助修改项目依赖配置，而不是要求你手工维护所有细节。

### `uv.lock` 是什么

这是理解"可复现环境"的关键。假设你声明 `requests >= 某个版本`，requests 自己还依赖其他包。真正运行环境不是只有 requests，而是一棵依赖图：

```text
项目
└── requests
    ├── urllib3
    ├── certifi
    ├── idna
    └── charset-normalizer
```

可以类比：

```text
pyproject.toml = 菜单 / 需求说明
uv.lock        = 最终采购清单
.venv          = 真正做出来的一桌菜
```

`pyproject.toml` 更接近"我这个项目需要什么"，`uv.lock` 更接近"经过解析后，这套项目环境具体锁定成什么版本组合"。

### 为什么 `.venv` 通常不提交 Git

因为 `.venv` 是可以重建的运行环境。项目真正重要的是源代码、`pyproject.toml`、`uv.lock`。另一台机器拿到代码后执行 `uv sync` 就可以根据项目配置与锁文件恢复环境。所以一般 `.venv/` 会放进 `.gitignore`。

这是一个非常重要的工程思想：

> 保存"如何构建环境的声明"，而不是把整个环境本身当源代码保存。

---

## 五、常用命令

### `uv add`：添加项目依赖

```powershell
uv add requests
uv add numpy pandas matplotlib
uv add pytest --dev       # 开发阶段依赖
uv remove requests        # 删除依赖
```

uv 会同步维护项目元数据和环境。这比随手 `pip install xxx` 更强调"项目依赖必须被项目记录"。

### `uv run`：为什么很好用

传统方式需要先 `.venv\Scripts\Activate.ps1` 再 `python main.py`，uv 可以直接：

```powershell
uv run python main.py
uv run python --version
uv run pytest
```

可以理解为：uv 帮我找到当前项目应该使用的 Python 环境，然后在那个环境里执行命令。这可以减少"我现在到底激活的是哪个环境"的混乱。

### 还能不能手工激活 `.venv`

可以。Windows PowerShell：

```powershell
.venv\Scripts\Activate.ps1     # 激活
deactivate                      # 退出
```

uv 没有废除传统虚拟环境概念，它只是让很多场景可以直接 `uv run ...` 而不必手工激活。

### `uv sync`：恢复项目环境

假设你从 Git 克隆了一个项目：

```powershell
git clone ...
cd project
uv sync
```

把项目环境同步到应有状态。可以把 `sync` 理解为：

> 根据项目声明和锁文件，把本地 `.venv` 恢复成项目要求的环境。

---

## 六、uv 与其他工具的关系

### uv 与 pip

pip 仍然是 Python 生态非常重要的基础工具，你应该**理解 pip**，因为：很多课程还使用 pip、很多旧项目使用 requirements.txt、大量文档仍提供 pip 安装命令、面试可能问 pip / venv。但新项目日常管理可以优先使用 uv。

```powershell
pip install requests         # 传统方式
uv add requests              # uv 项目方式
uv pip install -r requirements.txt  # 兼容旧项目
```

> pip 是 Python 包安装领域的重要基础工具；uv 在其上方提供了更完整的项目、环境、版本和依赖管理工作流，并提供 pip 兼容接口。

### uv 与 venv

`venv` 是 Python 标准库提供的虚拟环境机制。传统创建 `python -m venv .venv`，uv 可以自己创建 `uv venv`。在 uv 项目工作流中，很多时候甚至不用单独执行它，因为 `uv add` / `uv sync` / `uv run` 会围绕项目环境工作。

```text
venv = 虚拟环境这个基础机制/工具
uv   = 更高层的项目与环境管理工具
```

### uv 与 Conda / Miniconda

这是非常容易混淆的地方。**uv** 更聚焦于现代 Python 工程（Python 版本、项目、虚拟环境、包、依赖解析、锁文件、CLI 工具）；**Conda** 不只是 Python 包管理器，它还是跨语言的软件包与环境管理系统，可以管理 Python，也能管理很多非 Python 二进制依赖。因此某些科学计算、数据科学、ML、CUDA、原生库依赖复杂的环境、明确要求 conda 的课程，仍可能非常适合 Conda。

一种清晰的分工：

```text
Windows Python 工具链
│
├── uv
│   ├── 日常 Python 项目、后端、自动化脚本、爬虫、AI API / Agent 工程
│   └── 普通 Python 学习项目
│
└── Miniconda
    ├── 明确要求 Conda 的课程
    ├── 特殊数据科学环境
    ├── 特殊 ML 环境
    └── 非 Python 二进制依赖较复杂的环境
```

核心原则：

> 同一个项目尽量选择一个主要环境/依赖管理体系，不要一边让 uv 管、一边又让 conda 随意改同一环境。

### Jupyter 与 Conda 没有绑定

常见误解："Jupyter 是不是只能安装在 Anaconda / Conda 里？"不是。JupyterLab 本质上也是软件包，可以存在于不同 Python 环境中：

```powershell
conda install jupyterlab   # conda 环境
# 也可以在普通 Python/uv 管理的环境中使用
```

```text
Jupyter ≠ Conda
Jupyter ≠ Anaconda
```

Anaconda 只是历史上经常把 Jupyter 和数据科学工具预装在一起，所以很多初学者形成了绑定印象。

---

## 七、实践技巧

### 基础 CPython 可以被很多项目共用

例如 `D:\dev\python\uv-python\` 下的 CPython 3.13.x 可以作为多个项目创建/运行环境时的基础解释器来源：

```text
              CPython 3.13
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
 project-A      project-B    project-C
   .venv          .venv        .venv
```

这些项目使用同一 Python **版本来源**，但项目依赖环境相互隔离。注意："共享基础 Python"不等于"共享项目依赖"。

### NumPy 到底装在哪里

```powershell
cd project-a
uv add numpy
uv run python -c "import numpy; print(numpy.__file__)"
```

真正供项目导入的 NumPy 通常在 `project-a\.venv\Lib\site-packages\`。这也是排查"到底用了哪个包"的好方法。

### 如何查看当前到底用了哪个 Python

```powershell
uv run python -c "import sys; print(sys.executable)"  # 查看解释器路径
uv run python --version                                # 查看版本
where.exe python                                       # 手工激活后查看
```

调试 Python 环境问题时，第一件事通常不是重装，而是确认：

> 我当前到底运行的是哪个 python.exe？

### 推荐的 Windows 目录管理思路

一种清晰布局：

```text
D:\dev\python\
├── miniconda3\
├── uv\
├── uv-python\
└── uv-cache\
```

职责：

```text
miniconda3  → Conda 体系
uv          → uv.exe / uvx.exe
uv-python   → uv 管理的 CPython
uv-cache    → uv 共享缓存
```

而项目不要放进 uv 的内部目录：

```text
D:\dev\projects\
├── project-a\
│   ├── .venv\
│   ├── pyproject.toml
│   └── uv.lock
│
└── project-b\
    ├── .venv\
    ├── pyproject.toml
    └── uv.lock
```

> 工具归工具，解释器归解释器，缓存归缓存，项目环境跟项目走。

### 自定义 uv Python 与缓存目录

uv 管理的 Python 可以通过环境变量指定位置：

```powershell
[Environment]::SetEnvironmentVariable(
    "UV_PYTHON_INSTALL_DIR",
    "D:\dev\python\uv-python",
    "User"
)

[Environment]::SetEnvironmentVariable(
    "UV_CACHE_DIR",
    "D:\dev\python\uv-cache",
    "User"
)
```

重新打开终端后用 `uv python dir` / `uv cache dir` 确认即可。目录管理的目标应该是"可理解、可维护、可恢复"，而不是"所有文件必须物理集中到一个文件夹"。

---

## 八、日常工作流与示例

### 推荐的日常工作流

```powershell
# 新建项目
uv init my-project
cd my-project

# 固定 Python
uv python pin 3.13

# 添加运行依赖
uv add requests

# 添加开发依赖
uv add pytest --dev

# 运行
uv run python main.py

# 测试
uv run pytest

# 删除依赖
uv remove requests

# 恢复/同步环境
uv sync
```

### 一个完整示例

```powershell
uv init weather-demo
cd weather-demo
uv python pin 3.13
uv add requests
```

`main.py`：

```python
import requests

print("requests:", requests.__version__)
```

运行：

```powershell
uv run python main.py
uv run python -c "import sys; print(sys.executable)"
```

### 如果 Git 克隆了别人的 uv 项目

```text
开发者 A
   │
   └── 提交源代码 + pyproject.toml + uv.lock
          ↓
       Git 仓库
          ↓
开发者 B
   │
   └── uv sync
          ↓
     重建项目环境
```

---

## 九、面试速答

### Q: 为什么不用全局 pip？

> 全局安装依赖容易造成不同项目之间的版本冲突，也会让环境状态难以复现。现代 Python 项目通常为每个项目建立隔离的虚拟环境，并通过项目配置和锁文件记录依赖。uv 可以自动维护项目 `.venv`，同时通过 `pyproject.toml` 和 `uv.lock` 管理声明依赖和解析后的精确依赖，从而提高隔离性和可复现性。

### Q: 锁文件为什么重要？

> 只声明直接依赖并不能完全描述实际运行环境，因为直接依赖还会引入传递依赖，而且允许的版本范围可能随着时间解析出不同结果。锁文件记录经过依赖解析后的具体版本组合，使开发、CI 和部署环境更容易保持一致。uv 使用 `uv.lock` 来实现这一类可复现环境管理。

关键词：direct dependency（直接依赖）、transitive dependency（传递依赖）、dependency resolution（依赖解析）、lockfile（锁文件）、reproducibility（可复现性）。

### Q: uv 与 Conda 的区别？

> uv 更聚焦 Python 工程，包括 Python 版本、虚拟环境、PyPI 依赖、锁文件和项目运行等；Conda 是更广义的跨语言包和环境管理系统，在科学计算、机器学习以及复杂原生二进制依赖场景中仍有独特价值。因此二者不是简单的完全替代关系，可以按项目需求选择，但同一个项目最好明确主要环境管理工具，避免混用造成状态不一致。

### Q: uv 的价值是什么？

> 我把 uv 理解为 Python 工程的统一管理层。传统 Python 开发中，解释器版本、venv、pip、依赖锁定和 CLI 工具可能分别由不同工具负责。uv 尝试把这些能力整合到一个高性能工具中。它由 Rust 实现，本身可以作为独立原生程序运行，因此甚至能够反过来安装和管理 CPython。项目层面通过 `pyproject.toml` 描述依赖，通过 `uv.lock` 锁定解析结果，每个项目使用独立 `.venv`，再通过 `uv run` 在正确环境中执行命令。它的核心价值不是单纯"安装包更快"，而是降低 Python 工程环境管理的复杂度，并增强可复现性。

---

## 十、常见误区

### 误区 1：uv 是一种 Python

错误。`uv = 管理工具`，`CPython = Python 解释器`。

### 误区 2：装了 uv 就不用 Python

不准确。你仍然需要 Python 解释器，只是解释器可以由 uv 自动安装和管理。

### 误区 3：所有项目共用一份 NumPy

通常不是。`project-A\.venv` 和 `project-B\.venv` 各有自己的 NumPy。缓存可以共享，但运行环境隔离。

### 误区 4：`uv-python` 是 uv 的所有数据

不是。它只是 uv 管理的 Python 安装目录之一。

### 误区 5：uv-cache 就是 Python 包安装目录

不是。cache 是缓存。项目真正运行时使用的依赖通常在 `.venv`。

### 误区 6：有 uv 就不需要理解 pip / venv

不建议这么想。学习 Python 工程仍然应该理解：pip 是什么、venv 是什么、site-packages 是什么、PATH 是什么、Python 解释器是什么、虚拟环境为什么隔离。uv 是在这些基础概念之上提供更好的工程体验。

### 误区 7：Jupyter 只能通过 Conda 使用

错误。Jupyter 与 Conda 没有绑定，可以存在于不同 Python 环境中。

---

## 十一、常用命令速查

```powershell
# uv 自身
uv --version
where.exe uv

# Python 版本管理
uv python install 3.13
uv python list
uv python dir
uv python find 3.13
uv python pin 3.13

# 项目
uv init
uv init my-project
uv sync

# 依赖
uv add requests
uv add numpy pandas
uv add pytest --dev
uv remove requests

# 运行
uv run python
uv run python main.py
uv run pytest

# 虚拟环境
uv venv
.venv\Scripts\Activate.ps1
deactivate

# 环境诊断
uv run python --version
uv run python -c "import sys; print(sys.executable)"
uv run python -c "import numpy; print(numpy.__file__)"

# 缓存
uv cache dir
uv cache clean

# 兼容 requirements.txt
uv pip install -r requirements.txt
```

---

## 十二、抽象模型与学习路线

### 应该掌握的抽象模型

不要把 uv 学成一组需要背诵的命令。更重要的是建立下面这个模型：

```text
                    项目
                     │
              pyproject.toml
              "我需要什么"
                     │
                     ↓
                 uv 解析
                     │
                     ↓
                  uv.lock
             "最终锁成什么"
                     │
                     ↓
                   .venv
             "实际运行环境"
                     │
                     ↓
                uv run ...
```

与此同时：`uv python` 提供/选择 Python 解释器；`uv cache` 减少重复下载和构建。整个系统可以浓缩为：

```text
Python 版本 + 项目依赖声明 + 依赖解析/锁定 + 隔离虚拟环境 + 可复现执行 = 现代 Python 工程环境
```

uv 的作用，就是尽量把这些步骤统一起来。

### 从"会用"到"真正理解"的学习路线

- **第一层·会使用**：`uv init` / `uv add` / `uv run` / `uv sync`
- **第二层·理解环境**：CPython / `.venv` / site-packages / PATH / 解释器路径
- **第三层·理解依赖**：直接依赖 / 传递依赖 / 版本约束 / 依赖解析 / 锁文件
- **第四层·理解工程化**：`pyproject.toml` / `uv.lock` / Git / CI / 开发环境 / 部署环境 / 可复现性
- **第五层·能够比较工具**：uv vs pip / venv / pipx / Poetry / Conda，以及 Miniconda vs Anaconda

这时你对 uv 的理解就不再是"会敲几个命令"，而是能够解释：

> Python 项目为什么需要环境管理，以及 uv 在整个 Python 工程体系里解决了什么问题。

### 最终记忆版

只记住一句话：

> **uv 是用 Rust 实现的现代 Python 工程管理工具，它可以管理 Python 版本、项目虚拟环境、依赖、锁文件和命令运行；CPython 是解释器，`.venv` 是每个项目隔离的运行环境，`pyproject.toml` 描述项目需要什么，`uv.lock` 锁定最终依赖组合，uv cache 则负责共享缓存。**

再记住日常主线：

```powershell
uv init
uv python pin 3.13
uv add <package>
uv run python main.py
uv sync
```

理解这条主线以后，再学习其他 uv 子命令会容易很多。

### 进一步学习时值得研究的问题

1. `pyproject.toml` 的标准结构和 PEP 体系。
2. wheel、sdist 分别是什么。
3. Python 包为什么会有二进制兼容问题。
4. dependency resolver 如何解决版本约束。
5. lockfile 如何支持 CI/CD 可复现构建。
6. uv workspace 如何管理多包项目。
7. `uv tool` / `uvx` 与 pipx 的关系。
8. uv 如何使用全局缓存提高多个虚拟环境之间的存储与安装效率。
9. Conda 的 package/environment model 与 PyPI/virtualenv model 有什么本质差异。
10. Rust 为什么越来越常用于 Python 生态中的高性能基础设施工具。

如果能把这些问题逐步讲清楚，你掌握的就不仅是 uv，而是一整套现代 Python 工程环境模型。

---

## 可执行实践闭环

1. **练习任务**：用 `pyproject.toml` 和锁文件创建可复现 Python 项目。
2. **完成标准**：区分项目依赖、工具安装和 Python 版本管理，确保 CI 使用锁定环境。
3. **排错顺序**：检查解释器选择、锁文件变化、索引配置和构建后端。
4. **沉淀结果**：保留最小示例、关键命令、失败现象与解决依据；过一周不看原文重新完成一次。
