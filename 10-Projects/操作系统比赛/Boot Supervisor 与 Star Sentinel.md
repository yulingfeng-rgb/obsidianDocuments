# Boot Supervisor 与 Star Sentinel

---

## 一、项目概述

Boot Supervisor 是 StarryOS 启动问题的"哨兵"框架实现，用于 Host 侧启动监督、诊断、修复建议与报告。它是 AskingStar 项目启动链路（BootRunner → BootGate → KernelSoil）在 Host 侧的对应工具。

### 1.1 核心概念关系

**Star Sentinel（哨兵）和 Boot Supervisor 不是同一个东西，但它们密切相关：**

- **Star Sentinel（哨兵）** 是一个**框架理念和品牌名称**
- **Boot Supervisor** 是哨兵框架的**具体实现代码**

| Star Sentinel（哨兵） | Boot Supervisor |
|----------------------|-----------------|
| 框架理念 / 架构规范 | 具体 Python 代码实现 |
| 定义设计模式与插件体系 | 实现启动监督编排器 |
| 品牌标识 | 产品实体 |

**三者关系**：
1. 哨兵是"道"（理念），Boot Supervisor 是"术"（实现）
2. 哨兵是"框架"，Boot Supervisor 是"应用"
3. 哨兵是"品牌"，Boot Supervisor 是"产品"

### 1.2 项目定位

- **类型**: Host 侧启动监督、诊断、修复与报告框架
- **语言**: Python 3（依赖仅 `tomli`，Python 3.11+ 无需额外依赖）
- **运行环境**: Host 侧（不是 StarryOS guest 内部组件）
- **版本**: v1.3
- **角色**: Boot Supervision Orchestrator（启动监督编排器）
- **配置格式**: TOML

### 1.3 与 AskingStar 的关系

Boot Supervisor 与 AskingStar 的 C++ 启动链路互为补充：

| 维度 | Boot Supervisor（Python） | AskingStar Boot（C++） |
| --- | --- | --- |
| **定位** | Host 侧启动监督工具 | 嵌入式启动编排引擎 |
| **运行位置** | 开发机 / CI Host | AskingStar 运行时内部 |
| **核心能力** | 环境检查→构建→QEMU→串口→诊断→修复建议 | BootRunner→BootGate→ChargeToken→KernelSoil |
| **输出** | Markdown 报告 + JSONL 日志 | EventStore + GeneBank 记录 |
| **适配模式** | DryRun / Real QEMU | DryRun / HostMock / BoardReal |

AskingStar 的 `BootRunner` 通过 `JianmuArtifactIndex` 选择产物，经 `BootGate` 生成 `ChargeToken`（Force/Law/Wisdom 协议），再由 `KernelSoilBootAdapter` 分发启动。Boot Supervisor 则在更早期的开发调试阶段，对 StarryOS ELF 产物、地址布局、QEMU 启动进行自动化监督和诊断。

---

## 二、系统架构

### 2.1 整体架构图

