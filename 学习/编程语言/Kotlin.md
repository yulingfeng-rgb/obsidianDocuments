# Kotlin

> Kotlin 是 Android 官方推荐语言，语法简洁、空安全、兼容 Java。可以理解为"更现代的 Java"。本文介绍其核心特性、设计哲学与常用语法。

**关键词**：空安全、协程、Java 互操作、Android 开发、JVM、JetBrains

## 抽象类比

Kotlin 像 Java 的"精装升级版"——功能一样，但去掉了啰嗦的语法，加了空安全、协程等现代特性。

如果说 Java 是一本厚重的百科全书（全面但啰嗦），Kotlin 就是同一本书的"精华浓缩版"——内容一样，但更精炼、更好读。

---

## 什么是 Kotlin

Kotlin 诞生于 2011 年，由 JetBrains（IntelliJ IDEA 的开发商）设计。2017 年 Google 宣布 Kotlin 为 Android 官方推荐语言。

Kotlin 的设计目标：**保留 Java 的全部生态，同时修复它的痛点**。

Kotlin 的核心信条：
- **空安全**：编译时消灭 NullPointerException
- **简洁**：同样的功能，代码量比 Java 少 40%
- **协程**：异步代码写起来像同步
- **100% Java 互操作**：可以直接调用所有 Java 库

---

## 核心概念

### 空安全：编译器帮你防空指针

Java 最大的坑是 `NullPointerException`（程序员的噩梦）。Kotlin 在类型系统层面解决：

- `String`：不能为 null，编译器保证
- `String?`：可以为 null，但使用前必须处理空值

像给每个盒子贴了标签——"这个盒子可能有东西，也可能没东西"。打开之前你必须先检查，编译器会盯着你。

### 协程：异步不再痛苦

Java 的异步写起来很痛苦（回调地狱）。Kotlin 的协程让异步代码写起来像同步代码。

像做菜：
- **回调方式**：放锅 → 等水开 → 下面条 → 等面熟 → 捞面（每一步都要等）
- **协程方式**：烧水的同时切菜 → 水开了下面 → 等面熟的同时调味（看起来像同步，实际不阻塞）

### 与 Java 100% 互操作

Kotlin 编译后也是 JVM 字节码，可以直接调用 Java 库，Java 也能调用 Kotlin。像两种语言住在同一个小区，互相串门毫无障碍。

### 为什么 Google 选了 Kotlin 而不是 Java

2017 年 Google 宣布 Kotlin 为 Android 首选语言，原因：
- **Java 的痛点太明显**：NullPointerException 是 Android 崩溃的头号原因
- **Java 语法太啰嗦**：Android 开发体验差，代码量巨大
- **Kotlin 完美兼容**：不需要重写已有的 Java 代码，直接混用

Google 的潜台词：**Java 还是能用，但我们更推荐你用 Kotlin**。

### Kotlin 的杀手锏：协程

Android 开发最头疼的是异步——网络请求、数据库查询、动画，都不能卡主线程。

Java 的异步：回调套回调，代码嵌套五六层，俗称"回调地狱"。

Kotlin 的协程：异步代码写起来像同步代码。

```kotlin
// 看起来像同步，实际是异步
val user = api.getUser()      // 网络请求，但不阻塞
val orders = api.getOrders()  // 继续异步
showResult(user, orders)
```

### 常见误解

- **"学了 Kotlin 就不用学 Java 了"**：Kotlin 依赖 Java 生态，很多库还是 Java 写的，得能看懂 Java
- **"Kotlin 只能写 Android"**：Kotlin 也能写后端（Ktor、Spring Boot 支持），还能编译成 JS
- **"Kotlin 比 Java 快"**：运行速度一样（都跑在 JVM 上），Kotlin 的优势是开发效率，不是运行速度

---

## 设计哲学

Kotlin 诞生于 2011 年（JetBrains），解决的痛点是：**Java 语法太啰嗦、空指针崩溃太多，但 Java 生态太庞大不能抛弃**。Kotlin 的目标：保留 Java 全部生态，同时修复它的痛点。

### 解决的核心问题

- **空指针崩溃**：用可空类型（`String?`）在编译时消灭 NullPointerException
- **代码冗长**：同样的功能，代码量比 Java 少约 40%
- **异步痛苦**：用协程让异步代码写起来像同步代码

### 关键取舍

Kotlin 的哲学是"**不另起炉灶，在 Java 之上做精装修**"：
- 100% 互操作 → 可以直接调用所有 Java 库，但也要忍受 Java 的历史设计
- 编译成 JVM 字节码 → 能跑在 JVM 上，但运行速度和 Java 一样（没有性能优势）
- 语法丰富（扩展函数、数据类、协程）→ 简洁，但学习的东西更多

Kotlin 不追求"取代 Java"，而追求"让写 Java 生态的人更舒服"。

### 类比

Kotlin 像在老房子里"搞精装修"——不拆承重墙（Java 生态和 JVM），只换软装和水电（语法、空安全、协程）。你住着还是同一套房子，但体验焕然一新。老家具（Java 库）照样能用，新装修让它更好用。

---

## 安装与运行

