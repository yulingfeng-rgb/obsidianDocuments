# Docker

> Docker 是"容器化"工具，把应用和它需要的一切（代码、库、配置）打包在一起，在任何机器上都能一致运行。解决了"在我电脑上能跑"的问题。

**关键词**：容器化、镜像、容器、Dockerfile、部署

## 抽象类比

Docker 像集装箱——不管里面装什么货物，集装箱的尺寸是标准的，任何港口、任何轮船都能运。

以前部署软件像搬家：要担心新家有没有煤气、有没有网线。Docker 像把整个房间打包带走，到哪都能直接住。

---

## 什么是 Docker

Docker 诞生的核心问题：**"在我电脑上能跑，到你那就不行了"**

原因：操作系统版本不同、依赖库版本不同、配置不同。

Docker 的解决方案：**把应用和它需要的一切打包成镜像**，在任何机器上运行都一样。

想象你要开连锁餐厅：
- **不用 Docker**：每家店单独装修、单独采购、单独培训，口味可能不一样
- **用 Docker**：把整个厨房打包，每家店直接复制，口味完全一致

---

## 核心概念

### 镜像（Image）：菜谱

镜像是一个只读的模板，包含操作系统、运行环境、代码和依赖库。像一份完整的菜谱——食材、调料、做法都写好了。

### 容器（Container）：做出来的菜

容器是镜像运行起来的实例。一个镜像可以启动多个容器。像用菜谱做出来的一份菜，可以运行、可以停止。

### Dockerfile：自己写菜谱

Dockerfile 告诉 Docker 怎么构建镜像，每行是一条指令。像你自己写的菜谱，告诉厨师怎么做菜。

### Docker Hub：菜谱大全

公共镜像仓库，别人写好的镜像拿来就行。像菜谱网站——里面有成千上万的菜谱，你想做什么菜直接搜。

### 为什么 Docker 改变了部署

Docker 之前，部署软件是这样的：
1. 开发环境装好，代码能跑
2. 测试环境装好，代码跑不起来——"我电脑上明明能跑！"
3. 生产环境装好，又出问题了——运维和开发互相甩锅

Docker 之后：
1. 开发把整个环境打成镜像
2. 运维直接拿镜像跑——环境一模一样，不扯皮了

**Docker 消灭了"在我电脑上能跑"这句话。**

### Docker vs 虚拟机

很多人问：Docker 和虚拟机有什么区别？

| 维度 | Docker | 虚拟机 |
| :--- | :--- | :--- |
| **类比** | 合租公寓（共用厨房卫生间） | 独栋别墅（每栋都有独立设施） |
| **启动速度** | 秒级 | 分钟级 |
| **资源占用** | 小（共享宿主机内核） | 大（每个都要完整 OS） |
| **隔离性** | 进程级（较弱） | 完全隔离（较强） |
| **适合场景** | 微服务、CI/CD、快速部署 | 需要完全隔离、跑不同 OS |

一句话：**需要快速部署、轻量级隔离用 Docker；需要完全隔离、跑不同操作系统用虚拟机。**

### 常见误解

- **"Docker 就是虚拟机"**：不是。Docker 共享宿主机内核，虚拟机有独立内核
- **"Docker 很安全"**：隔离性不如虚拟机，生产环境要注意安全配置
- **"Docker 只适合大公司"**：小项目也能用，一个人开发用 Docker 管理环境也很方便

---

## 设计哲学

### 一次构建，到处运行

Docker 解决的核心问题：环境不一致。开发、测试、生产环境不一致，导致"在我电脑上能跑"的经典甩锅。

Docker 的思路：**把应用和它依赖的一切（代码、库、配置、运行时）打包成镜像**。镜像搬到哪都能跑，因为环境在镜像里，不在机器里。

类比：以前养宠物要自己搭窝、准备口粮、装围栏，换个地方就得重来。Docker 像把整个"宠物套装"（窝+口粮+围栏+宠物）打包成一个箱子，搬到哪打开都能用。

### 共享内核，轻量隔离

Docker 容器不虚拟化完整操作系统，而是**共享宿主机内核**，只隔离进程、文件系统、网络。

为什么这么设计？
- **启动快**：不用启动完整 OS，秒级启动
- **资源省**：不用每个容器跑一份 OS，内存占用小
- **密度高**：一台机器能跑几百个容器，虚拟机只能跑几个

类比：虚拟机像每家都建独立别墅（自带水电煤气）；Docker 像合租公寓（共用水电煤气，但各人有各人的房间和门锁）。

### 关键取舍

- **隔离性 vs 性能**：容器共享内核，隔离性不如虚拟机，但性能好得多。Docker 选择牺牲一点隔离换性能
- **不可变基础设施**：镜像一旦构建就不变，要更新就重新构建新镜像。避免"改了配置忘了记录"的混乱
- **分层复用**：镜像由多层组成，公共层可以复用，节省存储和传输。但层数太多会影响性能

---

## 安装

