# Redis

> Redis 是内存数据库，数据存在内存里，读写极快。常用作缓存、会话存储、消息队列。

**关键词**：内存数据库、缓存、键值存储、高性能、持久化

## 抽象类比

Redis 像一个超快的白板——数据写在白板上，看一眼就知道（微秒级），但白板擦掉就没了（重启会丢数据，需要配置持久化）。

如果说 MySQL 是仓库（数据存得多、安全，但取货要跑一趟），Redis 就是收银台旁边的货架（常用商品随手就拿，但放不下太多东西）。

---

## 什么是 Redis

Redis 全称 "Remote Dictionary Server"（远程字典服务器），是一个**基于内存**的键值数据库。

为什么需要 Redis？

MySQL 数据在硬盘上，每次读写要"跑一趟"——磁盘 I/O 是瓶颈。Redis 数据在内存里，"伸手就拿到"。速度差距：
- **MySQL**：毫秒级（1ms ~ 10ms）
- **Redis**：微秒级（0.1ms 左右）

典型用法：**MySQL 做主存储，Redis 做缓存**。热点数据放 Redis，减少 MySQL 压力。

---

## 核心概念

### 五种数据类型

Redis 不只是"键值对"那么简单，它支持五种数据类型，每种适合不同场景：

| 类型 | 类比 | 适合场景 |
| :--- | :--- | :--- |
| **String** | 便利贴 | 缓存、计数器 |
| **Hash** | 小表格 | 存储对象（用户信息） |
| **List** | 排队队列 | 消息队列、最新消息 |
| **Set** | 不重复的集合 | 标签、去重 |
| **ZSet** | 排行榜 | 排名、带权重的队列 |

### Redis 的典型用法

Redis 最常见的角色是**缓存**——把频繁访问的数据放内存里，减少数据库压力。

场景举例：
- **微博热搜**：每分钟几百万次访问，不可能每次都查 MySQL。放 Redis 里，微秒级响应
- **用户 Session**：用户登录后存在 Redis 里，过期自动删除
- **排行榜**：用 ZSet 天然支持排序，游戏排名、销量排名
- **分布式锁**：多个服务抢同一个资源，用 Redis 做锁

### Redis 的持久化

Redis 数据在内存里，重启会丢。怎么办？

- **RDB（快照）**：定时把内存数据保存到硬盘。像拍照——每隔一段时间拍一张，恢复时看最后一张照片
- **AOF（追加日志）**：每次写操作都记录到日志。像录像——记录每一步操作，恢复时重放

RDB 恢复快但可能丢数据（两次快照之间的操作丢了），AOF 丢的数据少但文件大、恢复慢。可以两个一起用。

### Redis vs Memcached

Memcached 也是内存缓存，为什么 Redis 赢了？
- Redis 支持更多数据类型（Memcached 只有 String）
- Redis 支持持久化（Memcached 重启全丢）
- Redis 支持集群、发布订阅、Lua 脚本

**Memcached 几乎已经被 Redis 淘汰了。**

### 常见误解

- **"Redis 能替代 MySQL"**：不能。Redis 是缓存，MySQL 是持久化存储。两者配合使用
- **"Redis 数据不会丢"**：默认会丢（重启），需要配置持久化。即使配了也可能丢一点点
- **"Redis 很快，所以什么都往里放"**：内存贵，Redis 只放热点数据，全量数据还是存 MySQL

---

## 设计哲学

### 内存优先，速度至上

Redis 的核心设计：**数据存在内存里**。内存读写速度是硬盘的 10 万倍，Redis 把这个优势用到极致。

为什么这么设计？因为很多场景（缓存、会话、排行榜）对速度的要求远高于对持久性的要求。与其在硬盘上慢吞吞地查，不如在内存里瞬间拿到。

类比：MySQL 像仓库（东西多、安全，但取货要跑一趟），Redis 像柜台（东西少、但伸手就拿到）。仓库和柜台配合使用最好。

### 单线程模型

Redis 的核心命令执行是单线程的。为什么单线程还这么快？
- **内存操作本身就快**：内存读写微秒级，不需要多线程来掩盖延迟
- **无锁无竞争**：没有多线程的锁竞争和上下文切换开销
- **I/O 多路复用**：一个线程监听多个连接，事件驱动

类比：一个超级高效的收银员，比多个互相抢客户的收银员更快。因为不用协调、不用锁、不用切换。

### 关键取舍

- **速度 vs 安全**：内存快但重启会丢数据。Redis 用 RDB/AOF 持久化来弥补，但本质上仍是缓存而非主存储
- **单线程 vs 多核**：单线程用不满多核 CPU。Redis 选择简单可靠，通过多实例部署来利用多核
- **功能 vs 性能**：Redis 支持多种数据结构，但每个操作都要保证 O(1) 或 O(log N) 的复杂度，不会为复杂功能牺牲速度

---

## 安装与启动

```bash
# Ubuntu
apt install redis-server
systemctl start redis-server

# macOS
brew install redis
brew services start redis

# Docker
docker run -d -p 6379:6379 redis
```

```bash
redis-cli                       # 进入命令行
redis-cli ping                  # 测试连接（返回 PONG）
```

---

## String（字符串）

最基础的类型，能存字符串、数字、甚至图片。

```bash
SET name "张三"                 # 设置值
GET name                        # 获取值 → "张三"

SET counter 1                   # 设置数字
INCR counter                    # 自增 → 2
INCRBY counter 10               # 加 10 → 12

SET session "token" EX 3600     # 设置值，1 小时后过期（适合缓存）
TTL session                     # 查看剩余存活时间
```

