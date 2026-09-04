# Shell

> Shell 是操作系统的命令解释器，也是用户与内核之间的桥梁。它既是一个交互式命令行，又是一门脚本语言，能将一系列命令组合成自动化流程。本文以 bash 为主线，系统梳理 Shell 脚本编程的语法、文本处理三剑客、管道重定向与最佳实践。

**关键词**：bash、脚本编程、管道重定向、grep、sed、awk、shellcheck

## 抽象类比

Shell 像"乐高积木的连接器"。操作系统本身已经提供了很多小工具（ls、cat、grep、sort），每个工具像一块独立的积木，而 Shell 用"管道"把这些积木串起来，让前一块的输出变成后一块的输入，最终拼出复杂的自动化流程。

再打个比方：图形界面像"成品家具"，好看但功能固定；Shell 像"零件 + 工具箱"，初始学习陡峭，但能任意组合出你想要的所有家具。运维工程师、算法工程师、后端开发几乎每天都会用到 Shell。

## 一、Shell 是什么

Shell 是**用户与操作系统内核之间的命令解释器**，承担两个角色：

- **交互式命令行**：用户输入命令，Shell 解析并交给内核执行
- **脚本语言**：把一系列命令写入 `.sh` 文件，批量执行

它位于最外层（"壳"的英文就是 shell），内核在里层：

```mermaid
flowchart LR
    A[用户输入命令] --> B[Shell 解释]
    B --> C[系统调用]
    C --> D[操作系统内核]
    D --> E[硬件]
    E --> |结果| D
    D --> |返回| C
    C --> |输出| B
    B --> |显示| A
```

Shell 的核心价值：

- 自动化：重复任务脚本化
- 管道组合：小工具串成大流程
- 远程管理：SSH + Shell 是服务器管理标配
- 与系统深度集成：能直接调用文件系统、进程、网络

## 二、Shell 的种类

主流 Shell 一览：

- **sh（Bourne Shell）**：最早的现代 Shell，所有 Shell 的"共同祖先"
- **bash（Bourne Again Shell）**：sh 的增强版，Linux 事实标准
- **zsh**：功能丰富，oh-my-zsh 配置后体验极佳，macOS 默认
- **fish**：开箱即用，语法不兼容 bash
- **PowerShell**：微软的现代化 Shell，对象管道，跨平台

本文以 bash 为主线，因为它是 Linux 发行版默认 Shell，几乎所有 CI/CD 环境兼容，语法兼容 sh，可移植性最好。查看与切换默认 Shell：

```bash
echo $SHELL          # 当前默认 Shell
cat /etc/shells      # 系统支持的所有 Shell
chsh -s /bin/zsh     # 修改默认 Shell
```

## 三、基础语法

### 1. Shebang

脚本第一行指定解释器：

```bash
#!/bin/bash         # 用 bash 解释
#!/usr/bin/env bash # 更通用的写法
#!/usr/bin/python3  # Python 脚本
```

### 2. 变量

```bash
name="Shell"        # 赋值，等号两边不能有空格
echo $name          # 取值要加 $
echo "Hello, ${name}!"  # 推荐用花括号明确边界
```

- 单引号 `''`：原样输出，不展开变量
- 双引号 `""`：会展开变量和命令替换
- 反引号或 `$()`：命令替换，执行命令并取输出

### 3. 字符串与数组

```bash
str="Hello World"
echo ${#str}              # 字符串长度：11
echo ${str:0:5}           # 截取：Hello
echo ${str/world/shell}   # 替换

arr=(apple banana cherry)
echo ${arr[0]}            # apple
echo ${arr[@]}            # 全部元素
echo ${#arr[@]}           # 元素个数：3
arr+=(date)              # 追加
```

### 4. 注释与特殊变量

```bash
# 单行注释
: '多行注释用冒号 + 引号块'
```

特殊变量速查：

- `$0`：脚本名；`$1 ~ $9`：位置参数；`$#`：参数个数
- `$@`：所有参数（每个独立）；`$*`：所有参数（合并成单字符串）
- `$?`：上一条命令退出码；`$$`：当前进程 PID

## 四、流程控制

### 1. 条件判断

```bash
if [ -f file.txt ]; then
    echo "文件存在"
elif [ -d dir ]; then
    echo "是目录"
else
    echo "都不存在"
fi
```

`test` 命令常用判断：

- 文件：`-f` 存在且是普通文件、`-d` 是目录、`-e` 存在
- 字符串：`-z` 为空、`-n` 非空、`==` 相等
- 数字：`-eq`、`-ne`、`-lt`、`-gt`、`-le`、`-ge`
- 逻辑：`&&` 与、`||` 或、`!` 非

### 2. 循环

```bash
# for 遍历
for i in 1 2 3; do
    echo $i
done

# for C 风格
for ((i=0; i<5; i++)); do
    echo $i
done

# while
while [ $count -lt 10 ]; do
    count=$((count + 1))
done

# until：条件不满足时执行
until [ $count -ge 10 ]; do
    count=$((count + 1))
done
```

### 3. case 语句