```
┌──────────────────────────────────────────────────────────────┐
│                 Star Sentinel 框架理念                         │
│                                                              │
│  核心设计模式：                                               │
│  - 插件化架构（SPI，7 种 PluginKind）                         │
│  - 责任链模式（Pipeline 优先级链式执行）                       │
│  - 观察者模式（EventBus + 3 种 Observer）                     │
│  - 状态机（BootStateMachine 启动状态管理）                    │
│  - 策略模式（RepairPlugin 分级修复策略）                      │
│  - 配置中心（LocalConfigCenter，TOML 驱动）                  │
│                                                              │
│  核心能力：                                                   │
│  - 启动监督（Boot Supervision）                               │
│  - 故障诊断（8 种 Monitor 自动检测）                          │
│  - 修复建议（7 种 RepairPlugin 分级修复）                    │
│  - 报告生成（Markdown + JSONL）                               │
│  - 热度评分（HeatScorer 量化启动健康度）                      │
│  - 权限控制（PermissionGuard 细粒度权限）                     │
│  - 地址空间建模（ELF64 解析 + VA/PA 一致性校验）             │
└──────────────────────────┬───────────────────────────────────┘
                           │ 实现
                           ▼
┌──────────────────────────────────────────────────────────────┐
│              Boot Supervisor 代码实现                          │
│                                                              │
│  核心组件（core/）：                                          │
│  - scheduler.py           → SupervisorScheduler 监工调度器    │
│  - state_machine.py       → BootStateMachine 启动状态机       │
│  - event_bus.py           → EventBus 事件总线                 │
│  - registry.py            → SPIRegistry 插件注册表            │
│  - spi_registry.py        → SPI 扩展注册                      │
│  - permission.py          → PermissionGuard 权限守卫          │
│  - heat.py                → HeatScorer 热度评分器             │
│  - config_center.py       → LocalConfigCenter 配置中心        │
│  - context.py             → BootContext 共享上下文             │
│  - result.py              → HandlerResult 标准化结果          │
│  - repair_orchestrator.py → RepairOrchestrator 修复编排器     │
│                                                              │
│  处理器（handlers/）：8 种                                    │
│  监控器（monitors/）：8 种                                    │
│  观察者（observers/）：3 种                                   │
│  适配器（adapters/）：9 种                                    │
│  修复插件（plugins/repair/）：7 种                            │
│  SPI 定义（spi/）：PluginKind + PluginMode + Protocol         │
│  工具（tools/）：ELF64 解析器 + 地址空间模型                  │
└──────────────────────────────────────────────────────────────┘
```

### 2.2 目录结构

