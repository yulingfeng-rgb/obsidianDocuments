# Git

> Git 是版本控制工具，记录每次修改代码的历史，随时可以回退、分支协作。几乎所有程序员每天都在用它，不会 Git 等于不会团队合作。

## 一句话理解

Git 像游戏的"存档系统"——每打完一关存个档，打输了可以读档重来。多人玩的时候，每个人有自己的存档线，互不影响，最后合并。

---

## 第一步：安装与配置

### 安装 Git

- **Windows**：去 [git-scm.com](https://git-scm.com) 下载安装，一路默认即可
- **Linux**：`sudo apt install git`

### 配置身份

Git 需要知道你是谁，每次提交都会记录你的名字和邮箱。

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

> `--global` 表示全局设置，只配一次。`--global` 可以换成 `--local` 只对某个仓库生效。

### 验证安装

```bash
git --version
```

能输出版本号就说明装好了。

---

## 第二步：创建仓库

### 方式一：从零开始

```bash
mkdir my-project
cd my-project
git init
```

这会在当前目录创建一个 `.git` 隐藏文件夹，你的仓库就建好了。

### 方式二：克隆远程仓库

```bash
git clone https://github.com/xxx/xxx.git
```

这会把远程仓库完整复制到本地，包括所有历史记录。

---

## 第三步：日常三板斧（add → commit → push）

这是你每天重复最多的流程：

```bash
# 1. 查看当前状态（养成习惯，随时看看改了啥）
git status

# 2. 把修改加入暂存区
git add .                # 暂存所有修改
git add 文件名           # 只暂存某个文件

# 3. 提交到本地仓库
git commit -m "说明这次改了什么"

# 4. 推送到远程仓库
git push
```

### 写好的 commit 信息

好的提交信息像标题——一眼看懂这次改了什么。

```bash
# 好的
git commit -m "修复登录页面验证码不刷新的 bug"
git commit -m "新增用户头像上传功能"

# 差的
git commit -m "改了点东西"
git commit -m "update"
```

---

## 第四步：分支（Branch）

分支是 Git 最强大的功能——你可以在不影响主线的情况下开发新功能。

### 基本操作

```bash
git branch                    # 查看当前分支
git branch feature-login      # 创建新分支
git checkout feature-login    # 切换到新分支
git checkout -b feature-login # 创建并切换（上面两步合一）
```

### 合并分支

功能开发完了，把它合回主分支：

```bash
git checkout main             # 先切回主分支
git merge feature-login       # 把 feature-login 合并进来
```

### 删除分支

合并后分支就没用了，清理掉：

```bash
git branch -d feature-login
```

### 解决冲突

合并时如果两个人改了同一行代码，Git 会报冲突。打开文件，找到 `<<<<<<<` 标记，手动选择保留哪段代码，然后：

```bash
git add .
git commit -m "解决合并冲突"
```

---

## 第五步：远程协作

### 拉取最新代码

开始工作前先拉取别人的修改，避免冲突：

```bash
git pull
```

### 推送到远程

```bash
git push origin 分支名
```

### 查看远程仓库

```bash
git remote -v                 # 查看远程地址
git remote add origin url     # 添加远程仓库
```

---

## 第六步：常用实用命令

### 查看历史

```bash
git log                       # 查看提交历史
git log --oneline             # 精简模式，一行一个提交
git log --graph               # 图形化显示分支
```

### 撤销操作

```bash
# 撤销工作区的修改（还没 add）
git checkout -- 文件名

# 撤销暂存（add 了但没 commit）
git reset HEAD 文件名

# 回退到上一次提交（保留修改在工作区）
git reset --soft HEAD~1

# 回退到某次提交（丢弃所有修改，危险！）
git reset --hard 提交ID
```

### 暂存工作（做一半先放一边）

```bash
git stash                     # 暂存当前修改
git stash pop                 # 恢复暂存的修改
```

场景：你在开发新功能，突然要修一个 bug。先 `stash` 暂存，修完 bug，再 `stash pop` 恢复。

### 查看差异

```bash
git diff                      # 工作区 vs 暂存区
git diff --staged             # 暂存区 vs 仓库
```

---

## 第七步：.gitignore 文件

有些文件不该被 Git 跟踪（编译产物、密码文件、node_modules），用 `.gitignore` 排除。

```gitignore
# 依赖
node_modules/
__pycache__/

# 编译产物
*.class
*.o
build/

# 系统文件
.DS_Store
Thumbs.db

# 环境配置
.env
*.key
```

---

## 第八步：GitHub 协作流程

### Fork + Pull Request（给别人的项目贡献代码）

1. 在 GitHub 上 **Fork** 目标仓库（复制一份到你自己的账号下）
2. **Clone** 你自己的 Fork
3. 创建分支，开发功能
4. **Push** 到你的 Fork
5. 在 GitHub 上发起 **Pull Request**（请求原作者合并）
6. 等待审查和合并

### 团队协作常见流程

```
main          ← 稳定版本，不要直接改
  └── dev     ← 开发主线
       ├── feature-A  ← 张三做功能 A
       ├── feature-B  ← 李四做功能 B
       └── fix-bug    ← 王五修 bug
```

每人一个分支，做完合并回 `dev`，测试通过后合并到 `main` 发布。

---

## 总结

### 三句话讲清楚 Git

1. Git 是版本控制系统，记录代码每次修改的历史
2. 日常流程：`add` → `commit` → `push`，用分支隔离开发
3. 分支机制让多人协作互不干扰，合并时再整合

### 核心类比

> Git 像游戏的"存档系统"——每次修改存个档，搞砸了可以读档，多人各有存档线最后合并。

### 关键词

**Git、版本控制、分支、提交、仓库、GitHub、协作**
# Git

> Git 是版本控制工具，记录每次修改代码的历史，随时可以回退、分支协作。几乎所有程序员每天都在用它，不会 Git 等于不会团队合作。

## 先用一句生活话理解

Git 像游戏的"存档系统"——每打完一关存个档，打输了可以读档重来。多人玩的时候，每个人有自己的存档线，互不影响，最后合并。

没有 Git 的代码协作像多人同时编辑一个 Word 文档，互相覆盖、混乱不堪。有了 Git，每个人在自己的分支上开发，完成后合并。

## 什么是 Git？

想象你要写论文：
- **不用 Git**：论文_final_v1.doc、论文_final_v2.doc、论文_final_最终版.doc……
- **用 Git**：只有一个文件，每次修改自动记录，随时回到任何历史版本

**简单说**：Git 是**分布式版本控制系统**，记录代码的每一次变化，支持多人协作。

---

## Git 的设计哲学

Git 诞生于 2005 年，设计者是 Linus Torvalds（也是 Linux 之父）。核心思想：
- **分布式**：每个人本地都有完整的仓库副本
- **分支廉价**：创建和切换分支极快，鼓励频繁分支
- **快照存储**：保存的是每次的完整快照，不是差异

---

## 核心概念

### 1. 仓库（Repository）：项目的所有历史

**类比**：仓库像一本日记，记录了项目从第一天到现在的所有变化。

- **本地仓库**：你电脑上的完整副本
- **远程仓库**：服务器上的共享副本（如 GitHub）

---

### 2. 提交（Commit）：一次存档

**类比**：提交像游戏存档，记录这一刻代码的状态。

每次提交都有：
- 唯一 ID（哈希值）
- 提交信息（说明做了什么）
- 时间戳和作者

---

### 3. 分支（Branch）：平行世界

**类比**：分支像平行宇宙——主分支是"主线剧情"，你可以开一个分支去尝试新功能，搞砸了也不影响主线。

- `main`：主分支，稳定版本
- `feature/xxx`：功能分支，开发新功能
- 开发完成后"合并"回主分支

---

### 4. 工作区 → 暂存区 → 仓库

**类比**：像寄快递——写好包裹（工作区）→ 打包封箱（暂存区）→ 交给快递（提交到仓库）。

```bash
git add .          # 工作区 → 暂存区
git commit -m "说明"  # 暂存区 → 仓库
git push           # 本地仓库 → 远程仓库
```

---

### 5. 远程协作：拉取与推送

**类比**：像多人共同维护一本百科全书。

```bash
git pull           # 把别人的修改拉下来
git push           # 把自己的修改推上去
git clone          # 把远程仓库完整复制一份
```

---

## Git 的应用场景

| 场景 | 用什么 | 为什么 |
| :--- | :--- | :--- |
| **个人项目** | git init | 记录历史、随时回退 |
| **团队协作** | GitHub/GitLab | 多人分支协作 |
| **开源贡献** | Fork + PR | 给别人的项目提修改 |
| **代码审查** | Pull Request | 合并前审查代码 |
| **版本发布** | Tag | 标记 v1.0、v2.0 等版本 |

---

## 总结

### 三句话讲清楚 Git

1. Git 是版本控制系统，记录代码每次修改的历史
2. 分支机制让多人协作互不干扰，合并时再整合
3. 几乎所有开发团队的标配工具，不会 Git 无法团队合作

### 核心类比

> Git 像游戏的"存档系统"——每次修改存个档，搞砸了可以读档，多人各有存档线最后合并。

### 关键词

**Git、版本控制、分支、提交、仓库、GitHub、协作**