- **Windows/Mac**：去 [docker.com](https://www.docker.com/products/docker-desktop/) 下载 Docker Desktop
- **Linux**：`curl -fsSL https://get.docker.com | sh`

```bash
docker --version              # 验证安装
docker info                   # 查看详细信息
```

---

## 镜像操作

```bash
docker pull nginx             # 下载镜像
docker pull nginx:1.24        # 下载指定版本
docker images                 # 查看本地所有镜像
docker rmi nginx              # 删除镜像（用镜像名或 ID）
docker rmi $(docker images -q)  # 删除所有镜像
docker build -t myapp:1.0 .   # 从 Dockerfile 构建镜像
docker tag myapp:1.0 user/myapp:1.0  # 给镜像打标签
```

---

## 容器操作

```bash
# 启动容器
docker run nginx                    # 前台运行
docker run -d nginx                 # 后台运行（最常用）
docker run -d -p 8080:80 nginx      # 映射端口（本机 8080 → 容器 80）
docker run -d --name myweb nginx    # 指定容器名
docker run -it ubuntu bash          # 交互式进入容器

# 查看容器
docker ps                       # 查看运行中的容器
docker ps -a                    # 查看所有容器（含已停止）

# 控制容器
docker stop 容器ID              # 停止
docker start 容器ID             # 启动
docker restart 容器ID           # 重启
docker kill 容器ID              # 强制停止

# 进入容器
docker exec -it 容器ID bash     # 进入容器内部（像 SSH 进去）

# 查看日志
docker logs 容器ID              # 查看容器日志
docker logs -f 容器ID           # 实时跟踪日志

# 删除容器
docker rm 容器ID                # 删除已停止的容器
docker rm -f 容器ID             # 强制删除运行中的容器
docker rm $(docker ps -aq)      # 删除所有容器
```

---

## Dockerfile

```dockerfile
FROM python:3.11                # 基础镜像
WORKDIR /app                    # 设置工作目录
COPY requirements.txt .         # 复制依赖文件
RUN pip install -r requirements.txt  # 安装依赖
COPY . .                        # 复制项目代码
EXPOSE 8000                     # 声明端口（仅文档作用）
CMD ["python", "app.py"]        # 容器启动时执行的命令
```

```bash
docker build -t myapp:1.0 .     # 构建镜像（注意最后的 .）
docker run -d -p 8000:8000 myapp:1.0  # 运行
```

### 常用指令

| 指令 | 作用 |
| :--- | :--- |
| `FROM` | 指定基础镜像 |
| `WORKDIR` | 设置工作目录 |
| `COPY` | 复制文件到镜像 |
| `ADD` | 类似 COPY，但支持自动解压 tar |
| `RUN` | 构建时执行命令 |
| `CMD` | 容器启动时执行（可被覆盖） |
| `ENTRYPOINT` | 容器启动时执行（不易被覆盖） |
| `EXPOSE` | 声明端口 |
| `ENV` | 设置环境变量 |

---

## 数据持久化

容器删除后数据会丢失。用卷（Volume）或挂载（Bind Mount）持久化。

```bash
# 数据卷（Docker 管理）
docker volume create mydata              # 创建卷
docker run -d -v mydata:/var/lib/mysql mysql  # 挂载卷

# 绑定挂载（本机目录）
docker run -d -v /home/user/data:/app/data myapp  # 本机目录:容器目录
```

---

## Docker Compose

多容器应用用 Docker Compose 一键管理。写一个 `docker-compose.yml`，一条命令启动所有服务。

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8000:8000"
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
    volumes:
      - dbdata:/var/lib/mysql

volumes:
  dbdata:
```

```bash
docker compose up -d          # 后台启动所有服务
docker compose down           # 停止并删除所有容器
docker compose ps             # 查看状态
docker compose logs -f        # 实时查看日志
docker compose restart web    # 重启某个服务
```

---

## Docker vs 虚拟机

| 维度 | Docker | 虚拟机 |
| :--- | :--- | :--- |
| **启动速度** | 秒级 | 分钟级 |
| **资源占用** | 小（共享内核） | 大（完整 OS） |
| **隔离性** | 进程级 | 完全隔离 |
| **适合场景** | 微服务、CI/CD | 需要完全隔离 |

---

## 面试速答

### Q: Docker 和虚拟机有什么区别？
A: Docker 共享宿主机内核，启动秒级，资源占用小，适合微服务。虚拟机有完整独立 OS，启动分钟级，资源占用大，隔离性强。需要轻量快速部署用 Docker，需要完全隔离跑不同 OS 用虚拟机。

### Q: Docker 镜像和容器什么关系？
A: 镜像是只读模板，包含运行应用所需的一切。容器是镜像运行起来的实例，可启动、停止、删除。一个镜像可以启动多个容器，像类和对象的关系。

### Q: Dockerfile 中 CMD 和 ENTRYPOINT 的区别？
A: CMD 可以被 docker run 后面的参数覆盖，适合指定默认启动命令。ENTRYPOINT 不容易被覆盖，docker run 后的参数会作为它的参数传入。通常用 ENTRYPOINT 指定程序，CMD 指定默认参数。

### Q: 容器删除后数据怎么办？
A: 容器本身是临时的，删除后数据丢失。需要用数据卷（Volume）或绑定挂载（Bind Mount）把数据存到宿主机，这样容器删除后数据还在。

### Q: Docker Compose 是什么？什么时候用？
A: Docker Compose 是用 YAML 文件定义和运行多容器应用的工具。单容器用 docker run 就够，但像"Web + 数据库 + 缓存"这种多服务应用，一个个 docker run 麻烦且容易出错。Compose 让你用一份 `docker-compose.yml` 描述所有服务，一条 `docker compose up -d` 启动整个应用。

---

## 可执行实践闭环

1. **练习任务**：把一个含数据库的应用写成多阶段镜像并用 Compose 启动。
2. **完成标准**：镜像是否可复现、容器是否无特权运行、数据是否挂载、健康检查是否真实。
3. **排错顺序**：按“容器状态 → 日志 → inspect → 网络/DNS → 挂载与权限”排查。
4. **沉淀结果**：保留最小示例、关键命令、失败现象与解决依据；过一周不看原文重新完成一次。
