# 我的 Windows 开发环境

> **用途**：记录当前电脑硬件、Windows 系统、开发环境及软件配置，作为长期维护、环境恢复及重装系统后的参考。

---

## 一、设备概览

- 品牌/型号：Lenovo Legion Y9000P IRX8（82WK）
- 设备名称：LegionZY
- 操作系统：Windows 11 家庭中文版 25H2（64 位，x64）
- CPU：Intel Core i9-13900HX（24 核 / 32 线程）
- GPU：Intel UHD Graphics + NVIDIA GeForce RTX 4060 Laptop GPU（8 GB）
- 内存：32 GB DDR5-5200（SK Hynix）
- 硬盘：Samsung MZVL21T0HCLR-00BL2（1 TB PCIe NVMe SSD）
- 屏幕：16 英寸，2560 × 1600（16:10）
- BIOS：LENOVO KWCN54WW（2025-10-22）

---

## 二、电脑硬件

### 处理器（CPU）

- Intel Core i9-13900HX
- 24 核 / 32 线程
- 基础频率：2.20 GHz

### 显卡（GPU）

**集成显卡**

- Intel UHD Graphics

**独立显卡**

- NVIDIA GeForce RTX 4060 Laptop GPU
- 显存：8 GB

### 内存

- 容量：32 GB（31.7 GB 可用）
- 品牌：SK Hynix
- 类型：DDR5
- 频率：5200 MT/s

### 存储

- 型号：Samsung MZVL21T0HCLR-00BL2
- 类型：PCIe NVMe SSD
- 容量：1 TB

### 显示器

- 尺寸：16 英寸
- 分辨率：2560 × 1600
- 比例：16:10

### 电池

- 品牌：Sunwoda
- 设计容量：80 Wh

### 主板

- 厂商：Lenovo
- 型号：LNVNB161216

### 网络

**无线网卡**

- Intel Wi‑Fi 6E AX211

**有线网卡**

- Realtek PCIe GbE Family Controller

### 声卡

- Realtek High Definition Audio (SST)：主板音频
- NVIDIA High Definition Audio：显卡 HDMI / DP 音频
- Nahimic VAD：虚拟音频设备
- Nahimic Easy Surround Device：环绕声设备
- Nahimic Mirroring Device：音频镜像设备

---

## 三、BIOS 与驱动

### BIOS

- BIOS 版本：LENOVO KWCN54WW
- BIOS 日期：2025-10-22
- BIOS 模式：UEFI
- SMBIOS：3.4
- 嵌入式控制器版本：1.51
- 安全启动：已启用

### NVIDIA

- 驱动版本：610.74
- 驱动类型：GeForce Game Ready Driver
- 发布日期：2026-07-07

### Intel

- 显卡驱动：32.0.101.6733
- 发布日期：2025-04-15

---

## 四、Windows 系统

- 系统版本：Windows 11 家庭中文版
- 版本号：25H2
- 操作系统版本：26200.8894
- 功能体验包：1000.26100.334.0
- 安装日期：2025-11-12
- 系统类型：64 位操作系统，基于 x64 处理器

---

## 五、开发环境

### Java

- JDK 8：兼容旧版 Java 项目
- JDK 17：主力 Java 开发
- Maven：Java 构建与依赖管理
- Tomcat：Java Web 容器
- Nginx：Web 服务器与反向代理
- JMeter：性能与压力测试

### Python / AI

- Anaconda：Python 环境管理

### C / C++

- MinGW-w64：GCC 编译工具链

### 数据库

- MySQL：关系型数据库
- PostgreSQL：关系型数据库

### Linux

- WSL：Windows Subsystem for Linux

### 文档

- Pandoc：文档格式转换
- TeX Live：LaTeX 排版环境

### 系统工具

- Git：版本控制
- WinGet：Windows 包管理器

### 终端

- CMD：传统命令提示符
- Windows PowerShell：Windows 自带脚本终端
- PowerShell 7：新版跨平台 PowerShell
- Windows Terminal：统一管理各种终端

---

## 六、开发工具

- Visual Studio Code：轻量级代码编辑器
- IntelliJ IDEA：Java 集成开发环境
- Trae CN：字节跳动的 AI IDE
- ChatGPT：AI 助手
- Claude Code + API：AI CLI 工具
- CCSwitch：Claude Code 模型切换工具
- Apifox：API 调试、测试与文档管理
- Obsidian：Markdown 知识管理软件

---

## 七、办公与日常软件

### 办公

- Microsoft Word：文档编辑
- Microsoft Excel：电子表格
- Microsoft PowerPoint：演示文稿
- Microsoft OneNote：笔记
- 腾讯会议：在线会议
- Trello：任务与看板管理

### 通讯

- 微信：即时通讯
- QQ：即时通讯

### 浏览器

- Microsoft Edge：默认浏览器
- Google Chrome：浏览器

### 网络

- Clash Verge：代理工具
- EasyConnect：校园 / 企业 VPN

### 工具

- Bandizip：压缩软件
- PixPin：截图、录屏与贴图
- PotPlayer：媒体播放器
- Geek Uninstaller：软件卸载工具
- 百度网盘：云存储

### Windows 自带

- Microsoft Store：应用商店
- Windows 照片：图片查看
- Windows 安全中心：系统安全

### 娱乐

- 哔哩哔哩：视频平台

---

## 八、游戏相关

- NVIDIA App：驱动更新与功能管理
- NVIDIA Control Panel：显卡控制面板
- Intel Graphics Software：Intel 核显管理
- Steam：游戏平台
- UU 加速器：游戏网络加速
- Xbox Game Bar：Windows 游戏工具

---

## 系统实践与验收

- **最小系统**：建立一份可重建清单：系统版本、硬件驱动、包管理器、终端、语言运行时和关键配置。
- **验收方法**：用脚本输出版本与路径，但不记录许可证、token、序列号、Wi-Fi 密码等秘密。
- **边界与风险**：设备状态随更新变化；本页应标注采集日期，并把个人资产信息与通用知识分开。

实践记录应包含版本、连接或部署拓扑、关键配置、原始现象、测量方法和恢复步骤，使别人能够在不同环境中复现并判断差异。
