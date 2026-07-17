# Swift

> Swift 是 Apple 为 iOS/macOS 开发打造的编程语言，语法现代、安全、快速。取代了老旧的 Objective-C。本文介绍其核心特性、设计哲学与常用语法。

**关键词**：可选类型、协议导向、值类型、Apple 生态、类型安全、Objective-C 互操作

## 抽象类比

Swift 像 Objective-C 的"现代化精装版"——去掉了晦涩的语法，加了类型安全、可选类型、模式匹配等现代特性。

如果说 Objective-C 是一辆功能强大但操作复杂的老式跑车，Swift 就是同款车的"智能驾驶版"——速度一样快，但加了自动刹车、车道保持。

---

## 什么是 Swift

Swift 诞生于 2014 年，由 Apple 的 Chris Lattner 设计。目标是**取代 Objective-C，成为 Apple 生态的首选语言**。

Swift 的设计目标：
- **安全**：编译时消灭尽可能多的错误
- **快速**：运行速度接近 C/C++
- **现代**：语法简洁，支持闭包、泛型、可选类型等现代特性

Swift 不只是"Apple 专用"——现在也能用于服务端开发（Vapor）、命令行工具等。

---

## 核心概念

### 可选类型（Optional）：显式处理"没有值"

Swift 不允许"偷偷"使用空值。变量要么是确定类型，要么是"可选类型"（可能有值，可能是 nil）。

像快递柜——有些柜子"保证有包裹"（`String`），有些柜子"可能有也可能没有"（`String?`）。打开后者之前，你必须先检查，编译器会盯着你。

```swift
var email: String? = nil
// print(email!)          // 强制解包：如果为 nil 就崩溃
print(email ?? "未设置")   // 安全：为 nil 时用默认值
```

### 值类型优先

Swift 的 `struct` 是值类型（赋值时复制），不是引用类型。

像复印文件——你拿走的是复印件，怎么改都不影响原件。减少了"一个人改了，所有人都受影响"的 bug。

### 协议导向

Swift 鼓励用"协议"（Protocol）定义行为规范，而不是用继承。

像定义"能力清单"——能飞、能游泳，而不是"鸟类继承动物类"。一架飞机和一只鸟都能"飞"，但它们不需要有共同的祖先。

### 为什么 Apple 要造 Swift

Objective-C 是 1980 年代的语言，到 2010 年已经非常过时：
- 语法晦涩（方括号满天飞：`[object method:arg]`）
- 没有现代语言特性（没有泛型、没有类型推断）
- 内存管理靠手动引用计数，容易出 bug

Apple 需要一个**现代化的语言来替代 Objective-C**，同时保持和 Apple 框架的完美兼容。Swift 应运而生。

### Swift 的"可选类型"为什么是革命性的

在 Objective-C/Java 里，任何对象都可能是 null。你调用一个方法返回 null，再调用它的方法——程序崩溃。这就是 `NullPointerException`，iOS 崩溃日志里最常见的错误。

Swift 说：**不行，你得显式告诉我"这个值可能没有"**。

```swift
func findUser(id: Int) -> User? {  // 返回 User? 表示"可能有，可能没有"
    // ...
}

let user = findUser(id: 1)
// user.name          // 编译报错！user 可能是 nil
// user?.name         // 安全：如果是 nil 就返回 nil
// user!.name         // 强制：我保证不是 nil（如果不是就崩溃）
```

这个设计**从语言层面消灭了空指针崩溃**。一旦习惯了，你会觉得其他语言都不安全。

### 常见误解

- **"Swift 只能写 iOS"**：Swift 也能写 macOS、watchOS、tvOS 应用，还能写服务端（Vapor）
- **"Swift 和 Objective-C 不兼容"**：完全兼容，可以混用，渐进迁移
- **"Swift 很难学"**：比 Objective-C 简单多了，有 Python/Kotlin 经验的人几天就能上手

---

## 设计哲学

Swift 诞生于 2014 年（Apple，Chris Lattner），解决的问题是：**Objective-C 是 1980 年代的语言，语法晦涩、没有现代特性、空指针崩溃频发**。Apple 需要一门现代化的语言来接班，同时不能破坏现有生态。

### 解决的核心问题

- **空指针崩溃**：用可选类型（Optional）从语言层面消灭 nil 崩溃
- **语法现代化**：闭包、泛型、模式匹配，告别 Objective-C 的方括号语法
- **性能**：编译型，运行速度接近 C/C++

### 关键取舍

Swift 的哲学是"**安全优先，但不牺牲性能**"：
- 强制处理空值（可选类型）→ 安全，但代码要多写解包逻辑
- 值类型优先（struct）→ 减少共享状态 bug，但和 Objective-C 互操作时需要桥接
- 和 Objective-C 完全兼容 → 可以渐进迁移，但也背上了一些历史包袱

Swift 假设"程序员会犯错，编译器应该拦住你"——这和 C 的"信任程序员"正好相反。

### 类比

Swift 像把老式跑车的"手动挡换成自动挡，再加全套主动安全系统"——速度没变快，但开起来更省心，新手也不会轻易追尾。它保留了 Objective-C 的全部动力（Apple 框架），只是让你开得更安全、更舒服。

---

## 安装与运行

```bash
swift --version                 # 查看版本
swift hello.swift               # 运行脚本
```