```bash
case $1 in
    start)
        echo "启动服务"
        ;;
    stop)
        echo "停止服务"
        ;;
    restart)
        echo "重启服务"
        ;;
    *)
        echo "用法: $0 {start|stop|restart}"
        ;;
esac
```

### 4. break 与 continue

- `break`：跳出整个循环
- `continue`：跳过本次循环剩余语句
- `break n` / `continue n`：操作 n 层循环

## 五、函数与作用域

### 1. 函数定义与参数

```bash
greet() {
    echo "Hello, $1"     # $1 是函数第一个参数
    return 0              # 返回退出码 0~255
}
greet "Shell"
```

- 函数内 `$1 $2 ...` 表示函数参数，`$#` 是参数个数
- 函数不能直接返回字符串，只能返回退出码

### 2. local 变量与返回值

```bash
counter() {
    local count=0    # 只在函数内可见
    count=$((count + 1))
    echo $count
}
```

- 不写 local 的变量默认是全局的，会污染调用者作用域
- `return`：从函数返回，仅影响 `$?`
- `exit`：直接结束整个脚本
- 想从函数"返回字符串"常用：函数内 `echo` 输出，调用者用 `$(func)` 捕获

```bash
get_name() { echo "Shell"; }
name=$(get_name)
```

## 六、文本处理三剑客

### 1. grep：查找

- 基本用途：在文本中按模式搜索行
- 常用参数：
  - `-i` 忽略大小写
  - `-v` 反向匹配
  - `-n` 显示行号
  - `-r` 递归搜索目录
  - `-E` 扩展正则
  - `-c` 只输出匹配行数

```bash
grep "error" app.log          # 找 error 行
grep -rn "TODO" src/           # 递归找 TODO
grep -E "[0-9]{4}" data.txt    # 找 4 位数字
grep -v "^#" config.conf       # 去掉注释行
```

### 2. sed：流编辑器

- 基本用途：对文本流做替换、删除、插入

```bash
sed 's/foo/bar/' file.txt       # 替换第一个
sed 's/foo/bar/g' file.txt      # 替换所有
sed '/^#/d' file.txt            # 删除注释行
sed -i 's/old/new/g' file.txt   # 原地修改
sed -n '10,20p' file.txt         # 打印 10-20 行
```

### 3. awk：列处理

- 基本用途：按列处理文本
- 内置变量：`NR` 行号、`NF` 列数、`FS` 分隔符

```bash
awk '{print $1, $3}' file.txt              # 打印第 1、3 列
awk -F',' '{print $2}' data.csv            # 逗号分隔
awk '$3 > 100 {print $0}' score.txt        # 筛选
awk '{sum += $1} END {print sum}' nums.txt  # 求和
```

### 4. 三剑客组合实战

```bash
# 统计每个 IP 的访问次数，取前 10
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10

# 提取 ERROR 行中的时间戳
grep "ERROR" app.log | awk '{print $1}' | sort | uniq -c
```

## 七、管道与重定向

### 1. 标准流

- 标准输入 stdin（fd 0）
- 标准输出 stdout（fd 1）
- 标准错误 stderr（fd 2）

### 2. 重定向符

- `>`：覆盖写入文件
- `>>`：追加写入文件
- `<`：从文件读入
- `2>`：重定向错误
- `&>`：合并标准输出与错误
- `2>&1`：把错误重定向到标准输出
- `/dev/null`：黑洞，丢弃输出

```bash
cmd > out.log 2> err.log     # 分别保存
cmd > all.log 2>&1           # 合并到同一文件
cmd &> all.log               # 上式简写
cmd > /dev/null 2>&1         # 丢弃所有输出
```

### 3. 管道

- `|`：把左侧命令的 stdout 作为右侧命令的 stdin
- 数据流式传递，不需要中间临时文件
- 可链式调用多个命令

```bash
cat log | grep error | wc -l
ps aux | grep python | awk '{print $2}'
```

### 4. 子 Shell 与命令分组

- `(cmd)`：在子进程中执行，不影响当前环境，如 `(cd /tmp && pwd)` 后当前目录不变
- `{ cmd; }`：在当前 shell 执行，影响环境，`{}` 后必须有 `;` 或换行

## 八、常用内置命令与工具

### 1. 输出与读取

- `echo`：简单输出；`printf`：格式化输出，如 `printf "%-10s %5d\n" "Name" 100`
- `read`：从 stdin 读取，如 `read -p "请输入姓名：" name`

### 2. test 与 []

- `test -f file` 等价于 `[ -f file ]`
- `[[ ]]` 是 bash 增强版：支持模式匹配、逻辑运算符更自然
- 例：`if [[ $str == *.txt ]]; then echo "是 txt"; fi`

### 3. source、export、xargs、find

- `source file.sh` 或 `. file.sh`：在当前 shell 执行脚本，常用于加载配置
- `export VAR`：把变量导出为环境变量，子进程可见
- `xargs`：把 stdin 转成命令参数，常与 find 配合
  - `find . -name "*.log" | xargs rm`
  - `find . -name "*.py" | xargs grep "TODO"`
- `find`：按条件查找文件
  - `find . -type f -mtime -7`：7 天内修改
  - `find . -size +100M`
  - `find . -name "*.tmp" -delete`

