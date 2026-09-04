# Ruby

> Ruby 是一门"让程序员快乐"的语言，语法优雅、表达力强。Ruby on Rails 框架让 Web 开发变得高效。本文介绍其核心特性、设计哲学与常用语法。

**关键词**：一切皆对象、块(Block)、Rails、约定优于配置、动态类型、表达力

## 抽象类比

Ruby 像一首诗——语法像自然语言一样流畅，写代码像写文章。"一切皆对象"是它的核心信条。

如果说 Java 像填表格（严谨但枯燥），Python 像写英语作文（简洁好读），Ruby 就像写诗——优雅、有韵律、让人愉悦。

---

## 什么是 Ruby

Ruby 诞生于 1995 年，设计者是日本的松本行弘（Matz）。设计目标：**让程序员快乐**。

Matz 说："Ruby 的目标是让程序员开心，而不是让机器高效。"这听起来很任性，但正是这种理念让 Ruby 的语法极其优雅。

Ruby 的核心信条：
- **一切皆对象**：数字、字符串、甚至 `true`/`false` 都是对象
- **表达力强**：很多操作只需要一行
- **开发者友好**：语法像自然语言，读起来像英语

---

## 核心概念

### 一切皆对象

在 Ruby 里，数字、字符串、甚至 `true`/`false` 都是对象。`1 + 2` 其实是"调用数字 1 的 `+` 方法，传入参数 2"。

像现实世界——所有东西都有行为。数字 1 不只是个数字，它能回答"你是偶数吗？"（`1.even?` → false）。

### 块（Block）：代码也是数据

Ruby 可以把一段代码当参数传给方法。像把"一段待办的事"交给别人，让别人决定什么时候执行。

这是 Ruby 最强大的特性之一——你可以把逻辑"注入"到方法里，让方法变得更灵活。

### 开发者友好

Ruby 的设计目标是"让程序员开心"。很多在其他语言里需要好几行的代码，在 Ruby 里一行就搞定。

比如：`5.times { puts "你好" }` —— 数字 5 直接调用 `times` 方法，打印 5 次"你好"。

### 为什么 Ruby on Rails 改变了 Web 开发

2004 年，一个叫 DHH 的丹麦人用 Ruby 写了一个框架叫 Rails。它提出了一个革命性的理念：**"约定优于配置"**。

之前的 Java Web 开发：写 XML 配置文件、配数据库连接、配路由……还没开始写业务逻辑，配置已经写了一天。

Rails 说：**你按我的规矩来，我帮你搞定一切**。数据库表叫 `users`，模型自动叫 `User`；路由自动映射到控制器。开箱即用。

GitHub 最初就是用 Rails 写的。Twitter 最初也是。

### Ruby 的现状

Ruby 的热度在下降，但不是因为语言不好，而是因为：
- **Python** 抢了数据科学和 AI 的生态位
- **Node.js** 抢了"动态语言写后端"的生态位
- **Go** 抢了"新语言写后端"的生态位

但 Ruby 依然是独立开发者和创业公司的最爱——开发速度极快，一个人能顶一个团队。

### 常见误解

- **"Ruby 已经死了"**：Ruby on Rails 依然在维护，很多 SaaS 产品还在用
- **"Ruby 只能写 Web"**：Ruby 是通用语言，脚本、CLI 工具都能写
- **"Ruby 性能很差"**：MRI（官方解释器）确实慢，但有 JRuby（JVM 上跑）和 TruffleRuby（GraalVM 上跑）

---

## 设计哲学

Ruby 诞生于 1995 年（松本行弘/Matz），设计目标很任性：**让程序员快乐**。Matz 说"Ruby 的目标是让程序员开心，而不是让机器高效"——这在追求性能的语言界是个异类，却造就了 Ruby 极致的优雅。

### 解决的核心问题

- **代码不优雅**：语法像自然语言，读起来像英语句子
- **重复样板代码**：一切皆对象 + 块（Block），表达力极强，一行顶别的语言五行
- **开发效率**：Ruby on Rails 的"约定优于配置"，让一个人能顶一个团队

### 关键取舍

Ruby 的哲学是"**程序员体验高于机器性能**"：
- 动态类型 + 解释执行 → 灵活优雅，但运行慢
- 一切皆对象 → 统一优雅，但有性能开销
- 语法糖丰富 → 好读好写，但同一件事有多种写法（不够统一）

Ruby 不追求"跑得最快"，而追求"写得最爽"。它适合快速原型和创业初期——开发速度就是一切。

### 类比

Ruby 像一支"书写极其流畅的钢笔"——它不是为了写得最快（打字机更快），而是为了写得最舒服、最有韵律。用 Ruby 写代码像写诗，每一行都让人愉悦。当你享受书写的过程，灵感自然就来了。

---

## 安装与运行