在线试用：[swift.org](https://www.swift.org/swiftly/) 或 Xcode Playground

---

## Hello World

```swift
print("Hello, World!")
```

---

## 变量与类型

```swift
var name: String = "张三"       // 可变
let age: Int = 25               // 不可变（推荐用 let）
let price = 9.99                // 自动推断 Double
let isAdult = true              // 自动推断 Bool
```

Swift 推荐用 `let`（不可变），只有需要修改时才用 `var`。

---

## 可选类型（Optional）

```swift
var email: String? = nil        // 可选类型：可能有值，可能是 nil
var phone: String = "123"       // 非可选：不能为 nil

// 安全解包
if let e = email {
    print(e)                    // email 有值时执行
} else {
    print("没有邮箱")
}

// 可选链
let count = email?.count        // email 为 nil 时返回 nil

// 强制解包（不推荐，可能崩溃）
// let e = email!

// 默认值
let e = email ?? "未设置"        // email 为 nil 时用默认值
```

---

## 控制流

```swift
// if-else
if age >= 18 {
    print("成年")
} else if age >= 12 {
    print("青少年")
} else {
    print("儿童")
}

// for-in 循环
for i in 1...5 {                // 1 到 5（包含）
    print(i)
}

for i in 1..<5 {                // 1 到 4（不包含 5）
    print(i)
}

for name in ["张三", "李四"] {
    print(name)
}

// switch（必须穷举所有情况）
switch grade {
case "A": print("优秀")
case "B": print("良好")
case "C": print("及格")
default: print("其他")
}
```

---

## 函数

```swift
// 基本函数
func add(a: Int, b: Int) -> Int {
    return a + b
}

// 参数标签（调用时用标签名）
func greet(name: String, greeting: String = "你好") {
    print("\(greeting)，\(name)！")
}

greet(name: "张三")
greet(name: "张三", greeting: "早上好")

// 多返回值
func divide(a: Double, b: Double) -> (result: Double, error: String?) {
    if b == 0 {
        return (0, "除数不能为 0")
    }
    return (a / b, nil)
}
```

---

## 结构体与协议

```swift
struct Student {
    var name: String
    var age: Int

    func greet() {
        print("你好，我叫\(name)")
    }
}

let s = Student(name: "张三", age: 20)
s.greet()

// 协议（= 接口）
protocol Greetable {
    func greet() -> String
}

struct Dog: Greetable {
    func greet() -> String {
        return "汪汪！"
    }
}
```

---

## 枚举

```swift
enum Direction {
    case north, south, east, west
}

let dir = Direction.north

// 带值的枚举
enum Result {
    case success(String)
    case failure(Error)
}

let result = Result.success("完成")
```

---

## 定位速查

| 维度 | Swift |
| :--- | :--- |
| **类型检查** | 静态类型 |
| **运行方式** | 编译型 |
| **核心特色** | 可选类型、协议导向、值类型优先 |
| **适合场景** | iOS/macOS 应用开发 |
| **代表框架** | SwiftUI、UIKit |

---

## 面试速答

### Q: Swift 的可选类型（Optional）解决什么问题？
A: 解决空指针崩溃。Swift 把"可能为空"明确标出来（`String?`），使用前必须解包，编译器强制你处理 nil 的情况。这样空指针错误在编译时就被拦住，而不是运行时崩溃。这是 Swift 相比 Objective-C/Java 最大的安全优势。

### Q: struct 和 class 有什么区别？
A: struct 是值类型（赋值时复制），class 是引用类型（赋值时共享引用）。struct 没有继承，class 可以继承。Swift 推荐优先用 struct——值类型避免了共享状态导致的 bug，且在多线程下更安全。Apple 框架里大量使用 struct。

### Q: Swift 和 Objective-C 能混用吗？
A: 能完全混用。Swift 可以调用 Objective-C 代码，Objective-C 也能调用 Swift（通过桥接头文件）。这让你可以渐进迁移老项目，不用一次性重写。Apple 框架大部分还是 Objective-C 写的，Swift 通过桥接无缝使用。

### Q: Swift 的协议（Protocol）和继承有什么区别？
A: 协议定义"能力"（能做什么），不关心实现者是谁；继承定义"是什么"（父子关系）。一个类型可以遵守多个协议（多能力），但只能继承一个父类。Swift 鼓励"协议导向编程"——用协议组合能力，比继承更灵活、耦合更低。

### Q: Swift 为什么推荐用 let 而不是 var？
A: let 是不可变（常量），var 是可变（变量）。不可变让代码更安全——不会在别处被意外修改，多线程下天然安全，编译器也能优化。Swift 的哲学是"默认不可变，需要改时才用 var"，这减少了大量因状态变化导致的 bug。

---

## 如何给别人讲

1. **一句话定位**：Swift 是 Apple 的现代编程语言，像"老式跑车的智能驾驶版"——速度没变，但更安全更好开
2. **核心卖点**：可选类型从语言层面消灭空指针崩溃、语法现代、性能接近 C
3. **设计取舍**：安全优先（强制处理 nil、值类型优先），代价是和 Objective-C 互操作时有学习成本
4. **谁在用**：iOS/macOS/watchOS/tvOS 应用开发，也能写服务端（Vapor）
5. **面试一句话**：Swift 的核心是可选类型和协议导向，取代了老旧的 Objective-C，是 Apple 生态首选语言