### 4. 其他常用工具

- `cut`：按列切分，`cut -d',' -f2 data.csv`
- `sort`：排序，`sort -t',' -k2 -n`
- `uniq`：去重（需配合 sort），`sort file | uniq -c`
- `tr`：字符替换，`tr 'a-z' 'A-Z' < file.txt`
- `head` / `tail`：取头部 / 尾部；`tail -f app.log` 实时查看日志
- `wc`：统计行数、词数、字符数

## 九、Shell 脚本实战

### 1. 批量重命名文件

```bash
#!/bin/bash
# 把所有 .txt 改成 .md
for file in *.txt; do
    mv "$file" "${file%.txt}.md"
done
```

### 2. 日志分析

```bash
#!/bin/bash
# 统计今天访问量最多的 10 个 IP
log="/var/log/nginx/access.log"
grep "$(date +%d/%b/%Y)" "$log" | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
```

### 3. 定时任务

- `crontab -e` 编辑定时任务，格式为 `分 时 日 月 周 命令`
- `0 2 * * * /home/user/backup.sh`：每天凌晨 2 点备份
- `0 9 * * 1 /home/user/report.sh`：每周一 9 点跑报告

### 4. CI 脚本与环境初始化

```bash
#!/bin/bash
set -euo pipefail
pip install -r requirements.txt
pytest tests/ --cov=src
docker build -t myapp:${CI_COMMIT_SHA:0:7} .
docker push myapp:${CI_COMMIT_SHA:0:7}
```

环境初始化常用模式：

```bash
WORK_DIR="$HOME/project"
mkdir -p "$WORK_DIR" && cd "$WORK_DIR"
git clone git@github.com:org/repo.git && cd repo
pip install -r requirements.txt
```

## 十、Shell 编程的陷阱与最佳实践

### 1. Quoting 陷阱

- 变量不加引号会在空格处被拆分
- 始终用双引号包裹变量：`"$var"`

```bash
# 危险：文件名带空格会出错
for f in $(ls); do ...; done

# 安全
for f in *; do echo "$f"; done
```

### 2. 空格敏感

```bash
if[$x=$y]      # 错
if [ "$x" = "$y" ]  # 对，括号内两侧必须有空格
```

### 3. 子Shell 副作用

管道在子 shell 中执行，子 shell 内对变量的修改不会影响父 shell：

```bash
# 错误：count 不会改变
count=0
cat file | while read line; do count=$((count + 1)); done
echo $count   # 0

# 正确：用进程替换，循环在当前 shell 执行
count=0
while read line; do count=$((count + 1)); done < <(cat file)
echo $count
```

### 4. set 选项

脚本开头加上 `set -euo pipefail` 可大幅提升健壮性：

- `set -e`：出错立即退出
- `set -u`：使用未定义变量报错
- `set -o pipefail`：管道中任意一段失败则整体失败

### 5. shellcheck

- 静态分析工具，能发现大量陷阱
- 可集成到 CI、VS Code 插件
- 每写完一个脚本都过一遍 shellcheck

```bash
shellcheck myscript.sh
```

### 6. 其他最佳实践

- 脚本加 Shebang `#!/usr/bin/env bash`，关键路径加 `set -euo pipefail`
- 函数化、参数化，避免硬编码，复杂逻辑写注释
- 路径与变量始终加双引号，用 `[[ ]]` 代替 `[ ]`
- 临时文件用 `mktemp`，避免命名冲突
- 退出时清理临时资源，可用 `trap` 注册退出回调

## 面试速答

- **Q1：管道的本质是什么？**
  A：管道把前一个命令的标准输出连接到后一个命令的标准输入，数据在内存中流式传递，无需中间临时文件。底层是通过 `pipe()` 系统调用 + 子进程实现的。

- **Q2：`set -euo pipefail` 各起什么作用？**
  A：`-e` 让命令失败时立即退出脚本；`-u` 让使用未定义变量报错；`pipefail` 让管道的退出码取最后一个失败的命令。三者组合能极大提升脚本的健壮性。

- **Q3：单引号和双引号的区别？**
  A：单引号内所有字符原样输出，不展开变量也不做命令替换；双引号内会展开 `$var`、`$(cmd)` 等结构。涉及变量时务必用双引号包裹避免空格拆分。

- **Q4：grep、sed、awk 的核心定位分别是什么？**
  A：grep 负责按模式查找行；sed 负责对文本流做替换、删除等编辑；awk 擅长按列处理结构化文本。三者经常通过管道组合完成复杂任务。

- **Q5：如何避免 Shell 脚本中变量未定义导致的隐蔽错误？**
  A：脚本开头加 `set -u` 让未定义变量直接报错；变量始终用双引号包裹；对依赖的外部环境变量做存在性检查；用 shellcheck 做静态扫描。

## 相关链接

- 内部链接：[[Linux]]、[[正则表达式]]、[[Git]]、[[CICD]]、[[运维基础]]
- 外部资源：
  - [Bash 官方手册](https://www.gnu.org/software/bash/manual/)
  - [ShellCheck 在线工具](https://www.shellcheck.net/)
  - [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line)