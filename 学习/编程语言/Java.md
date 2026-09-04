# Java

> Java 是"企业级开发"的代名词，跨平台、强类型、生态庞大。后端、Android、大数据领域的首选。

**关键词**：面向对象、JVM、跨平台、强类型、Spring、垃圾回收

## 抽象类比

Java 像一辆重型卡车——启动慢、油耗大，但能拉几十吨货，跑起来稳如泰山。大项目、团队协作的首选。

如果说 Python 是家用轿车（灵活方便），C 是赛车（极致速度），Java 就是重型卡车——拉得多、跑得稳、适合长途运输。

---

## 什么是 Java

Java 诞生于 1995 年，设计者是 James Gosling（Sun 公司）。核心口号："**Write Once, Run Anywhere**"（一次编写，到处运行）。

Java 的核心：
- **面向对象**：一切皆对象
- **强类型**：编译时检查错误，大项目更安全
- **跨平台**：靠 JVM 实现"到处运行"

---

## 核心概念

### 面向对象：一切皆对象

Java 的代码必须写在类里。每个东西都是"对象"，对象有属性（数据）和方法（行为）。

像现实世界——"张三"是一个对象，有名字、年龄（属性），能说话、走路（方法）。你不需直接操作数据，而是通过"方法"让对象自己处理。

### 跨平台：一次编写，到处运行

Java 代码编译成"字节码"，由 JVM（Java 虚拟机）解释执行。只要目标机器装了 JVM，同一份代码就能跑。

像把信翻译成"世界语"——任何国家的人都能看懂。Windows、Linux、Mac 只要装了 JVM，都能跑同一份 Java 代码。

### 强类型：严格的检查站

变量必须先声明类型，编译时就检查类型错误。像填表格——每个格子规定了填什么，填错了直接打回。

好处：大项目不容易出错，IDE 智能提示更准确。坏处：代码啰嗦，写起来慢。

### 为什么 Java 能统治企业后端

Java 能在企业后端称霸 20 年，不是因为技术最先进，而是因为**生态最成熟**：
- **Spring 框架**：企业开发的一站式解决方案，几乎什么都有现成的
- **JVM 生态**：Kotlin、Scala、Groovy 都能跑在 JVM 上
- **稳定**：大银行、大公司的系统求稳，Java 20 年没出过大问题

Java 就像企业界的"英语"——不是最好用的语言，但所有人都懂。

### Java 的"危机"

近几年 Java 面临挑战：
- **Go**：云原生时代的新宠（Docker、K8s 都是 Go 写的）
- **Kotlin**：Android 官方推荐，比 Java 简洁 40%
- **Python**：AI 时代的首选

但 Java 的基本盘依然稳固——全球前 1000 家企业，80% 的后端有 Java。

### 常见误解

- **"Java 很慢"**：JVM 的 JIT 编译器经过 20 年优化，运行速度已经接近 C++
- **"Java 已经过时了"**：语言排行榜常年前三，企业招聘需求巨大
- **"Java 代码啰嗦"**：确实，但"啰嗦"也意味着"清晰"——大团队最怕的不是代码多，而是看不懂

---

## 设计哲学

Java 诞生于 1995 年，那时的痛点是：**C++ 太危险（内存泄漏满天飞），且代码绑定平台（Windows 写的程序不能跑在 Unix 上）**。James Gosling 想造一门"写一次到处运行"的语言。

### 解决的核心问题

- **跨平台**：代码编译成字节码，由 JVM 解释执行——任何装了 JVM 的机器都能跑
- **内存安全**：自动垃圾回收，不用手动 free，告别内存泄漏
- **大工程协作**：强类型 + 面向对象 + 规范语法，适合几十人团队

### 关键取舍

Java 的哲学是"**规范比灵活更重要**"：
- 用强类型和啰嗦语法换安全 → 代价是代码冗长（Hello World 要五六行）
- 用 JVM 换跨平台 → 代价是启动慢、占内存（"重型卡车"）
- 用虚拟机层换隔离 → 代价是性能不如直接编译的 C

Java 不追求"短小精悍"，而追求"十年后还能维护"。啰嗦代码恰恰是大型团队最需要的——清晰胜过聪明。

### 类比

Java 像一个"标准集装箱"——又重又大，但港口都认它，装上船就能运到任何地方。它的价值不在于箱子本身多先进，而在于整个物流体系（JVM 生态、Spring 框架、海量库）都围绕它建立。

---

## 安装与运行

