# Python

> Python 以简洁易读著称，像"用英语写代码"。广泛用于数据科学、AI、自动化脚本和 Web 后端。本文介绍其核心特性、设计哲学与常用语法。

**关键词**：动态类型、简洁语法、标准库、数据科学、AI、GIL

## 抽象类比

Python 像自动挡汽车——操作简单、上手快，虽然速度不是最快，但日常使用最舒服。

如果说 C 是手动挡赛车，Java 是重型卡车，Python 就是自动挡家用轿车——大多数人日常开的是家用轿车。

---

## 什么是 Python

Python 诞生于 1991 年，设计者是 Guido van Rossum。设计目标：**让代码像英语一样好读**。

Python 的核心信条：
- **简洁**：能一行搞定的不写两行
- **可读**：代码像伪代码，读起来像英语
- **电池已装好**：标准库极其丰富，开箱即用

---

## 核心概念

### 动态类型：变量没有固定类型

- **静态类型**（Java）：变量像贴了标签的盒子，"这个盒子只能装书"
- **动态类型**（Python）：变量像没贴标签的盒子，"今天装书，明天装衣服，都行"

好处：写代码快，灵活。坏处：运行时才发现类型错误，大项目容易翻车。

### 缩进即语法：没有大括号

Python 用缩进表示代码块，不用 `{}`。强制你写出整齐的代码——"格式不对，直接报错"。

好处是：代码天然整齐，团队协作风格统一。坏处是：缩进错了就报错，不够宽容。

### 电池已装好：标准库极其丰富

Python 自带几百个库，像一把瑞士军刀——文件操作、网络请求、数据处理、加密、压缩……几乎不用装第三方库就能干很多事。

这就是 Python 的哲学："batteries included"（电池已装好）。

### 为什么 Python 这么火

Python 火了三波：
- **2000s**：运维和自动化脚本（替代 shell 脚本）
- **2010s**：数据科学（NumPy + Pandas + Matplotlib）
- **2020s**：AI 和机器学习（PyTorch + TensorFlow）

Python 能成为 AI 时代的"官方语言"，不是因为它性能强（其实性能很差），而是因为**生态太强**——所有最新的 AI 论文，第一个发布的都是 Python 代码。

### Python 的代价

Python 的简洁是有代价的：
- **速度慢**：比 C 慢 10-100 倍（GIL 限制 + 解释执行）
- **不适合移动端**：几乎没有 Python 手机 App
- **不适合高性能计算**：底层都是 C/C++ 写的，Python 只是"调用层"

所以真正的 AI 系统：Python 写"调度"，C/C++ 写"计算"。Python 是指挥家，不是演奏者。

### 常见误解

- **"Python 很慢，所以不好"**：慢的是 Python 本身，但它调用的库（NumPy、PyTorch）底层都是 C，速度很快
- **"Python 适合做所有事"**：不适合移动端、嵌入式、高性能后端
- **"学了 Python 就不用学其他语言了"**：Python 是"胶水语言"，理解底层还得学 C

---

## 设计哲学

Python 的设计动机是：**编程太难了，应该像写英语一样自然**。Guido van Rossum 在 1989 年的圣诞假期开始写 Python，初衷是给非计算机专业的人一门"好读好用"的语言。

### 解决的核心问题

- **代码可读性差**：用缩进强制格式统一，代码像伪代码一样好读
- **重复造轮子**：标准库极其丰富（"电池已装好"），开箱即用
- **入门门槛高**：动态类型 + 简洁语法，新手半小时就能写出有用程序

### 关键取舍

Python 的哲学是"**可读性比性能更重要**"：
- 用动态类型换灵活性 → 代价是运行慢、大项目易出类型 bug
- 用解释执行换便捷 → 代价是速度比 C 慢 10-100 倍
- 用 GIL（全局解释器锁）换内存安全 → 代价是真并行困难

Python 不追求"跑得快"，而追求"写得快、改得快、看得懂"。真正的性能瓶颈交给 C 扩展（NumPy/PyTorch 底层都是 C）。

### 类比

Python 像一把"手感极好的厨师刀"——不是切得最快的，但握着最舒服，让大多数人都能愉快地做饭。它信奉"简单胜于复杂"，宁可慢一点，也要让代码读起来像在说话。

---

## 安装与运行

```bash
python --version                # 查看版本
python hello.py                 # 运行脚本
python                          # 进入交互式环境（exit() 退出）
pip install requests            # 安装第三方库
```

---

## Hello World

```python
print("Hello, World!")
```

---

## 变量与类型

```python
name = "张三"                   # 字符串
age = 25                        # 整数
price = 9.99                    # 浮点数
is_adult = True                 # 布尔值
```

Python 是动态类型，变量不需要声明类型。

---

## 字符串

```python
s = "Hello, Python!"
print(s.upper())                     # 转大写
print(s.replace("Python", "World"))  # 替换
print(s.split(", "))                 # 分割 → ["Hello", "Python!"]
print(f"我叫{name}，今年{age}岁")      # 格式化字符串
```

---

## 列表与字典

```python
# 列表（有序、可变）
nums = [1, 2, 3, 4, 5]
nums.append(6)                  # 追加
nums[0]                         # 1
nums[1:3]                       # [2, 3]（切片）
len(nums)                       # 6

# 字典（键值对）
user = {"name": "张三", "age": 25}
user["email"] = "zhang@test.com"   # 添加
user["name"]                       # "张三"
user.get("phone", "未知")           # 不存在返回默认值
```

---

## 控制流

