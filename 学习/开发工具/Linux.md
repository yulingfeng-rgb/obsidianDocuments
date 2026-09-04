# Linux

> Linux 是一个开源免费的操作系统，服务器、手机（Android）、嵌入式设备的首选。程序员几乎都要和它打交道。

**关键词**：操作系统、命令行、开源、文件系统、权限

## 抽象类比

Linux 像一辆手动挡卡车——没有 Windows 那么花哨的界面，但稳定、高效、可控。专业司机（程序员）都爱开它。

Windows 像自动挡轿车，开箱即用、界面友好；Linux 像手动挡卡车，启动要懂点原理，但跑起来又快又稳。

---

## 什么是 Linux

Linux 诞生于 1991 年，设计者是 Linus Torvalds（芬兰大学生）。最初只是"想免费替代 Minix 操作系统"的 hobby project，没想到成了全世界服务器和开发者的首选。

Linux 的核心信条：
- **一切皆文件**：硬件、进程、设备都用文件表示
- **小工具组合**：每个工具只做一件事，组合起来威力巨大
- **命令行优先**：图形界面是可选的，命令行才是核心
- **开源免费**：谁都能用、谁都能改

---

## 核心概念

### 文件系统：树状结构

Linux 的文件系统像一棵倒过来的树，根目录是 `/`。所有东西都挂在这棵树上——你的文件、系统配置、甚至硬件设备。

```
/
├── home/        # 用户主目录（你的文件在这里）
├── etc/         # 配置文件
├── var/         # 可变数据（日志等）
├── usr/         # 用户程序
├── tmp/         # 临时文件
├── bin/         # 基本命令
├── dev/         # 设备文件
└── proc/        # 进程信息
```

### 命令行：和电脑对话

Windows 像触屏手机，点一点就行；Linux 像语音助手，你要用文字告诉它做什么。

命令行看起来原始，但效率远超鼠标——批量操作、自动化脚本、远程管理，都得靠它。

### 权限管理：谁能做什么

Linux 像一栋办公楼，每个人有不同的门禁卡。
- **r（读）**：能看文件
- **w（写）**：能改文件
- **x（执行）**：能运行文件

### 包管理：应用商店

包管理器像手机的应用商店，一键安装软件。Ubuntu 用 `apt`，CentOS 用 `yum`，Arch 用 `pacman`。

### 为什么服务器都用 Linux

全球服务器 90%+ 跑的是 Linux。原因：
- **免费**：Windows Server 要钱，Linux 不要
- **稳定**：Linux 服务器可以跑几年不重启
- **安全**：开源，漏洞被发现和修复得快
- **性能好**：没有图形界面的开销，资源全给业务
- **可定制**：想要什么装什么，不想要什么删什么

### Linux 的历史

1991 年，芬兰赫尔辛基大学的学生 Linus Torvalds 觉得 Minix 操作系统不够用，自己写了一个内核发到网上，说"just a hobby, won't be big and professional"。

结果全世界程序员一起贡献代码，Linux 变成了今天统治服务器和手机（Android 基于 Linux）的操作系统。

**Linux 是"开源协作"最成功的案例**——没有一家公司控制它，但全世界都在用。

### Linux 发行版

Linux 内核 + 各种工具 = 发行版。常见的：
- **Ubuntu**：最流行，新手友好，社区大
- **CentOS**：企业常用，稳定（但已停止维护，替代品是 Rocky Linux）
- **Arch**：滚动更新，最新软件，但容易搞崩，"我用 Arch btw" 是程序员梗
- **Debian**：Ubuntu 的上游，极其稳定

### 常见误解

- **"Linux 很难"**：命令行入门需要几天，但学会后效率远超 Windows
- **"Linux 没有图形界面"**：有的，GNOME、KDE 等桌面环境，日常办公没问题
- **"Linux 不适合开发"**：恰恰相反，程序员首选，工具链最完善

---

## 设计哲学

### 一切皆文件

Linux 最核心的设计理念：**所有东西都抽象成文件**。普通文件是文件，硬件设备是文件（/dev/sda），进程信息是文件（/proc），网络配置是文件。