```bash
java --version                  # 查看版本
javac Hello.java                # 编译
java Hello                      # 运行
```

---

## Hello World

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

---

## 变量与类型

```java
int age = 25;                   // 整数
double price = 9.99;            // 浮点数
String name = "张三";           // 字符串
boolean isAdult = true;         // 布尔值
char grade = 'A';               // 字符

final int MAX = 100;            // 常量（不可修改）
```

---

## 控制流

```java
// if-else
if (age >= 18) {
    System.out.println("成年");
} else if (age >= 12) {
    System.out.println("青少年");
} else {
    System.out.println("儿童");
}

// for 循环
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}

// 增强 for（遍历数组/集合）
int[] nums = {1, 2, 3, 4, 5};
for (int n : nums) {
    System.out.println(n);
}

// switch
switch (grade) {
    case 'A': System.out.println("优秀"); break;
    case 'B': System.out.println("良好"); break;
    default:  System.out.println("其他"); break;
}
```

---

## 类与面向对象

```java
class Student {
    String name;
    int age;

    // 构造方法
    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // 方法
    void greet() {
        System.out.println("你好，我叫" + name);
    }
}

Student s = new Student("张三", 20);
s.greet();
```

### 继承

```java
class Animal {
    void speak() {
        System.out.println("...");
    }
}

class Dog extends Animal {
    @Override
    void speak() {
        System.out.println("汪汪！");
    }
}
```

### 接口

```java
interface Runnable {
    void run();
}

class Runner implements Runnable {
    public void run() {
        System.out.println("跑起来了！");
    }
}
```

---

## 集合框架

```java
import java.util.*;

// List（有序、可重复）
List<String> list = new ArrayList<>();
list.add("张三");
list.add("李四");
list.get(0);                    // "张三"

// Map（键值对）
Map<String, Integer> map = new HashMap<>();
map.put("张三", 90);
map.put("李四", 85);
map.get("张三");                // 90

// Set（不重复）
Set<String> set = new HashSet<>();
set.add("张三");
set.add("张三");                // 不会重复添加
```

---

## Lambda 与 Stream

```java
List<String> names = Arrays.asList("张三", "李四", "王五");

// Lambda 表达式
names.forEach(name -> System.out.println(name));

// Stream 过滤 + 收集
List<String> result = names.stream()
    .filter(name -> name.startsWith("张"))
    .collect(Collectors.toList());
```

---

## 异常处理

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("除数不能为 0: " + e.getMessage());
} finally {
    System.out.println("无论如何都会执行");
}
```

---

## 定位速查

| 维度 | Java |
| :--- | :--- |
| **类型检查** | 静态类型 |
| **运行方式** | 编译 + 解释（JVM） |
| **核心特色** | 面向对象、跨平台、生态庞大 |
| **适合场景** | 企业后端、Android、大数据 |
| **代表框架** | Spring Boot、Hibernate |

---

## 面试速答

### Q: Java 为什么能"一次编写，到处运行"？
A: Java 代码编译成字节码（.class），不是机器码。字节码由 JVM 解释执行，只要目标机器装了 JVM，就能跑同一份字节码。JVM 屏蔽了底层操作系统和硬件的差异。

### Q: JVM 的垃圾回收（GC）是怎么工作的？
A: GC 自动回收不再被引用的对象，程序员不用手动 free。基本思路是"标记-清除"：先从根对象出发标记所有可达对象，然后回收不可达的。现代 JVM 用分代回收（新生代用复制算法，老年代用标记整理），兼顾效率和碎片。

### Q: Java 的接口和抽象类有什么区别？
A: 接口只定义行为规范（方法签名），不包含实现（Java 8 后可有 default 方法）；抽象类可以包含成员变量和实现方法。一个类只能继承一个抽象类，但可以实现多个接口。接口用于定义"能力"，抽象类用于共享"模板"。

### Q: Spring 为什么能统治 Java 后端？
A: Spring 提供了一站式解决方案——依赖注入（解耦组件）、AOP（统一处理日志/事务）、Spring Boot（约定优于配置，开箱即用）。它把企业开发常见的痛点都封装好了，加上 20 年积累的生态，几乎成了 Java 后端的"标配"。

### Q: Java 真的很慢吗？
A: 不。JVM 的 JIT 编译器会在运行时把热点代码编译成机器码，运行速度已经接近 C++。启动确实慢（JVM 预热），但长期运行的服务端程序性能很好。慢的印象来自早期 Java 和 GUI 程序。