```text
boot_supervisor/
├── main.py                              # CLI 入口
├── requirements.txt                     # 依赖（仅 tomli for Python<3.11）
├── README.md                            # 使用说明
│
├── core/                                # 核心组件（11 个模块）
│   ├── context.py                       # BootContext 共享上下文
│   ├── config_center.py                 # LocalConfigCenter TOML 配置中心
│   ├── scheduler.py                     # SupervisorScheduler 监工调度器
│   ├── state_machine.py                 # BootStateMachine 启动状态机
│   ├── event_bus.py                     # EventBus 观察者事件总线
│   ├── registry.py                      # SPIRegistry 插件注册表
│   ├── spi_registry.py                  # SPI 扩展注册
│   ├── permission.py                    # PermissionGuard 权限控制
│   ├── heat.py                          # HeatScorer 热度评分
│   ├── result.py                        # HandlerResult 标准化结果
│   └── repair_orchestrator.py           # RepairOrchestrator 修复编排器
│
├── handlers/                            # 处理器（8 个 Pipeline Handler）
│   ├── env_check.py                     # 环境检查（工具链存在性）
│   ├── build.py                         # 构建代理（make aarch64-build）
│   ├── artifact_check.py               # 产物检查（ELF/bin/rootfs 存在性）
│   ├── address_space_model.py           # 地址空间模型（VA/PA 一致性）
│   ├── layout_check.py                  # 布局检查（ELF 段布局）
│   ├── qemu_launch.py                   # QEMU 启动
│   ├── serial_capture.py                # 串口采集
│   └── stage_detect.py                  # 启动阶段检测
│
├── monitors/                            # 监控器（8 个自动诊断器）
│   ├── data_abort.py                    # Data Abort 异常检测
│   ├── no_output.py                     # 无串口输出检测
│   ├── shell_ready.py                   # Shell 就绪检测
│   ├── block_device_missing.py          # 块设备缺失检测
│   ├── executable_path_missing.py       # 可执行路径缺失检测
│   ├── elf_load_address.py             # ELF LOAD 地址校验（VMA/PA）
│   ├── boot_loader_alignment.py         # .boot_loader 4K 对齐检测
│   └── rootfs_shell.py                  # rootfs /bin/sh 存在性检测
│
├── observers/                           # 观察者（3 种输出）
│   ├── console_observer.py              # 终端实时输出
│   ├── jsonl_observer.py                # JSONL 事件日志
│   └── markdown_reporter.py             # Markdown 诊断报告
│
├── adapters/                            # 工具适配器（9 种外部工具）
│   ├── base.py                          # ToolAdapter 基类 + ToolResult
│   ├── qemu_adapter.py                  # QEMU 适配器
│   ├── binutils_adapter.py              # readelf/objdump/nm 适配器
│   ├── git_adapter.py                   # Git 适配器
│   ├── pytest_adapter.py                # Pytest 适配器
│   ├── gdb_adapter.py                   # GDB 适配器
│   ├── dtc_adapter.py                   # DTC/FDT 适配器
│   ├── renode_adapter.py                # Renode 适配器
│   └── robot_adapter.py                 # Robot Framework 适配器
│
├── plugins/                             # 插件系统
│   ├── __init__.py
│   ├── analyzers/__init__.py            # 分析器插件占位
│   ├── guards/__init__.py               # 守卫插件占位
│   ├── fault_injection/__init__.py      # 故障注入插件占位
│   └── repair/                          # 修复插件（7 种）
│       ├── __init__.py                  # REPAIR_PLUGINS 注册表
│       ├── base.py                      # ReportOnlyRepairPlugin 基类
│       ├── address_mapping_repair.py    # P0 地址映射修复
│       ├── qemu_script_repair.py        # QEMU 脚本修复
│       ├── platform_config_repair.py    # 平台配置修复
│       ├── early_uart_injector.py       # P1 早期 UART 注入
│       ├── minimal_vbar_injector.py     # 最小 VBAR 注入
│       ├── linker_script_patch_suggester.py  # 链接脚本补丁建议
│       └── bootloader_reloc_patch_suggester.py # Bootloader 重定位建议
│
├── spi/                                 # SPI 插件协议定义
│   ├── handler.py                       # Handler 协议
│   └── plugin.py                        # PluginKind + PluginMode + RepairPlugin
│
├── tools/                               # 独立工具
│   ├── address_space_model.py           # ELF64 解析 + 地址空间一致性校验
│   └── address_mapping_repair.py        # 地址映射修复工具
│
├── workflows/                           # 工作流定义
│   └── star_sentinel_v1_3.toml          # v1.3 工作流
│
├── config/                              # 配置文件
│   ├── supervisor.toml                  # DryRun 配置（默认）
│   └── supervisor.real-qemu.toml        # 真实 QEMU 配置
│
└── tests/                               # 单元测试（17 个测试文件）
    ├── test_address_mapping_repair.py
    ├── test_boot_stage_monitors.py
    ├── test_build_handler.py
    ├── test_config_center.py
    ├── test_data_abort_monitor.py
    ├── test_env_check.py
    ├── test_event_bus.py
    ├── test_heat.py
    ├── test_layout_check.py
    ├── test_markdown_reporter.py
    ├── test_no_output_monitor.py
    ├── test_permission.py
    ├── test_qemu_launch.py
    ├── test_registry.py
    ├── test_repair_orchestrator.py
    ├── test_scheduler.py
    └── test_state_machine.py
```

---

## 三、核心模块详解

### 3.1 核心组件（core/）

#### 3.1.1 BootContext（共享上下文）

所有 Handler、Monitor、Observer 共享的运行时上下文：

```python
@dataclass
class BootContext:
    repo_root: Path           # 仓库根目录
    config_path: Path         # 配置文件路径
    config: dict              # 解析后的配置
    event_bus: EventBus       # 事件总线
    permission_guard: PermissionGuard  # 权限守卫
    facts: dict               # 事实收集器（各 Handler 写入）
    events: list              # 事件日志
    state: dict               # 状态字典
    config_center: LocalConfigCenter   # 配置中心
```

#### 3.1.2 LocalConfigCenter（配置中心）

基于 TOML 的配置中心，支持：
- Python 3.11+ 内置 `tomllib`，低版本回退 `tomli`
- 自动推导 `repo_root`
- 路径解析（相对路径 → 绝对路径）
- Pipeline 排序与过滤

#### 3.1.3 HandlerResult（标准化结果）

```python
class HandlerStatus: PASS | FAIL | SKIP | PERMISSION_DENIED
class FailureReason: NONE | PERMISSION_DENIED | MISSING_PATH | INVALID_ARTIFACT | CONFIG_ERROR | EXECUTION_DISABLED | UNKNOWN
```

#### 3.1.4 SupervisorScheduler（监工调度器）

