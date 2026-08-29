# TypeScript

> TypeScript 是 JavaScript 的"加强版"，加了类型检查。它是 JavaScript 的超集，所有 JS 代码都能直接跑，但 TS 能在编译时发现错误。大项目首选。本文介绍其核心特性、设计哲学与常用语法。

**关键词**：类型系统、JS 超集、编译时检查、接口、泛型、渐进式

## 抽象类比

TypeScript 像 JavaScript 的"安全升级版"——功能一样，但加了安全带和倒车雷达。你开车的方式没变，但更安全了。

如果说 JavaScript 是自由奔放的越野车，TypeScript 就是同款车的"智能安全版"——加了车道保持、自动刹车，开起来更放心。

---

## 什么是 TypeScript

想象你要写一篇文章：
- **JavaScript**：像随手写草稿，想怎么写就怎么写，但可能有错别字
- **TypeScript**：像用 Word 写文档，有拼写检查、语法检查，写完自动纠错

TypeScript 诞生于 2012 年，由微软推出。目标是"**给 JavaScript 加上类型，让大项目更好维护**"。

TypeScript 最终编译成 JavaScript，所以能在任何 JS 环境运行。

### 为什么需要 TypeScript

JavaScript 有一个致命问题：**项目越大越容易翻车**。

小项目几十行代码，动态类型没问题。但大项目几万行代码时：
- 变量类型不确定，运行时才发现"这里怎么是 undefined"
- 重构时改了一个地方，不知道哪些地方会崩
- 团队协作时，不知道别人传的函数参数是什么类型

TypeScript 的解法：**在写代码的时候就告诉你类型对不对**。不用等到用户点了按钮才发现报错。

### TS 的设计取舍

TypeScript 选择了"**渐进式**"——不是另起炉灶，而是在 JavaScript 上面加一层。

这个选择很聪明：
- 所有 JS 代码都是合法 TS 代码 → 迁移成本低
- 编译后就是 JS → 能在任何 JS 环境运行
- 可以一点点加类型 → 不用一次改完

代价是什么？多了一步编译。但现代工具链（VS Code、Vite）已经让这一步几乎无感。

### 常见误解

- **"TS 比 JS 慢"**：错。TS 编译后就是 JS，运行速度一模一样
- **"TS 能在运行时检查类型"**：错。TS 的类型检查只在编译时，运行时类型信息会被擦除
- **"学了 TS 就不用学 JS 了"**：错。TS 是 JS 的超集，你得先会 JS

### 谁在用 TypeScript

- **前端**：React、Vue、Angular 的官方模板都支持 TS
- **后端**：NestJS（Node.js 框架）默认用 TS
- **大厂**：微软、Google、Airbnb、Shopify 全面采用
- **VS Code 本身**：就是用 TypeScript 写的

一句话总结：**小项目可以不用 TS，但只要项目超过 3 个人协作，TS 几乎是必须的。**

---

## 核心概念

### 类型系统：给变量贴标签

- **JavaScript**：变量像没标签的盒子，"今天装书，明天装衣服"
- **TypeScript**：变量像贴了标签的盒子，"这个盒子只能装书"

```typescript
let name: string = "Alice"  // 只能是字符串
let age: number = 25        // 只能是数字
name = 123  // 编译报错！类型不对
```

好处：编译时就能发现类型错误，不用等到运行时。

### 接口：定义"长什么样"

接口像一份"简历模板"，规定了你必须填哪些项。

```typescript
interface User {
  name: string
  age: number
  email?: string  // 可选
}
```

接口让代码有"契约"，团队协作更清晰。

### 泛型：灵活的容器

泛型像一个"万能盒子"，装什么类型都行，但装进去后就确定了。让代码既灵活又安全。

### 渐进式采用

TypeScript 是 JavaScript 的超集，所有 JS 代码都是合法的 TS 代码。可以一点点加类型，不用一次改完。

---

## 设计哲学

TypeScript 诞生于 2012 年（微软），解决的痛点很明确：**JavaScript 项目越大越容易翻车**。几万行 JS 代码里，一个类型错误可能要等用户点了按钮才暴露。TS 的目标：在写代码的时候就告诉你哪里错了。

### 解决的核心问题

- **类型安全**：编译时发现类型错误，不用等到运行时
- **大项目可维护**：重构时编译器告诉你哪些地方会崩
- **团队协作**：类型即文档，别人传什么参数一目了然

### 关键取舍

TypeScript 的哲学是"**渐进式增强，而不是另起炉灶**"：
- 它是 JS 的超集 → 所有 JS 代码都是合法 TS，迁移成本几乎为零
- 类型只在编译时存在 → 运行时类型信息被擦除，不增加运行开销
- 可以一点点加类型 → 不用一次性改造整个项目

代价是多了一步编译，但现代工具链让这步几乎无感。TS 不替代 JS，而是"给 JS 装上安全带"。

### 类比

TypeScript 像给越野车"加装车道保持和自动刹车"——驾驶方式（写代码的方式）没变，照样能去任何地方（编译成 JS 到处跑），但开起来更放心，不会再不知不觉开出车道。

