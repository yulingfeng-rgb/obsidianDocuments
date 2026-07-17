# pip

> pip 是 Python 的包管理器，用来安装和管理第三方库。就像 Python 的"应用商店"。

**关键词**：Python、包管理、PyPI、虚拟环境、依赖

## 抽象类比

pip 像 Python 世界的快递站——你需要什么库，告诉它名字，它就帮你下载安装好。

如果说 Python 是一台手机，pip 就是 App Store——没有 pip，你得手动下载库文件、解压、配置路径。有了 pip，一行命令搞定。

---

## 什么是 pip

pip 是 Python 官方推荐的包管理工具，全称"Pip Installs Packages"（递归缩写）。它从 PyPI（Python Package Index）下载库——PyPI 上有超过 40 万个开源包。

pip 能做什么：
- **安装库**：`pip install requests`
- **管理版本**：指定安装某个版本
- **管理依赖**：根据 requirements.txt 批量安装项目需要的所有库
- **虚拟环境**：配合 venv 为每个项目创建独立环境

### 为什么需要虚拟环境

想象你有两个项目：
- 项目 A 需要 `requests==2.28`
- 项目 B 需要 `requests==2.25`

如果都装在全局环境，版本冲突。虚拟环境就是给每个项目一个"隔离的 Python"，互不干扰。

```bash
python -m venv .venv            # 创建虚拟环境
.venv\Scripts\activate          # 激活（Windows）
source .venv/bin/activate       # 激活（macOS/Linux）
deactivate                      # 退出虚拟环境
```

### pip 的常见问题

- **"下载慢"**：默认从国外 PyPI 下载，换国内源（清华、阿里云）能快 10 倍
- **"依赖冲突"**：A 库依赖 X 1.0，B 库依赖 X 2.0，pip 会报错。解决方案：用虚拟环境隔离
- **"装了但找不到"**：可能是激活了虚拟环境但包装在了全局，或者反过来

---

## 设计哲学

### 标准化包管理

pip 的核心设计：**统一 Python 包的安装方式**。在 pip 之前，装 Python 库要手动下载、解压、配置路径，每个库的安装方法还不一样。

pip 的思路：所有包都从 PyPI（Python Package Index）下载，一条命令搞定。开发者把包传到 PyPI，用户用 pip 下载，标准统一。

类比：以前买东西要跑各个商店，每家支付方式还不同。pip 像建了一个统一超市——所有商品都在这，统一的购物流程，一键下单。

### 依赖自动解析

pip 会自动解析依赖：你装 A，A 依赖 B，pip 会自动把 B 也装上。不用手动一个个装。

但 pip 的依赖解析比较"简单粗暴"——它不检查已安装的包是否兼容新装的包，可能导致版本冲突。这也是为什么需要虚拟环境来隔离。

### 关键取舍

- **简单 vs 智能**：pip 安装简单直接，但依赖解析不如 conda 智能。pip 选择简单，把隔离的事交给 venv
- **包数量 vs 质量**：PyPI 有 40 万+ 包，数量碾压 conda，但质量参差不齐
- **官方标准 vs 第三方**：pip 是 Python 官方推荐工具，标准库自带，不用额外安装

---

## 安装库

```bash
pip install requests            # 安装 requests 库
pip install requests==2.28.0    # 安装指定版本
pip install "requests>=2.28"    # 安装满足条件的版本
pip install -r requirements.txt # 根据文件批量安装
```

---

## 查看与卸载

```bash
pip list                        # 查看已安装的库
pip show requests               # 查看某个库的详细信息
pip freeze                      # 列出已安装库（适合导出 requirements.txt）
pip install --upgrade requests  # 升级库
pip uninstall requests          # 卸载库
```

---

## 虚拟环境

每个项目独立的依赖环境，避免版本冲突。

```bash
python -m venv .venv            # 创建虚拟环境（名为 .venv）

# 激活虚拟环境
.venv\Scripts\activate          # Windows
source .venv/bin/activate       # macOS/Linux

# 退出虚拟环境
deactivate
```

---

## requirements.txt

项目的"购物清单"，记录所有依赖。

```bash
pip freeze > requirements.txt   # 导出当前环境所有依赖
pip install -r requirements.txt # 根据清单安装
```

---

## 换源加速

国内下载慢，可以换国内镜像源。

```bash
pip install requests -i https://pypi.tuna.tsinghua.edu.cn/simple
```

永久换源：

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

常用镜像源：
- 清华：`https://pypi.tuna.tsinghua.edu.cn/simple`
- 阿里云：`https://mirrors.aliyun.com/pypi/simple`
- 中科大：`https://pypi.mirrors.ustc.edu.cn/simple`

---

## pip vs conda

| 维度 | pip | conda |
| :--- | :--- | :--- |
| **管理范围** | 只管理 Python 包 | 管理 Python 包 + 系统库 |
| **虚拟环境** | 需要 venv | 自带环境管理 |
| **非 Python 依赖** | 不支持 | 支持（如 CUDA、C 库） |
| **适合场景** | 纯 Python 项目 | 数据科学、机器学习 |

---

## 面试速答

### Q: pip install 时 --user 和虚拟环境有什么区别？
A: --user 装到当前用户的目录下，只对当前用户可见，但仍可能和其他包冲突。虚拟环境是完全隔离的 Python 环境，每个项目独立，互不干扰。推荐用虚拟环境而不是 --user。

### Q: requirements.txt 是什么？
A: 项目的依赖清单，记录所有需要安装的包和版本。用 pip freeze > requirements.txt 导出，pip install -r requirements.txt 安装。保证不同机器上依赖一致。

### Q: pip 下载慢怎么办？
A: 默认从国外 PyPI 下载，换成国内镜像源（清华、阿里云）能快 10 倍。临时用 -i 参数指定源，永久用 pip config set global.index-url 设置。常用清华源：https://pypi.tuna.tsinghua.edu.cn/simple

### Q: 为什么每个项目要用虚拟环境？
A: 不同项目可能依赖同一个包的不同版本，全局环境只能装一个版本，会冲突。虚拟环境给每个项目独立的 Python 环境，互不干扰。用 venv 创建，激活后安装的包只在这个环境里。

---

## 如何给别人讲

1. **一句话定位**：pip 是 Python 的包管理器，像 Python 的应用商店
2. **核心价值**：一条命令安装第三方库，自动解析依赖，不用手动下载配置
3. **必备搭配**：虚拟环境（venv）——每个项目独立环境，避免版本冲突
4. **常见痛点**：下载慢（换国内源）、依赖冲突（用虚拟环境）
5. **面试一句话**：pip 是 Python 官方包管理工具，从 PyPI 下载包，配合 venv 做环境隔离