按 Pipeline 优先级顺序执行 Handler，支持 `on_fail = "stop" | "continue"` 策略。

#### 3.1.5 RepairOrchestrator（修复编排器）

根据事件类型自动选择修复插件，支持三种模式：
- `AUTO`：自动执行（P0 非破坏性记录）
- `ASSISTED`：辅助模式（需人工确认）
- `REPORT_ONLY`：仅报告

#### 3.1.6 HeatScorer（热度评分器）

基于配置的 `bootability` 评分规则量化启动健康度：

| 条件 | 分数 |
| --- | --- |
| 完全通过 | 100 |
| raw entry 不匹配 | 15 |
| 布局检查失败 | 25 |
| 产物检查失败 | 35 |
| QEMU 启动失败 | 45 |
| 运行时异常 | 55 |
| 构建失败 | 0 |

### 3.2 SPI 插件系统

#### PluginKind（7 种插件类型）

| 类型 | 用途 |
| --- | --- |
| `handler` | Pipeline 处理器 |
| `guard` | 权限守卫 |
| `analyzer` | 分析器 |
| `observer` | 观察者 |
| `repair` | 修复插件 |
| `fault_injection` | 故障注入 |
| `report` | 报告生成 |

#### PluginMode（3 种执行模式）

| 模式 | 含义 |
| --- | --- |
| `AUTO` | 自动执行 |
| `ASSISTED` | 辅助模式，需人工确认 |
| `REPORT_ONLY` | 仅报告，不执行 |

### 3.3 处理器（handlers/）

Pipeline 执行顺序（默认配置）：

| 优先级 | Handler | 功能 | on_fail |
| --- | --- | --- | --- |
| 100 | **EnvCheckHandler** | 检查工具链（qemu/make/cargo/readelf 等） | stop |
| 200 | **BuildHandler** | 代理 StarryOS 构建 | stop |
| 300 | **LayoutCheckHandler** | 检查 ELF 段布局 | continue |
| 350 | **AddressSpaceModelHandler** | 校验 VA/PA/Entry/_start 一致性 | continue |
| 400 | **QemuLaunchHandler** | 启动 QEMU | continue |
| 500 | **SerialCaptureHandler** | 捕获串口输出 | continue |
| 600 | **StageDetectHandler** | 检测启动阶段 | continue |

**新增 Handler 说明**：
- `ArtifactCheckHandler`：检查 ELF/bin/rootfs 产物是否存在（在 pipeline 中可选启用）
- `AddressSpaceModelHandler`：使用内置 ELF64 解析器校验 Linker VA、LOAD PA、`__early_boot`、`_start` 偏移的一致性

### 3.4 监控器（monitors/）

| 监控器 | 检测内容 | 事件类型 | 触发条件 |
| --- | --- | --- | --- |
| **DataAbortMonitor** | Data Abort 异常 | `DATA_ABORT` | ESR/ELR/FAR 异常 |
| **NoOutputMonitor** | 无串口输出 | `NO_KERNEL_OUTPUT` | 超时无内核日志 |
| **ShellReadyMonitor** | Shell 就绪 | `SHELL_READY` | 检测到 shell 提示符 |
| **BlockDeviceMissingMonitor** | 块设备缺失 | `BLOCK_DEVICE_MISSING` | "No block device found" |
| **ExecutablePathMissingMonitor** | 可执行路径缺失 | `EXECUTABLE_PATH_MISSING` | "Failed to resolve executable path" |
| **ElfLoadAddressMonitor** | ELF LOAD 地址 | `ELF_LOAD_ADDRESS_INVALID` | VMA 不在高半区或 PA 不在 0x40200000 附近 |
| **BootLoaderAlignmentMonitor** | .boot_loader 对齐 | `BOOT_LOADER_ALIGNMENT_INVALID` | 段地址或 LOADER_BIN 未 4K 对齐 |
| **RootfsShellMonitor** | rootfs /bin/sh | `ROOTFS_SHELL_MISSING` | rootfs 中无 /bin/sh 或串口报 NotFound |

### 3.5 修复插件（plugins/repair/）