---

## 安装与运行

```bash
npm install -g typescript       # 全局安装
tsc --init                      # 初始化配置（生成 tsconfig.json）
tsc hello.ts                    # 编译成 JS
tsc --watch                     # 监听模式（改完自动编译）
```

---

## Hello World

```typescript
function greet(name: string): string {
    return `你好，${name}！`
}

console.log(greet("张三"))
```

---

## 基础类型

```typescript
let name: string = "张三"       // 字符串
let age: number = 25            // 数字
let isAdult: boolean = true     // 布尔值
let nothing: null = null        // 空值
let notDefined: undefined = undefined

// 数组
let nums: number[] = [1, 2, 3]
let names: string[] = ["张三", "李四"]

// 元组（固定长度和类型）
let pair: [string, number] = ["张三", 25]

// any（任意类型，尽量避免）
let anything: any = "hello"
anything = 123                  // 不报错，但不安全
```

---

## 接口（Interface）

```typescript
interface User {
    name: string
    age: number
    email?: string              // 可选属性
}

const user: User = { name: "张三", age: 25 }
// const bad: User = { name: "张三" }  // 报错！缺少 age

// 函数参数类型
function greet(user: User): string {
    return `你好，${user.name}！`
}
```

---

## 函数

```typescript
// 参数类型 + 返回类型
function add(a: number, b: number): number {
    return a + b
}

// 可选参数
function greet(name: string, greeting?: string): string {
    return greeting ? `${greeting}，${name}！` : `你好，${name}！`
}

// 箭头函数
const multiply = (a: number, b: number): number => a * b
```

---

## 类

```typescript
class Student {
    name: string
    age: number

    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }

    greet(): void {
        console.log(`你好，我叫${this.name}`)
    }
}

const s = new Student("张三", 20)
s.greet()

// 继承
class GradStudent extends Student {
    major: string

    constructor(name: string, age: number, major: string) {
        super(name, age)
        this.major = major
    }
}
```

---

## 泛型

```typescript
function identity<T>(arg: T): T {
    return arg
}

identity<string>("hello")       // T 是 string
identity<number>(123)           // T 是 number

// 泛型接口
interface ApiResponse<T> {
    data: T
    status: number
}

const response: ApiResponse<string[]> = {
    data: ["张三", "李四"],
    status: 200
}
```

---

## 枚举

```typescript
enum Direction {
    Up,
    Down,
    Left,
    Right
}

let dir: Direction = Direction.Up

// 字符串枚举
enum Color {
    Red = "RED",
    Green = "GREEN",
    Blue = "BLUE"
}
```

---

## 类型断言与联合类型

```typescript
// 联合类型：可以是多种类型之一
let id: string | number
id = "123"
id = 456

// 类型断言：告诉编译器"我知道这是什么类型"
const el = document.getElementById("app") as HTMLInputElement
const len = (el as HTMLInputElement).value.length

// 类型守卫
function isString(val: string | number): val is string {
    return typeof val === "string"
}
```

---

## 定位速查

| 维度 | TypeScript |
| :--- | :--- |
| **类型检查** | 静态类型 |
| **运行方式** | 编译成 JS 后运行 |
| **核心特色** | 类型系统、接口、泛型、IDE 友好 |
| **适合场景** | 大型前端项目、团队协作、Node.js 后端 |
| **与 JS 关系** | 超集，所有 JS 代码都是合法 TS |
| **代表框架** | Angular、NestJS、Next.js |

---

## 面试速答

### Q: TypeScript 和 JavaScript 的关系是什么？
A: TypeScript 是 JavaScript 的超集——所有合法 JS 代码都是合法 TS 代码。TS 额外加了类型系统，编译后会擦除类型信息，输出纯 JS。所以 TS 能在任何 JS 环境运行，迁移成本极低。

### Q: TS 的类型检查在什么时候进行？
A: 只在编译时。TS 编译成 JS 后，所有类型信息会被擦除，运行时没有任何类型检查。所以 TS 不能做运行时类型校验——如果要校验外部数据（如 API 返回），需要额外的运行时验证库（如 zod）。

### Q: interface 和 type 有什么区别？
A: 功能上很接近，都能描述对象形状。区别：interface 支持声明合并（同名 interface 会合并），可以被类 implements；type 更灵活，能定义联合类型、交叉类型、条件类型等。一般描述对象用 interface，需要组合/联合时用 type。

### Q: 为什么大项目要用 TypeScript？
A: 类型即文档——看函数签名就知道参数和返回值类型；编译时发现错误——不用等运行时；重构更安全——改一个类型，所有受影响的地方编译器都会报错；IDE 智能提示更准。项目越大、人越多，这些收益越明显。

### Q: 什么是泛型？有什么用？
A: 泛型是"类型的参数"。定义时不指定具体类型，调用时再确定，让同一个函数/类能处理多种类型且保持类型安全。比如 `function identity<T>(arg: T): T`，传入 string 返回 string，传入 number 返回 number，类型不会丢失。
