# Rust

> Rust 是一门追求"零成本抽象"和"内存安全"的语言。没有垃圾回收，但编译时就能保证内存安全。连续多年 Stack Overflow "最受喜爱语言"。本文介绍其核心特性、设计哲学与常用语法。

**关键词**：所有权、内存安全、零成本抽象、无畏并发、编译型、Borrow Checker

## 抽象类比

Rust 像一辆带全套安全系统的赛车——速度极快（接近 C/C++），但不会翻车（编译时消除内存错误）。学习曲线陡，但学会后写出的代码极其可靠。

如果说 C 是一把锋利的手术刀（威力大但容易割伤自己），Java 是带安全护栏的工厂（安全但慢），Rust 就是一把"智能手术刀"——同样锋利，但刀上有传感器，危险操作会自动锁住。

---

## 什么是 Rust

Rust 诞生于 2010 年，设计者是 Mozilla 的 Graydon Hoare。设计目标：**解决 C/C++ 的痛点——内存不安全、并发难写、没有现代工具链**。

Rust 的核心信条：
- **内存安全**：编译时消灭空指针、数据竞争、内存泄漏
- **零成本抽象**：高级特性（迭代器、闭包、泛型）不牺牲性能
- **无畏并发**：类型系统保证并发安全

Rust 连续多年 Stack Overflow "最受喜爱语言"，但学习曲线也是出了名的陡。

---

## 核心概念

### 所有权（Ownership）：每个值只有一个主人

Rust 最独特的设计。每个值在任意时刻只有一个变量拥有它。变量离开作用域，值自动释放。不需要手动 `free`，也不需要垃圾回收。

像图书馆借书——一本书同时只能被一个人借走。你要看？要么等别人还了（所有权转移），要么你复印一份（clone）。

### 借用（Borrowing）：借来看看，不拿走

不能把值给别人（所有权转移），但可以"借"给别人看看（引用）。

借用分两种：
- **只读借用**（`&T`）：可以多人同时借——"大家都来看看"
- **可变借用**（`&mut T`）：只能一个人借，且不能同时有只读借用——"我在改东西，别来打扰"

这个规则在编译时强制执行，所以数据竞争在运行前就被消灭了。

### 零成本抽象：高级特性不牺牲性能

迭代器、闭包、泛型……这些高级特性编译后和手写 C 一样快。

"你不用为没用到的东西付费"——这是 C++ 的原则，Rust 也继承了。用高级特性不会让程序变慢，编译器会优化到和手写底层代码一样快。

### 为什么 Rust 被称为"未来之星"

Rust 解决了一个困扰业界 40 年的难题：**怎么同时拥有 C 的速度和 Java 的安全？**

在 Rust 之前，你只能二选一：
- 要速度？用 C/C++，但内存泄漏、空指针、数据竞争随时教你做人
- 要安全？用 Java/Python，但有垃圾回收的开销

Rust 说：**我全都要**。通过所有权系统在编译时消灭内存错误，不需要垃圾回收，速度接近 C。

这就是为什么 Linux 内核从 2023 年开始接受 Rust 代码——这是 C 统治了 30 年的地盘。

### Rust 的学习曲线

Rust 的学习曲线是出了名的陡。原因：
- **所有权系统**：其他语言没有的概念，编译器会反复"教育"你
- **生命周期**：引用的有效期规则，初学者最容易卡住的地方
- **编译器报错**：Rust 编译器以"严格但友好"著称——报错信息很长，但会告诉你怎么改

好消息是：**一旦代码编译通过，几乎不会有运行时 bug**。"与编译器搏斗"的过程，就是在消灭 bug 的过程。

### 谁在用 Rust

- **Mozilla**：Firefox 的渲染引擎用 Rust 重写
- **微软**：Windows 内核开始用 Rust 重写关键模块
- **AWS**：Firecracker（Lambda 的底层引擎）用 Rust 写的
- **Discord**：把核心服务从 Go 迁移到 Rust（因为性能）
- **Linux 内核**：2023 年开始支持 Rust 驱动

### 常见误解

- **"Rust 会取代 C"**：不会。C 的简单性和生态无可替代，Rust 更适合"新项目"
- **"Rust 编译太慢"**：确实比 Go 慢很多，但编译一次，运行无数次
- **"Rust 只能写底层系统"**：其实 Web（Wasm）、CLI 工具、Web 后端都能写

---

## 设计哲学

Rust 诞生于 2010 年，解决的是一个困扰业界 40 年的难题：**怎么同时拥有 C 的速度和 Java 的安全？** 在 Rust 之前，速度和安全只能二选一。

### 解决的核心问题

- **内存安全**：编译时消灭空指针、悬垂指针、数据竞争——不需要垃圾回收
- **无畏并发**：类型系统保证"数据竞争根本编译不过"
- **零成本抽象**：高级特性（迭代器、闭包、泛型）编译后和手写 C 一样快

### 关键取舍

Rust 的哲学是"**把运行时错误提前到编译时**"：
- 用所有权系统换内存安全 → 代价是学习曲线陡峭（"和编译器搏斗"）
- 用编译时检查换运行时性能 → 代价是编译慢
- 没有继承、没有异常 → 用 trait 和 Result 替代

Rust 把其他语言在运行时才发现的 bug，全部挪到编译时。**一旦编译通过，几乎不会有运行时崩溃**。

### 类比

Rust 像一辆"装了赛车级主动安全系统的 F1"——速度和 C 一样快，但每个危险操作都会被系统提前拦下。你要花时间学会和安全系统配合（这就是陡峭的学习曲线），但学会后，翻车几乎不可能。

---