| 插件 | 级别 | 模式 | 监听事件 | 功能 |
| --- | --- | --- | --- | --- |
| **AddressMappingRepairPlugin** | P0 | AUTO | RAW_BIN_ENTRY_MISMATCH 等 | 地址映射一致性修复（非破坏性记录） |
| **QemuScriptRepairPlugin** | P1 | ASSISTED | QEMU 启动失败 | QEMU 脚本修复建议 |
| **PlatformConfigRepairPlugin** | P1 | ASSISTED | 平台配置异常 | 平台配置修复建议 |
| **EarlyUARTInjector** | P1 | ASSISTED | NO_KERNEL_OUTPUT | 建议注入早期 UART putc 标记 |
| **MinimalVBARInjector** | P1 | ASSISTED | Data Abort | 建议注入最小 VBAR 异常向量 |
| **LinkerScriptPatchSuggester** | P2 | REPORT_ONLY | 布局异常 | 链接脚本补丁建议 |
| **BootloaderRelocPatchSuggester** | P2 | REPORT_ONLY | 重定位异常 | Bootloader 重定位补丁建议 |

### 3.6 适配器（adapters/）

| 适配器 | 工具 | 用途 |
| --- | --- | --- |
| **QemuAdapter** | qemu-system-aarch64 | QEMU 启动控制 |
| **BinutilsAdapter** | readelf/objdump/nm/addr2line | ELF 分析 |
| **GitAdapter** | git | 版本信息 |
| **PytestAdapter** | pytest | 测试执行 |
| **GdbAdapter** | gdb | 调试 |
| **DtcAdapter** | dtc/fdtdump | 设备树处理 |
| **RenodeAdapter** | renode | Renode 仿真 |
| **RobotAdapter** | robot | Robot Framework |

所有适配器继承 `ToolAdapter` 基类，提供统一的 `is_available()` 和 `run()` 接口。

### 3.7 工具（tools/）

#### AddressSpaceModelTool

独立的 ELF64 解析与地址空间一致性校验工具：

- 纯 Python 实现 ELF64 解析（不依赖外部 readelf）
- 校验 6 项一致性：ENTRY、FIRST_LOAD_VIRT、FIRST_LOAD_PHYS、`__EARLY_BOOT`、`_START_OFFSET`、`LOAD_OFFSET`
- 输出 `AddressSpaceModelResult`（含 mismatches 和 suggestions）

---

## 四、配置与工作流

### 4.1 配置文件

项目提供两套配置：

| 配置 | 文件 | 用途 |
| --- | --- | --- |
| **DryRun** | `config/supervisor.toml` | 默认配置，不执行 QEMU |
| **Real QEMU** | `config/supervisor.real-qemu.toml` | 真实构建并启动 QEMU |

关键配置段：

```toml
[sentinel]
version = "1.3"
role = "Boot Supervision Orchestrator"
core = ["state_machine", "event_bus", "spi_registry", "policy", "reports"]

[project]
root = "."
kernel_dir = "kernel/starryos"

[runtime]
log_dir = "reports/latest"
fail_fast = true
default_timeout_sec = 30

[permissions]
allow_build = true
allow_qemu = false          # DryRun 默认禁止
allow_write_reports = true
allow_repair = false
allow_patch = false

[address_space]
model = "high_va_low_lma"
linker_va = "0xffff800000000000"
load_pa = "0x40200000"
start_offset = "0x240"
early_stage = "mmu_off_until_somehal_loader"

[artifacts]
elf = "kernel/starryos/starryos_aarch64-dyn.elf"
bin = "kernel/starryos/starryos_aarch64-dyn.bin"
disk = "kernel/starryos/rootfs-aarch64.img"

[[pipeline]]
id = "env_check"
plugin = "EnvCheckHandler"
priority = 100
on_fail = "stop"
```

### 4.2 工作流定义

```toml
[workflow]
name = "star-sentinel-v1.3"
steps = ["Build", "Boot", "Observe", "Diagnose", "Suggest Patch", "Verify"]
```

**工作流程**：

