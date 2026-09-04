# Go

> Go 是 Google 开发的编程语言，天生支持并发，编译速度快，语法简洁。适合写后端服务、微服务、云原生工具。

**关键词**：goroutine、channel、并发、编译型、云原生、组合优于继承

## 抽象类比

Go 像瑞士军刀——功能不花哨，但每样都好用。编译快、运行快、并发简单，是"工程化"的典范。

如果说 Java 是功能齐全的厨房（什么都能做但复杂），Go 就是一个精简的料理台——只有最常用的工具，但每样都打磨得很好。

---

## 什么是 Go

Go 诞生于 2009 年，由 Google 的三位大牛（Rob Pike、Ken Thompson、Robert Griesemer）设计。设计目标：**解决大型软件开发中的痛点——编译慢、依赖管理混乱、并发难写**。

Go 的核心信条：
- **简单**：语法少，学习快，代码风格统一
- **高效**：编译极快，运行接近 C
- **并发友好**：goroutine + channel，并发像写同步代码一样简单

Docker、Kubernetes、Terraform 都是 Go 写的。

---

## 核心概念

### 并发不是并行

- **并行**：同时做多件事（多核同时跑）——像多个厨师同时做菜
- **并发**：同时处理多件事（一个核来回切换）——像一个厨师同时照看多个锅

Go 的并发模型是 goroutine + channel：
- **goroutine**：轻量级线程，启动成本极低，可以轻松开几万个
- **channel**：goroutine 之间的传话筒，数据通过 channel 传递，避免共享内存

### 组合优于继承

Go 没有类、没有继承，用结构体 + 接口组合来实现复用。像搭积木——把不同功能的积木块组合在一起，而不是一层套一层。

好处：代码扁平、容易理解。没有"继承链太深导致改一处崩全身"的问题。

### 编译即部署

Go 编译成单一二进制文件，不需要运行时、不需要依赖库。编译完直接扔到服务器上就能跑。像把整个应用"冻干"了，到哪儿加水就能用。

### 为什么 Go 在云原生时代称王

Docker、Kubernetes、Terraform、Prometheus、etcd……云原生的基础设施几乎全是 Go 写的。

原因：
- **并发强**：微服务就是大量并发请求，goroutine 天生擅长
- **编译快**：几秒编译完，开发体验好
- **二进制部署**：不需要装运行时，Docker 镜像可以小到几 MB
- **语法简单**：新人看两天代码就能上手

Go 不是"最好的语言"，但是"最适合写基础设施的语言"。

### Go 的取舍

Go 故意做得很简单：
- **没有泛型**（2022 年才加了基础泛型）
- **没有类、没有继承**
- **错误处理靠返回值**（没有 try-catch）

很多人觉得"太简陋了"。但 Go 的设计哲学是：**简单比功能多更重要**。一个团队 100 人，代码风格统一，比一个人写出花哨代码更重要。

### 常见误解

- **"Go 不适合写业务逻辑"**：其实很多公司的后端 API 就是 Go 写的，性能极好
- **"Go 的并发是万能的"**：goroutine 虽轻，但几百万并发时内存也会爆
- **"Go 很简单，不需要学"**：语法简单 ≠ 写好代码简单，并发编程的坑还是很多的

---

## 设计哲学

Go 的设计动机很现实：**Google 内部的 C++ 代码编译要几十分钟，依赖管理一团乱，新人上手要几个月**。三位创始人决定造一门"为大型工程而生"的语言。

### 解决的核心问题

- **编译慢**：Go 把编译速度做到极致，几万行代码秒级编译完
- **依赖乱**：强制包管理（go mod），依赖关系一目了然
- **并发难**：把并发做成语言级特性（goroutine），像写普通函数一样简单

### 关键取舍

Go 的哲学是"**少即是多**"——刻意砍掉很多东西：
- 砍掉继承和类 → 用组合代替，代码更扁平
- 砍掉异常 → 用返回值处理错误，流程更清晰
- 砍掉泛型（直到 2022 年才加）→ 保持语言简单

这些取舍换来的是：**新人两天能上手，十万行代码一个人也能看懂**。

### 类比

Go 像一辆"丰田海拉克斯皮卡"——没有真皮座椅和自动泊车，但结实、省油、谁都会开，扔到沙漠里照样跑。它不追求"让你一个人写出最酷的代码"，而是追求"一百人的团队代码风格完全一致"。

---

## 安装与运行

```bash
go version                      # 查看版本
go run hello.go                 # 直接运行
go build hello.go               # 编译成可执行文件
go mod init myproject           # 初始化模块
go get github.com/gin-gonic/gin # 安装第三方库
```

---

## Hello World

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