好处：用同一套工具（cat、grep、echo）就能操作所有东西。不用为每种设备学新命令。

类比：如果每个设备都是不同形状的插头，你需要一堆不同的插座。Linux 把所有东西都做成统一形状的 USB 接口——一套工具通吃。

### 小工具组合

Linux 的每个命令只做一件事，但做得极好。`grep` 只管搜索，`sort` 只管排序，`wc` 只管计数。用管道（|）把它们串起来，威力巨大。

类比：乐高积木——每块积木很简单，但组合起来能搭出任何东西。不需要一个"万能工具"，而是用小工具拼出解决方案。

### 命令行优先

Linux 把命令行作为核心，图形界面只是可选附加。为什么？
- **效率**：一行命令完成鼠标点几十次的操作
- **自动化**：命令可以写进脚本，批量执行
- **远程管理**：服务器在机房，靠 SSH 命令行管理

### 关键取舍

- **简洁 vs 易用**：命令行学习曲线陡，但学会后效率远超图形界面。Linux 选择服务专业用户
- **稳定 vs 新潮**：Linux 优先保证稳定性，服务器可以几年不重启。新功能不会贸然加入
- **开源 vs 商业**：开源让 Linux 被全世界审查和改进，但也意味着没有单一公司兜底

---

## 文件与目录

```bash
ls                              # 列出当前目录文件
ls -la                          # 详细信息 + 隐藏文件
ls -lh                          # 人类可读的文件大小

cd /home/user                   # 切换目录
cd ~                            # 回到家目录
cd ..                           # 回到上一级

pwd                             # 显示当前路径

mkdir dir                       # 创建目录
mkdir -p a/b/c                  # 递归创建多级目录

touch file.txt                  # 创建空文件
cp file.txt backup.txt          # 复制文件
cp -r dir1 dir2                 # 递归复制目录
mv old.txt new.txt              # 移动/重命名
mv file.txt /other/path/        # 移动到指定目录
rm file.txt                     # 删除文件
rm -r dir/                      # 递归删除目录（危险！）
rm -rf dir/                     # 强制递归删除（非常危险！）
```

---

## 查看与搜索文件

```bash
cat file.txt                    # 查看文件全部内容
less file.txt                   # 分页查看（按 q 退出）
head -n 20 file.txt             # 查看前 20 行
tail -n 20 file.txt             # 查看后 20 行
tail -f log.txt                 # 实时跟踪日志文件

find / -name "*.txt"            # 按名字查找文件
find . -size +100M              # 查找大于 100M 的文件

grep "关键词" file.txt           # 在文件中搜索文本
grep -r "关键词" .               # 递归搜索目录下所有文件
grep -i "hello" file.txt        # 忽略大小写
grep -n "关键词" file.txt        # 显示行号
```

---

## 权限管理

Linux 像一栋办公楼，每个人有不同的门禁卡。

```bash
ls -la                          # 查看权限（-rw-r--r-- 格式）
```

权限格式：`-rwxrwxrwx` → 所有者 / 同组 / 其他人，每组 r（读）w（写）x（执行）

```bash
chmod 755 file                  # 设置权限（rwxr-xr-x）
chmod +x script.sh              # 添加执行权限
chmod -w file.txt               # 去掉写权限

chown user:group file           # 修改所有者
chown -R user:group dir/        # 递归修改目录所有者
```

---

## 用户管理

```bash
whoami                          # 当前用户名
sudo command                    # 以 root 权限执行
su - username                   # 切换用户

useradd username                # 创建用户
passwd username                 # 设置密码
userdel username                # 删除用户
usermod -aG groupname username  # 把用户加入某个组
```

---

## 包管理

包管理器像手机的应用商店，一键安装软件。

```bash
# Ubuntu/Debian
apt install nginx               # 安装
apt remove nginx                # 卸载
apt update                      # 更新软件源
apt upgrade                     # 升级所有软件
apt search nginx                # 搜索软件

# CentOS/RHEL
yum install nginx
yum remove nginx
yum update

# Arch
pacman -S nginx
pacman -R nginx
```