1. **Build**：构建 StarryOS 内核
2. **Boot**：启动 QEMU 或 RK3588 开发板
3. **Observe**：观察启动过程，捕获串口日志
4. **Diagnose**：8 种 Monitor 自动诊断
5. **Suggest Patch**：7 种 RepairPlugin 分级修复建议
6. **Verify**：验证修复效果

---

## 五、运行与使用

### 5.1 命令行接口

```bash
# Dry-run 模式（只检查配置、产物和日志）
python3 -m boot_supervisor.main --config boot_supervisor/config/supervisor.toml --no-console

# 真实构建 + QEMU 启动
python3 -m boot_supervisor.main --config boot_supervisor/config/supervisor.real-qemu.toml --no-console

# 默认模式（终端输出 + JSONL + Markdown 报告）
python3 -m boot_supervisor.main

# 仅终端输出，不写 JSONL
python3 -m boot_supervisor.main --no-jsonl
```

### 5.2 输出产物

```text
reports/latest/
├── boot-report.md          # Markdown 诊断报告
├── boot-events.jsonl       # JSONL 事件日志
├── serial.log              # 串口输出日志
└── qemu-debug.log          # QEMU 调试日志（ESR/ELR/FAR）
```

### 5.3 单元测试

```bash
# 运行全部测试
python3 -m unittest discover -s boot_supervisor/tests

# 运行特定测试
python3 -m unittest boot_supervisor.tests.test_layout_check
python3 -m unittest boot_supervisor.tests.test_boot_stage_monitors
python3 -m unittest boot_supervisor.tests.test_config_center
python3 -m unittest boot_supervisor.tests.test_repair_orchestrator
```

---

## 六、StarryOS 启动模型

### 6.1 当前 ELF 启动模型

```text
QEMU 加载: starryos_aarch64-dyn.elf
ELF entry:  __early_boot（高半区 VMA: 0xffff800000000000）
第一个 LOAD PhysAddr/LMA: 0x40200000（低物理地址）
raw bin offset 0:  early boot preamble
_start:  在 preamble 后面，不要求 raw bin offset 0
```

### 6.2 地址空间模型

```text
Linker VA:   0xffff800000000000（高半区虚拟地址）
Load PA:     0x40200000（低物理地址，U-Boot 加载地址）
LOAD_OFFSET: VA - PA = 0xffff800000000000 - 0x40200000
_start 偏移: 0x240（somehal header offset）
早期阶段:    mmu_off_until_somehal_loader
```

### 6.3 推荐排查顺序

1. 工具链是否存在（qemu-system-aarch64 / make / cargo / readelf）
2. ELF、bin、rootfs 是否存在（ArtifactCheckHandler）
3. ELF entry、VirtAddr、PhysAddr 是否正确（AddressSpaceModelHandler）
4. raw bin offset 0 是否是 `__early_boot` preamble
5. `.boot_loader` 和 `LOADER_BIN` 是否 4K 对齐（BootLoaderAlignmentMonitor）
6. QEMU 是否挂载正确 rootfs（BlockDeviceMissingMonitor）
7. rootfs 是否有 `/bin/sh`（RootfsShellMonitor）
8. 串口是否出现 StarryOS 日志（NoOutputMonitor）
9. 如有异常，查看 `qemu-debug.log` 中的 ESR、ELR、FAR

---

## 七、与 AskingStar 的集成关系

### 7.1 启动链路对比

```text
Boot Supervisor（Host 侧 Python）:
  EnvCheck → Build → ArtifactCheck → LayoutCheck → AddressSpaceModel
  → QemuLaunch → SerialCapture → StageDetect → Monitor → RepairPlugin
  → MarkdownReport + JSONL

AskingStar Boot（C++ 运行时）:
  BootIntent → JianmuArtifactIndex → BootGate（ChargeToken/Force+Law+Wisdom）
  → GateDecisionTrace → KernelSoilBootAdapter → SoilRequest
  → BootPlan → BootResult → EventStore + GeneBank
```

### 7.2 共享概念