---

## 变量与类型

```go
var name string = "张三"        // 显式声明
var age int = 25
const pi float64 = 3.14159     // 常量

name := "李四"                  // 短变量声明（自动推断类型）
age := 25
```

常用类型：`int`, `float64`, `string`, `bool`, `[]int`（切片）, `map[string]int`（字典）

---

## 控制流

```go
// if-else（条件不用括号）
if age >= 18 {
    fmt.Println("成年")
} else {
    fmt.Println("未成年")
}

// for 循环（Go 只有 for）
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// for-range 遍历切片
names := []string{"张三", "李四", "王五"}
for i, name := range names {
    fmt.Printf("%d: %s\n", i, name)
}

// switch（不需要 break）
switch age {
case 18:
    fmt.Println("刚成年")
case 25:
    fmt.Println("青年")
default:
    fmt.Println("其他")
}
```

---

## 函数

```go
// 基本函数
func add(a int, b int) int {
    return a + b
}

// 多返回值
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("除数不能为 0")
    }
    return a / b, nil
}

// 调用
result, err := divide(10, 3)
if err != nil {
    fmt.Println("错误:", err)
}
```

---

## 结构体与方法

```go
type Student struct {
    Name string
    Age  int
}

// 方法：绑定到结构体
func (s Student) Greet() string {
    return "你好，我叫" + s.Name
}

s := Student{Name: "张三", Age: 20}
fmt.Println(s.Greet())
```

---

## 接口

Go 的接口是"隐式实现"的——只要实现了接口要求的方法，就算实现了接口。

```go
type Speaker interface {
    Speak() string
}

type Dog struct{}

func (d Dog) Speak() string {
    return "汪汪！"
}

func greet(s Speaker) {
    fmt.Println(s.Speak())
}

greet(Dog{})  // Dog 实现了 Speaker 接口，可以传入
```

---

## 切片与字典

```go
// 切片（动态数组）
nums := []int{1, 2, 3}
nums = append(nums, 4, 5)       // 追加元素

// 字典
scores := map[string]int{
    "张三": 90,
    "李四": 85,
}
scores["王五"] = 88               // 添加/修改
delete(scores, "李四")             // 删除
```

---

## 并发：goroutine 与 channel

```go
// goroutine：轻量级并发任务
go func() {
    fmt.Println("后台任务")
}()

// channel：goroutine 之间通信
ch := make(chan string)

go func() {
    ch <- "消息"                   // 发送
}()

msg := <-ch                       // 接收
fmt.Println(msg)
```

---

## 错误处理

Go 没有 try-catch，用返回值处理错误。

```go
result, err := someFunction()
if err != nil {
    fmt.Println("出错了:", err)
    return
}
fmt.Println("结果:", result)
```

---

## 定位速查

| 维度 | Go |
| :--- | :--- |
| **类型检查** | 静态类型 |
| **运行方式** | 编译型 |
| **核心特色** | goroutine 并发、channel 通信、编译极快 |
| **适合场景** | 后端服务、微服务、云原生工具（Docker/K8s 都是 Go 写的） |
| **代表框架** | Gin、Echo |

---

## 面试速答

### Q: Go 的 goroutine 和操作系统线程有什么区别？
A: goroutine 是 Go 运行时管理的轻量级线程，初始栈仅 2KB，由 Go 调度器在用户态切换，创建和切换成本极低，可以轻松创建十万级。操作系统线程通常需要 MB 级栈空间，由内核调度，切换需陷入内核态，成本高得多。

### Q: Go 为什么不用 try-catch 处理错误？
A: Go 认为错误是正常流程的一部分，应该显式处理而不是抛出去。用返回值（`result, err`）让错误处理一目了然，强制程序员在每个调用点决定怎么处理。这避免了异常中断流程、错误被忽略的问题。

### Q: channel 和共享内存有什么区别？
A: Go 的信条是"不要通过共享内存通信，而要通过通信共享内存"。channel 让 goroutine 之间传数据像传话筒，天然避免数据竞争；共享内存需要加锁，容易写出死锁和竞态。channel 更安全也更易理解。

### Q: Go 的接口为什么是隐式实现的？
A: Go 不需要像 Java 那样显式声明 `implements`，只要实现了接口的方法就算实现。这让接口的定义和实现解耦——你可以给第三方库的类型实现自己的接口，不用改它的代码。这也是"组合优于继承"的体现。

### Q: Go 适合做什么？不适合做什么？
A: 适合做后端服务、微服务、云原生工具（Docker/K8s 都是 Go 写的）、CLI 工具。不适合做系统级底层（留给 C/Rust）、GUI 桌面应用、数据科学和 AI。