## 安装与运行

```bash
rustc --version                 # 查看版本
rustc hello.rs                  # 编译
./hello                         # 运行

cargo new myproject             # 创建新项目
cargo build                     # 编译
cargo run                       # 编译并运行
cargo test                      # 运行测试
```

---

## Hello World

```rust
fn main() {
    println!("Hello, World!");
}
```

---

## 变量与类型

```rust
let x = 5;                      // 不可变（默认）
let mut y = 10;                 // 可变
let name: &str = "张三";        // 字符串切片
let age: i32 = 25;              // 32 位整数
let price: f64 = 9.99;          // 64 位浮点
let is_adult: bool = true;      // 布尔值

const MAX: i32 = 100;           // 常量
```

---

## 所有权与借用

```rust
// 所有权
let s1 = String::from("hello");
let s2 = s1;                    // s1 的所有权转移给 s2
// println!("{}", s1);          // 报错！s1 已经无效

// 克隆（深拷贝）
let s3 = s2.clone();
println!("{}", s2);             // 还可以用

// 借用（引用）
fn print_len(s: &String) {      // 借用，不拿走所有权
    println!("长度: {}", s.len());
}
print_len(&s2);                 // 传入引用
println!("{}", s2);             // s2 还能继续用

// 可变借用
fn add_exclaim(s: &mut String) {
    s.push_str("!");
}
let mut s4 = String::from("hello");
add_exclaim(&mut s4);
```

---

## 字符串

Rust 有两种字符串：`&str`（字符串切片，不可变引用）和 `String`（可拥有、可修改）。

```rust
let s = String::from("hello");
let slice: &str = &s[0..5];     // 字符串切片

let mut s = String::from("hello");
s.push_str(" world");           // 追加
s.push('!');                    // 追加字符
let len = s.len();              // 长度
```

---

## 控制流

```rust
// if-else
if age >= 18 {
    println!("成年");
} else {
    println!("未成年");
}

// for 循环
for i in 0..5 {                 // 0 到 4
    println!("{}", i);
}

for i in 0..=5 {                // 0 到 5（包含）
    println!("{}", i);
}

let names = vec!["张三", "李四", "王五"];
for name in &names {
    println!("{}", name);
}

// while
let mut n = 5;
while n > 0 {
    println!("{}", n);
    n -= 1;
}

// loop（无限循环，用 break 退出）
loop {
    // ...
    break;
}
```

---

## 函数

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b                       // 最后一个表达式作为返回值（不用分号）
}

fn greet(name: &str) {
    println!("你好，{}！", name);
}

let result = add(3, 5);
greet("张三");
```

---

## 结构体与枚举

```rust
struct Student {
    name: String,
    age: i32,
}

impl Student {
    fn new(name: &str, age: i32) -> Self {
        Student {
            name: String::from(name),
            age,
        }
    }

    fn greet(&self) {
        println!("你好，我叫{}", self.name);
    }
}

let s = Student::new("张三", 20);
s.greet();

// 枚举
enum Direction {
    North,
    South,
    East,
    West,
}

let dir = Direction::North;
```

---

## 错误处理

Rust 没有异常，用 `Result` 和 `Option` 处理错误。

```rust
// Result<T, E>
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        return Err(String::from("除数不能为 0"));
    }
    Ok(a / b)
}

match divide(10.0, 3.0) {
    Ok(result) => println!("结果: {}", result),
    Err(e) => println!("错误: {}", e),
}

// ? 运算符（简化错误传播）
fn do_math() -> Result<f64, String> {
    let result = divide(10.0, 3.0)?;  // 如果是 Err，直接返回
    Ok(result + 1.0)
}
```

---

## 定位速查

| 维度 | Rust |
| :--- | :--- |
| **类型检查** | 静态类型 |
| **运行方式** | 编译型 |
| **核心特色** | 所有权系统、零成本抽象、无 GC 但内存安全 |
| **适合场景** | 系统编程、WebAssembly、CLI 工具、嵌入式 |
| **代表框架** | Actix、Axum |

---

## 面试速答

### Q: Rust 的所有权（Ownership）是什么？
A: 每个值在任意时刻只有一个"主人"（变量）。主人离开作用域时值自动释放，不需要手动 free 也不需要垃圾回收。值赋值给别的变量时所有权转移，原变量失效。这样编译器就能静态追踪每个值的生命周期，消灭内存错误。

### Q: Rust 怎么实现"无 GC 但内存安全"？
A: 靠所有权 + 借用 + 生命周期三件套在编译时检查。所有权保证值有唯一释放点；借用规则（要么一个可变引用，要么多个不可变引用）消灭数据竞争；生命周期保证引用不会比被引用者活得更久。这些检查全在编译时完成，运行时零开销。

### Q: Rust 为什么学习曲线陡？
A: 因为所有权和生命周期是其他语言没有的概念。初学者会反复被编译器拒绝（"borrow checker"），要把习惯的写法改成符合所有权规则的形式。但好处是：一旦编译通过，几乎不会有内存相关的运行时 bug。

### Q: Rust 和 C++ 相比有什么优势？
A: Rust 在编译时保证内存安全和并发安全，C++ 靠程序员自觉。Rust 没有空指针、悬垂指针、数据竞争；C++ 这些都可能在运行时崩溃。Rust 的工具链（cargo）也更现代。代价是 Rust 学习曲线更陡。

### Q: Rust 适合做什么？
A: 系统编程（Linux 内核已支持 Rust）、WebAssembly、CLI 工具、嵌入式、高性能后端。适合对性能和安全都有要求的场景。不适合快速原型开发（编译慢、学习成本高）。