| 概念 | Boot Supervisor | AskingStar |
| --- | --- | --- |
| **事件驱动** | EventBus + Observer | EventStore + EventBus |
| **插件化** | SPIRegistry + PluginKind | 模块化库（asking_star_*） |
| **权限控制** | PermissionGuard | Gate + ChargeToken |
| **热度评分** | HeatScorer | RuntimeTemperature |
| **修复策略** | RepairPlugin（AUTO/ASSISTED/REPORT_ONLY） | AdmissionPolicy |
| **产物管理** | artifacts 配置段 | JianmuArtifactIndex |
| **追踪** | JSONL + Markdown | TraceEvent + TraceSink |

### 7.3 BoardReal 证据对应

Boot Supervisor 的 Monitor 输出可以为 AskingStar 的 BoardReal 证据链提供 Host 侧支撑：

| Boot Supervisor Monitor | 对应 AskingStar BoardReal 条件 |
| --- | --- |
| ElfLoadAddressMonitor | 产物合法性验证 |
| BootLoaderAlignmentMonitor | 产物合法性验证 |
| SerialCaptureHandler | UART/log 证据 |
| StageDetectHandler | 运行时间戳 + marker |
| RootfsShellMonitor | 启动成功证据 |

---

## 八、设计模式

| 设计模式 | 应用场景 | 优势 |
| --- | --- | --- |
| **插件化架构（SPI）** | 7 种 PluginKind，Handler/Monitor/Observer/Repair | 可扩展、可配置 |
| **责任链模式** | Pipeline 优先级链式执行 | 职责清晰、易于维护 |
| **观察者模式** | EventBus 事件通知（Console/JSONL/Markdown） | 解耦、灵活 |
| **状态机** | BootStateMachine 启动过程管理 | 状态清晰、易于调试 |
| **策略模式** | RepairPlugin 分级修复（AUTO/ASSISTED/REPORT_ONLY） | 可替换、可配置 |
| **适配器模式** | 9 种 ToolAdapter 统一外部工具接口 | 工具无关性 |
| **配置中心** | LocalConfigCenter TOML 驱动 | 声明式、可审计 |

---

## 九、常见问题 FAQ

**Q1：Boot Supervisor 运行在哪里？**
- 运行在 Host 侧（开发机），不是 StarryOS 内核内部组件

**Q2：Boot Supervisor 和 Star Sentinel 是什么关系？**
- Star Sentinel 是框架理念，Boot Supervisor 是具体实现

**Q3：Boot Supervisor 和 AskingStar 是什么关系？**
- Boot Supervisor 是 Host 侧的启动监督工具，AskingStar 是嵌入式运行时。二者通过产物管理和启动链路互补

**Q4：DryRun 和 Real QEMU 有什么区别？**
- DryRun 只检查配置和产物，不执行构建和 QEMU；Real QEMU 会真实构建并启动

**Q5：如何扩展 Boot Supervisor？**
- 通过 SPI 注册表添加自定义的 Handler、Monitor、Observer 或 RepairPlugin

**Q6：修复插件会自动修改代码吗？**
- 默认不会。`allow_repair = false` 和 `allow_patch = false`。P0 只做非破坏性记录，P1 需人工确认，P2 仅报告

**Q7：AddressSpaceModelTool 需要外部 readelf 吗？**
- 不需要。它用纯 Python 实现了 ELF64 解析器

---

## 附录 A：术语表

| 术语 | 全称 | 中文解释 |
| --- | --- | --- |
| SPI | Service Provider Interface | 服务提供者接口（插件机制） |
| Handler | - | 处理器（Pipeline 节点） |
| Monitor | - | 监控器（自动诊断） |
| Observer | - | 观察者（输出渲染） |
| Event Bus | - | 事件总线 |
| State Machine | - | 状态机 |
| Data Abort | - | 数据中止（ARM 异常） |
| VMA | Virtual Memory Address | 虚拟内存地址 |
| LMA | Load Memory Address | 加载内存地址 |
| PA | Physical Address | 物理地址 |
| ELF | Executable and Linkable Format | 可执行链接格式 |
| ESR | Exception Syndrome Register | 异常综合征寄存器 |
| ELR | Exception Link Register | 异常链接寄存器 |
| FAR | Fault Address Register | 故障地址寄存器 |
| VBAR | Vector Base Address Register | 向量基址寄存器 |