---

## 进程管理

```bash
ps                              # 查看进程
ps aux                          # 查看所有进程详情
top                             # 实时监控进程（按 q 退出）
htop                            # 更好用的 top（需安装）

kill PID                        # 终止进程
kill -9 PID                     # 强制终止
killall nginx                   # 按名字终止所有进程

bg                              # 把任务放到后台
fg                              # 把任务切回前台
jobs                            # 查看后台任务
nohup command &                 # 退出终端后仍继续运行
```

---

## 网络相关

```bash
ping baidu.com                  # 测试网络连通性
curl https://baidu.com          # 发送 HTTP 请求
wget url                        # 下载文件

ip addr                         # 查看 IP 地址
ifconfig                        # 查看网络接口（旧版）
netstat -tlnp                   # 查看端口占用
ss -tlnp                        # 查看端口占用（新版）

scp file user@host:/path        # 远程复制文件到服务器
ssh user@host                   # 远程登录
```

---

## 压缩与解压

```bash
tar -czf archive.tar.gz dir/    # 压缩为 .tar.gz
tar -xzf archive.tar.gz         # 解压 .tar.gz
tar -xjf archive.tar.bz2        # 解压 .tar.bz2

zip -r archive.zip dir/         # 压缩为 .zip
unzip archive.zip               # 解压 .zip
```

---

## 管道与重定向

管道像流水线——上一个命令的输出直接喂给下一个命令。

```bash
cat file.txt | grep "关键词"     # 查看文件并搜索
ls -la | sort -k5 -n            # 按文件大小排序
cat file.txt | wc -l            # 统计行数

echo "hello" > file.txt         # 写入文件（覆盖）
echo "world" >> file.txt        # 追加到文件
command 2>&1                    # 合并标准输出和错误输出
command > /dev/null             # 丢弃输出
```

---

## Vim 基础

Linux 上的文本编辑器，服务器上没有图形界面只能用它。

```bash
vim file.txt                    # 打开文件
```

| 操作 | 按键 |
| :--- | :--- |
| 进入编辑模式 | `i` |
| 退出编辑模式 | `Esc` |
| 保存 | `:w` |
| 退出 | `:q` |
| 保存并退出 | `:wq` 或 `:x` |
| 强制退出不保存 | `:q!` |
| 跳到行尾 | `G` |
| 跳到第 n 行 | `:n` |
| 删除一行 | `dd` |
| 搜索 | `/关键词`，按 `n` 下一个 |

---

## 面试速答

### Q: Linux 中硬链接和软链接的区别？
A: 硬链接指向同一个 inode（文件实体），删除原文件硬链接仍有效，不能跨文件系统。软链接（符号链接）指向文件路径，删除原文件软链接就失效，可以跨文件系统。软链接像 Windows 的快捷方式。

### Q: chmod 755 是什么意思？
A: 755 是 rwxr-xr-x 的缩写。7=4+2+1（读+写+执行）给所有者，5=4+1（读+执行）给同组用户，5=4+1（读+执行）给其他用户。常用在脚本和可执行文件上。

### Q: 怎么查看端口被哪个进程占用？
A: 用 `netstat -tlnp` 或 `ss -tlnp` 查看所有监听端口和对应进程。也可以用 `lsof -i:端口号` 查指定端口。找到 PID 后用 `kill` 终止。

### Q: Linux 为什么适合做服务器？
A: 免费、稳定（可以几年不重启）、安全（开源漏洞修复快）、性能好（无图形界面开销）、可远程管理（SSH 命令行）。全球 90%+ 服务器跑 Linux。

### Q: Linux 中管道（|）是什么？有什么用？
A: 管道把上一个命令的输出直接喂给下一个命令的输入，是 Linux "小工具组合"哲学的体现。比如 `cat log.txt | grep "error" | wc -l` 一行就能统计日志里 error 出现的次数。不用管道的话，要先把结果存文件、再用 grep 处理、再统计——繁琐得多。