---

## Hash（哈希）

存储对象，像一个小表格。

```bash
HSET user:1 name "张三"         # 设置字段
HSET user:1 age 25
HSET user:1 email "zhang@example.com"

HGET user:1 name                # 获取字段 → "张三"
HGETALL user:1                  # 获取所有字段和值
HMSET user:1 name "李四" age 30 # 同时设置多个字段
HDEL user:1 email               # 删除字段
```

---

## List（列表）

有序的队列，可以从两端推入/弹出。

```bash
LPUSH messages "你好"           # 从左边推入
LPUSH messages "世界"
RPUSH messages "结尾"           # 从右边推入

LRANGE messages 0 -1            # 查看所有元素
LRANGE messages 0 2             # 查看前 3 个

LPOP messages                   # 从左边弹出
RPOP messages                   # 从右边弹出

LLEN messages                   # 列表长度
```

---

## Set（集合）

不重复的无序集合。

```bash
SADD tags "python"              # 添加元素
SADD tags "redis"
SADD tags "python"              # 重复添加，不会生效

SMEMBERS tags                   # 查看所有元素
SISMEMBER tags "python"         # 判断是否存在（返回 1）
SCARD tags                      # 元素个数

SREM tags "redis"               # 删除元素
```

---

## ZSet（有序集合）

每个元素带一个分数（score），按分数排序。

```bash
ZADD leaderboard 100 "张三"     # 添加元素，分数 100
ZADD leaderboard 200 "李四"
ZADD leaderboard 150 "王五"

ZRANGE leaderboard 0 -1 WITHSCORES  # 按分数从低到高查看所有
ZREVRANGE leaderboard 0 2 WITHSCORES  # 从高到低，前 3 名

ZRANK leaderboard "张三"        # 查看排名（从低到高）
ZSCORE leaderboard "李四"       # 查看分数

ZINCRBY leaderboard 50 "张三"   # 增加分数
```

---

## 通用命令

```bash
KEYS *                          # 查看所有键（生产环境慎用）
KEYS user:*                     # 查看匹配模式的键
EXISTS name                     # 判断键是否存在
DEL name                        # 删除键
EXPIRE name 60                  # 设置过期时间（秒）
TTL name                        # 查看剩余存活时间
TYPE name                       # 查看键的类型
```

---

## 发布与订阅

```bash
SUBSCRIBE news                  # 订阅频道
PUBLISH news "今日头条"          # 发布消息到频道
```

---

## Redis vs MySQL

| 维度 | Redis | MySQL |
| :--- | :--- | :--- |
| **存储位置** | 内存 | 硬盘 |
| **读写速度** | 微秒级 | 毫秒级 |
| **数据结构** | 丰富（5 种） | 关系表 |
| **持久化** | 可选 | 默认 |
| **适合场景** | 缓存、高频读写 | 持久化存储、复杂查询 |

---

## 持久化

### RDB（快照）

定时把内存数据保存到硬盘。重启后恢复到最后一次快照。

```
# redis.conf
save 900 1        # 900 秒内至少 1 次修改就保存
save 300 10       # 300 秒内至少 10 次修改就保存
```

### AOF（追加日志）

每次写操作都记录到日志。数据更安全，但文件更大。

```
# redis.conf
appendonly yes    # 开启 AOF
appendfsync everysec  # 每秒同步一次
```

---

## 面试速答

### Q: Redis 为什么快？
A: 数据在内存里（微秒级读写），核心命令单线程执行（无锁竞争和上下文切换），使用 I/O 多路复用处理并发连接。单线程反而更快，因为内存操作本身极快，瓶颈不在 CPU 而在 I/O。

### Q: Redis 的持久化方式 RDB 和 AOF 怎么选？
A: RDB 是定时快照，恢复快但可能丢数据。AOF 是追加日志，丢数据少但文件大、恢复慢。对数据安全要求高用 AOF，对恢复速度要求高用 RDB，生产环境通常两个都开。

### Q: 缓存穿透、缓存击穿、缓存雪崩的区别？
A: 穿透是查不存在的数据，绕过缓存直接打数据库，用布隆过滤器防范。击穿是热点 key 过期瞬间大量请求打数据库，用互斥锁或热点数据永不过期。雪崩是大量 key 同时过期，用过期时间加随机值打散。

### Q: Redis 单线程为什么还能支持高并发？
A: Redis 用 I/O 多路复用（epoll）一个线程监听多个连接，命令执行在内存中极快（微秒级），不需要多线程。真正耗时的大命令（如 KEYS *）会阻塞，所以生产环境要避免。

### Q: Redis 五种数据类型分别适合什么场景？
A: String 适合缓存和计数器；Hash 适合存对象（如用户信息，比 String 存 JSON 更新字段更高效）；List 适合消息队列和最新消息列表；Set 适合去重和标签；ZSet（有序集合）适合排行榜和带权重的队列。选对数据类型能让操作复杂度保持 O(1) 或 O(log N)。

---

## 可执行实践闭环

1. **练习任务**：实现带过期与缓存击穿保护的查询缓存。
2. **完成标准**：键命名、TTL、容量上限、淘汰策略、持久化和一致性策略是否明确。
3. **排错顺序**：先观察延迟、命中率、内存、慢查询和连接数，再决定优化。
4. **沉淀结果**：保留最小示例、关键命令、失败现象与解决依据；过一周不看原文重新完成一次。