```bash
kotlinc hello.kt -include-runtime -d hello.jar  # 编译
java -jar hello.jar                             # 运行
```

在线试用：[play.kotlinlang.org](https://play.kotlinlang.org)

---

## Hello World

```kotlin
fun main() {
    println("Hello, World!")
}
```

---

## 变量与类型

```kotlin
val name: String = "张三"       // 不可变（推荐）
var age: Int = 25               // 可变
val price = 9.99                // 自动推断 Double
val isAdult = true              // 自动推断 Boolean

// 可空类型
var email: String? = null       // 可以为 null
var phone: String = "123"       // 不能为 null，赋值 null 会编译报错
```

---

## 空安全

```kotlin
var name: String? = "张三"

// name.length                  // 编译报错！可能为 null
name?.length                    // 安全调用：null 时返回 null
name?.length ?: 0               // Elvis 运算符：null 时返回 0
name!!.length                   // 强制非空：null 时抛异常

// 智能转换
if (name != null) {
    println(name.length)        // 这里 name 自动变成非空
}
```

---

## 控制流

```kotlin
// if-else（可以当表达式用）
val status = if (age >= 18) "成年" else "未成年"

// when（= switch 加强版）
when (grade) {
    "A" -> println("优秀")
    "B" -> println("良好")
    "C" -> println("及格")
    else -> println("其他")
}

// for 循环
for (i in 1..5) println(i)      // 1 到 5（包含）
for (i in 1 until 5) println(i) // 1 到 4（不包含 5）
for (name in listOf("张三", "李四")) println(name)

// while
while (n > 0) {
    println(n)
    n--
}
```

---

## 函数

```kotlin
// 基本函数
fun add(a: Int, b: Int): Int {
    return a + b
}

// 单行函数（省略花括号）
fun add(a: Int, b: Int) = a + b

// 默认参数
fun greet(name: String, greeting: String = "你好") {
    println("$greeting，$name！")
}

greet("张三")
greet("张三", "早上好")          // 命名参数
greet(greeting = "早上好", name = "张三")  // 可以乱序
```

---

## 数据类

```kotlin
data class User(val name: String, val age: Int)

val user = User("张三", 25)
println(user.name)              // "张三"
println(user)                   // User(name=张三, age=25)（自动生成 toString）

val (name, age) = user          // 解构赋值
```

`data class` 自动生成 `toString`、`equals`、`copy` 等方法。

---

## 集合操作

```kotlin
val list = listOf(1, 2, 3, 4, 5)

list.filter { it > 3 }          // [4, 5]
list.map { it * 2 }             // [2, 4, 6, 8, 10]
list.filter { it > 3 }.map { it * 2 }  // 链式调用 → [8, 10]

val map = mapOf("张三" to 90, "李四" to 85)
map["张三"]                      // 90
```

---

## 协程

```kotlin
import kotlinx.coroutines.*

// 定义挂起函数
suspend fun fetchData(): String {
    delay(1000)                  // 模拟网络请求
    return "数据"
}

// 使用协程
fun main() = runBlocking {
    val result = fetchData()     // 写起来像同步，实际是异步
    println(result)
}
```

---

## 定位速查

| 维度 | Kotlin |
| :--- | :--- |
| **类型检查** | 静态类型 |
| **运行方式** | 编译 + 解释（JVM）/ 编译为 JS / 原生编译 |
| **核心特色** | 空安全、协程、与 Java 互操作 |
| **适合场景** | Android 开发、后端服务 |
| **代表框架** | Spring Boot（Kotlin 支持）、Ktor |

---

## 面试速答

### Q: Kotlin 怎么解决空指针异常？
A: 通过类型系统区分可空类型（`String?`）和非空类型（`String`）。非空类型不能赋值为 null，编译器保证；可空类型使用前必须处理空值（用 `?.` 安全调用或 `!!` 强制非空）。这样空指针错误在编译时就被消灭。

### Q: Kotlin 和 Java 能完全互操作吗？
A: 能。Kotlin 编译成 JVM 字节码，可以直接调用所有 Java 库，Java 也能调用 Kotlin 代码。一个项目里可以同时有 .java 和 .kt 文件。这让现有 Java 项目能渐进迁移到 Kotlin，不用一次性重写。

### Q: Kotlin 的协程是什么？比线程好在哪？
A: 协程是轻量级的并发单元，由 Kotlin 运行时调度，不是操作系统线程。优势：挂起时不阻塞线程（可以干别的），代码写起来像同步（没有回调地狱），创建成本极低（可以开几万个）。适合异步网络请求、数据库查询等 I/O 密集场景。

### Q: data class 是什么？为什么好用？
A: data class 是专门用来存数据的类，自动生成 toString、equals、hashCode、copy、解构等方法。Java 里这些方法要手写或用 Lombok，Kotlin 一行搞定：`data class User(val name: String, val age: Int)`。大幅减少样板代码。

### Q: Kotlin 比 Java 好在哪？
A: 空安全（编译时消灭 NPE）、代码简洁（少约 40%）、协程（异步像同步）、扩展函数（给现有类加方法不用继承）、data class（少写样板）。运行速度和 Java 一样（都跑在 JVM），优势在开发效率和安全性。