```python
# if-elif-else
if age >= 18:
    print("成年")
elif age >= 12:
    print("青少年")
else:
    print("儿童")

# for 循环
for i in range(5):              # 0, 1, 2, 3, 4
    print(i)

for name in ["张三", "李四"]:
    print(name)

# while 循环
n = 5
while n > 0:
    print(n)
    n -= 1

# 列表推导式（一行生成列表）
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, ...]
```

---

## 函数

```python
def add(a, b):
    return a + b

def greet(name, greeting="你好"):  # 默认参数
    print(f"{greeting}，{name}！")

# 调用
result = add(3, 5)
greet("张三")
greet("张三", greeting="早上好")   # 关键字参数
```

---

## 类

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        print(f"你好，我叫{self.name}")

s = Student("张三", 20)
s.greet()
```

---

## 文件操作

```python
# 写入
with open("test.txt", "w") as f:
    f.write("Hello, File!\n")

# 读取
with open("test.txt", "r") as f:
    content = f.read()
    print(content)

# 逐行读取
with open("test.txt", "r") as f:
    for line in f:
        print(line.strip())
```

`with` 语句会自动关闭文件，不需要手动 `close()`。

---

## 异常处理

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"错误: {e}")
except Exception as e:
    print(f"其他错误: {e}")
finally:
    print("无论如何都会执行")
```

---

## 包管理与虚拟环境

Python 的包管理生态是工程化的核心，主要围绕 pip 与 venv 展开。

### pip：Python 包管理器

pip 是 Python 官方包管理工具，从 Python 2.7.9 / 3.4 起默认安装，从 PyPI（Python Package Index）下载安装包。

常用命令：

```bash
pip install requests              # 安装包
pip install requests==2.28.0      # 安装指定版本
pip install -r requirements.txt   # 按依赖文件批量安装
pip uninstall requests            # 卸载包
pip list                          # 列出已安装的包
pip show requests                 # 查看包的详细信息
pip freeze > requirements.txt     # 导出当前环境的依赖清单
pip install --upgrade requests    # 升级包
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple requests  # 用国内镜像加速
```

国内常用镜像源：

- 清华：https://pypi.tuna.tsinghua.edu.cn/simple
- 阿里：https://mirrors.aliyun.com/pypi/simple
- 腾讯：https://mirrors.cloud.tencent.com/pypi/simple

永久切换镜像源：`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`

### venv：虚拟环境

虚拟环境隔离不同项目的依赖，避免"项目 A 要 requests 2.20、项目 B 要 requests 2.28"的冲突。

```bash
python -m venv .venv              # 在当前目录创建虚拟环境
.venv\Scripts\activate            # Windows 激活
source .venv/bin/activate         # Linux/macOS 激活
deactivate                        # 退出虚拟环境
```

激活后 `pip install` 安装的包只对当前虚拟环境生效，不影响系统全局。

### 其他工具

- pip-tools：用 `pip-compile` 把 `requirements.in` 编译成精确锁版本的 `requirements.txt`
- poetry：现代化的依赖管理与打包工具，类似 Node 的 npm
- conda：科学计算场景常用，能装非 Python 依赖（如 CUDA、MKL），详见 [[conda]]
- uv：Rust 写的极速 pip 替代品，2024 年起流行

### 依赖文件规范

- requirements.txt：最基础的依赖清单，`pip install -r` 安装
- requirements-dev.txt：开发环境依赖（测试、lint、文档）
- pyproject.toml：现代 Python 项目标准（PEP 518/621），配合 poetry/hatch/pdm 使用
- Pipfile：pipenv 专用，已较少使用
- setup.py：传统打包脚本，正逐步被 pyproject.toml 替代

---

## 定位速查

| 维度 | Python |
| :--- | :--- |
| **类型检查** | 动态类型 |
| **运行方式** | 解释型 |
| **核心特色** | 语法简洁、库极其丰富、缩进即语法 |
| **适合场景** | 数据科学、AI、自动化脚本、Web 后端 |
| **代表框架** | Django、Flask、FastAPI |

---

## 面试速答

### Q: Python 为什么慢？怎么优化？
A: Python 慢主要是因为解释执行和 GIL（全局解释器锁，导致多线程无法真并行）。优化方法：用 C 扩展（NumPy/PyTorch 底层都是 C）、用多进程绕开 GIL、用 PyPy 解释器、或把性能瓶颈部分用 Cython 重写。

### Q: Python 的 GIL 是什么？有什么影响？
A: GIL 是全局解释器锁，同一时刻只允许一个线程执行 Python 字节码。它让内存管理简化（引用计数不需要加锁），但导致多线程无法利用多核。CPU 密集型任务要用多进程（multiprocessing）绕开，I/O 密集型任务多线程仍有效（等 I/O 时会释放 GIL）。

### Q: Python 是解释型还是编译型？
A: Python 先把源码编译成字节码（.pyc），再由解释器执行字节码。所以它是"先编译成字节码再解释执行"，严格说两者都有。.pyc 文件能缓存，下次运行更快。

### Q: 列表和元组有什么区别？
A: 列表可变（mutable），元组不可变（immutable）。元组因为不可变，可以作为字典的键、可以做哈希，性能也略快。列表适合需要增删改的场景，元组适合固定不变的数据（如坐标、配置）。

### Q: Python 为什么能成为 AI 首选语言？
A: 不是因为性能（其实很慢），而是生态：NumPy/Pandas 做数据处理，PyTorch/TensorFlow 做建模，所有最新 AI 论文都先发 Python 代码。Python 是"指挥层"，底层计算都是 C/C++，形成了无法替代的生态壁垒。