```bash
ruby --version                  # 查看版本
ruby hello.rb                   # 运行脚本
irb                             # 进入交互式环境（exit 退出）
gem install rails               # 安装 Rails
```

---

## Hello World

```ruby
puts "Hello, World!"
```

---

## 变量与类型

```ruby
name = "张三"                   # 字符串
age = 25                        # 整数（Fixnum 也是对象）
price = 9.99                    # 浮点数
is_adult = true                 # 布尔值
```

Ruby 是动态类型，变量不需要声明类型。

---

## 一切皆对象

```ruby
1.class                         # Integer
"hello".upcase                  # "HELLO"
[1, 2, 3].length                # 3
3.times { puts "你好" }          # 打印 3 次"你好"
5.even?                         # false
```

数字、字符串、数组都是对象，都能调用方法。

---

## 块（Block）

```ruby
# each 遍历
[1, 2, 3].each do |n|
    puts n
end

# 简写形式
[1, 2, 3].each { |n| puts n }

# map 转换
squares = [1, 2, 3].map { |n| n ** 2 }  # [1, 4, 9]

# select 过滤
evens = (1..10).select { |n| n.even? }  # [2, 4, 6, 8, 10]
```

---

## 控制流

```ruby
# if-elsif-else
if age >= 18
    puts "成年"
elsif age >= 12
    puts "青少年"
else
    puts "儿童"
end

# 后置 if（一行条件）
puts "成年" if age >= 18

# unless（= if not）
puts "未成年" unless age >= 18

# case-when
case grade
when "A" then puts "优秀"
when "B" then puts "良好"
else puts "其他"
end
```

---

## 函数（方法）

```ruby
def add(a, b)
    a + b                       # 最后一行自动作为返回值
end

def greet(name, greeting = "你好")
    puts "#{greeting}，#{name}！"
end

result = add(3, 5)
greet("张三")
```

---

## 类

```ruby
class Student
    attr_accessor :name, :age   # 自动生成 getter/setter

    def initialize(name, age)
        @name = name
        @age = age
    end

    def greet
        puts "你好，我叫#{@name}"
    end
end

s = Student.new("张三", 20)
s.greet
s.name                          # "张三"
```

---

## 常用数据结构

```ruby
# 数组
arr = [1, 2, 3]
arr << 4                        # 追加 → [1, 2, 3, 4]
arr.map { |n| n * 2 }           # [2, 4, 6, 8]

# 哈希（字典）
user = { name: "张三", age: 25 }
user[:email] = "zhang@test.com"
user[:name]                     # "张三"

# 范围
(1..5).to_a                     # [1, 2, 3, 4, 5]
('a'..'e').to_a                 # ["a", "b", "c", "d", "e"]
```

---

## 定位速查

| 维度 | Ruby |
| :--- | :--- |
| **类型检查** | 动态类型 |
| **运行方式** | 解释型 |
| **核心特色** | 一切皆对象、块（Block）、语法优雅 |
| **适合场景** | Web 开发、脚本、原型开发 |
| **代表框架** | Ruby on Rails、Sinatra |

---

## 面试速答

### Q: Ruby 的"一切皆对象"是什么意思？
A: 在 Ruby 里，数字、字符串、布尔值甚至类本身都是对象，都能调用方法。比如 `5.even?`、`"hello".upcase`。没有"基本类型"和"对象类型"的区别（不像 Java 有 int 和 Integer）。这让语言非常统一和灵活。

### Q: Ruby 的块（Block）是什么？有什么用？
A: 块是一段可以传给方法的代码，类似其他语言的 lambda/闭包。方法用 `yield` 调用块。常见用法：`[1,2,3].each { |n| puts n }`、`arr.map { |n| n * 2 }`。块让 Ruby 的迭代器极其优雅，是它最强大的特性之一。

### Q: Ruby on Rails 的"约定优于配置"是什么？
A: 框架预设了一套约定（表叫 users → 模型叫 User，路由自动映射），你按约定来就不用写配置文件。这和早期 Java Web（写一堆 XML 配置）形成鲜明对比。好处是开发极快，代价是要按 Rails 的规矩来，灵活性降低。

### Q: Ruby 和 Python 有什么区别？
A: 设计哲学不同：Ruby 追求"优雅和表达力"（多种写法，挑最优雅的），Python 追求"只有一种 obvious 的写法"。Ruby 更面向对象（一切皆对象），Python 更实用主义。Ruby 强在 Web（Rails），Python 强在数据科学和 AI。

### Q: Ruby 性能差怎么办？
A: 官方解释器 MRI 确实慢，但有替代方案：JRuby（跑在 JVM 上，可利用 Java 生态和 JIT）、TruffleRuby（跑在 GraalVM 上，性能接近 C）。另外性能瓶颈通常在数据库和 I/O，Ruby 本身往往不是瓶颈。Rails 开发快，适合创业初期。