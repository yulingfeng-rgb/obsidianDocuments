# pip 入门

> 小白先读：pip 是 Python 的包管理器，用来安装和管理 Python 库。你想用 pandas 分析数据？`pip install pandas` 就搞定了。
>
> 学这篇时先抓 3 个问题：pip 是什么？虚拟环境是什么？requirements.txt 是什么？

## 先用一句生活话理解

pip 像 Python 世界的"应用商店"——你想用什么功能，搜一下、装一下就有了。

就像手机装 App 一样，Python 装库用 pip。

## 什么是 pip？

想象你要做饭：
- **Python**：厨房
- **pip**：厨具商店，要什么工具来这里买
- **库**：各种厨具（锅、刀、搅拌机）

**简单说**：pip 是 **Python 的包管理工具**，用来安装第三方库。

---

## pip 的核心概念

### 1. 安装库

```bash
pip install requests      # 安装 requests 库
pip install pandas numpy  # 安装多个
pip install requests==2.28.0  # 指定版本
pip uninstall requests    # 卸载
```

---

### 2. 虚拟环境：独立厨房

**类比**：虚拟环境像独立的厨房，每个项目的调料互不影响。

项目 A 用 pandas 1.0，项目 B 用 pandas 2.0，不会冲突。

```bash
python -m venv myenv      # 创建虚拟环境
source myenv/bin/activate  # 激活（Linux/Mac）
myenv\Scripts\activate     # 激活（Windows）
deactivate                 # 退出
```

---

### 3. requirements.txt：购物清单

**类比**：requirements.txt 像购物清单，记录项目需要哪些库。

```txt
requests==2.28.0
pandas>=1.5.0
numpy
```

```bash
pip freeze > requirements.txt  # 导出清单
pip install -r requirements.txt  # 按清单安装
```

---

## pip 的应用场景

| 场景 | 用什么 | 为什么 |
| :--- | :--- | :--- |
| **数据分析** | pandas、numpy | 数据处理 |
| **Web 开发** | Django、Flask | Web 框架 |
| **爬虫** | requests、scrapy | 网络请求 |
| **机器学习** | tensorflow、pytorch | AI 框架 |

---

## 总结

### 三句话讲清楚 pip

1. pip 是 Python 的包管理工具，用来安装第三方库
2. 虚拟环境隔离项目依赖，避免版本冲突
3. requirements.txt 记录项目依赖，方便复现环境

### 核心类比

> pip 像 Python 世界的"应用商店"——想用什么功能，装一下就有了。

### 关键词

**pip、Python、包管理、虚拟环境、requirements.txt**
