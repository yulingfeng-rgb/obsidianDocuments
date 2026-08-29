# Git

> Git 是版本控制工具，记录每次修改代码的历史，随时可以回退、分支协作。几乎所有程序员每天都在用它，不会 Git 等于不会团队合作。

**关键词**：版本控制、分支、提交、协作、仓库

## 抽象类比

Git 像游戏的"存档系统"——每打完一关存个档，打输了可以读档重来。多人玩的时候，每个人有自己的存档线，互不影响，最后合并。

---

## 核心概念

### 仓库（Repository）：项目的档案柜

仓库是 Git 管理的目录，里面记录了项目的所有历史版本。像档案柜——每次提交都是一份档案，随时可以翻出来查看。

### 工作区、暂存区、仓库：三步走

- **工作区**：你正在编辑的文件，像厨房里正在切的菜
- **暂存区**（Staging Area）：`git add` 后进入的地方，像装盘待炒的菜——已经准备好，但还没下锅
- **仓库**：`git commit` 后存档的地方，像端上桌的菜——已经成品，记录在案

三步走：工作区 → `git add` → 暂存区 → `git commit` → 仓库。为什么要分三步？因为你想把相关的修改打包成一次提交，而不是每改一行就提交一次。暂存区让你挑选"这次提交包含哪些改动"。

### commit：存档点

每次 commit 都是项目的一个快照，记录"这一刻项目长什么样"。像游戏的存档点——打完一关存一个，死了可以读档回到这里。

commit 之间有父子关系，形成一条链。这就是提交历史。

### 分支（Branch）：平行宇宙

分支像平行宇宙——从某个时间点分叉，各自发展，互不影响。`main` 是主宇宙，`feature-login` 是开发登录功能的平行宇宙。

关键：Git 分支只是一个指针，指向某个 commit。创建分支只是新建一个指针，不复制任何文件。所以 Git 创建分支几乎是零成本的。

### merge：两条河流汇合

把两条分支合到一起，像两条河流汇合成一条。merge 会创建一个新的提交，把两个分支的修改合并起来。如果两边改了同一行，就会冲突，需要手动解决。

### rebase：重新串珠子

rebase 是另一种合并方式。把当前分支的提交"摘下来"，一个一个"重新串"到目标分支的末尾。像把一串珠子从旧绳子上取下来，串到新绳子上。

结果是一条直线的历史，比 merge 的分叉历史更整洁。但 rebase 会改写历史，已推送的分支不要 rebase。

---

## 设计哲学

### 为什么是分布式的？

早期的版本控制系统（如 SVN）是集中式的——有一台中央服务器，所有人都要连它才能提交。

问题：
- 中央服务器挂了，所有人都没法干活
- 没网就不能提交代码
- 中央服务器是单点故障，数据丢了就全完了

Git 的设计：**每个开发者本地都有完整的仓库**（完整历史、所有分支）。提交不需要网络，只在推送/拉取时才通信。

类比：集中式像所有人共用一个笔记本，一个人写字其他人只能等；分布式像每个人都有自己的笔记本，最后对一下笔记就行。

**分布式带来的好处**：
- 离线工作：飞机上、地铁里都能提交
- 快速操作：所有操作在本地，毫秒级
- 容灾：每个克隆都是完整备份，没有单点故障

### 为什么用快照而不是差异？

很多版本控制系统（如 SVN）记录的是"每次改了什么"——基于差异（delta）存储。

Git 记录的是"每次提交时项目长什么样"——基于快照（snapshot）存储。

为什么 Git 选快照？
- **快速分支**：分支只是指向某个快照的指针，不需要计算差异
- **快速回退**：直接跳到某个快照，不用回放差异
- **完整性校验**：每个快照都有 SHA-1 哈希，任何篡改都会被发现

类比：差异存储像记账本——每次记"今天花了多少"，要看余额得从头算；快照存储像每天拍张钱包照片——直接看照片就知道当时有多少钱。

### 关键取舍

- **安全 vs 灵活**：Git 允许改写历史（rebase、reset），灵活但有风险。Git 选择相信用户
- **存储空间 vs 速度**：快照比差异占空间，但 Git 用哈希树 + 压缩优化，速度优先
- **分布式 vs 一致性**：分布式没有"绝对正确"的版本，靠约定（如 main 分支）来协调

---

## 安装与配置

### 安装

- **Windows**：去 [git-scm.com](https://git-scm.com) 下载安装，一路默认即可
- **Linux**：`sudo apt install git`

### 配置身份

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

- `--global`：全局设置，只配一次
- `--local`：只对某个仓库生效
- `git config --list`：查看当前所有配置

### 验证

```bash
git --version
```

---

## 创建仓库

```bash
# 方式一：从零开始
mkdir my-project && cd my-project
git init

# 方式二：克隆远程仓库
git clone https://github.com/xxx/xxx.git
```

---

## 日常工作流（add → commit → push）

```bash
git status                   # 查看当前状态（养成习惯）
git add .                    # 暂存所有修改
git add 文件名               # 只暂存某个文件
git commit -m "说明改了什么"  # 提交到本地仓库
git push                     # 推送到远程仓库
```
 
### 写好 commit 信息

```bash
# 好的——一眼看懂改了什么
git commit -m "修复登录页面验证码不刷新的 bug"
git commit -m "新增用户头像上传功能"

# 差的
git commit -m "改了点东西"
git commit -m "update"
```

---

## 分支

```bash
git branch                    # 查看所有分支（* 标记当前分支）
git branch feature-login      # 创建新分支
git checkout feature-login    # 切换分支
git checkout -b feature-login # 创建并切换（上面两步合一）
git switch feature-login      # 切换分支（Git 2.23+ 新语法）
git switch -c feature-login   # 创建并切换（Git 2.23+ 新语法）
```

### 合并分支

```bash
git checkout main             # 先切回主分支
git merge feature-login       # 把 feature-login 合并进来
```

### 删除分支

```bash
git branch -d feature-login   # 删除已合并的分支
git branch -D feature-login   # 强制删除未合并的分支
```

### 解决冲突

合并时如果两人改了同一行代码，Git 会报冲突。打开文件，找到 `<<<<<<<` 标记，手动选择保留哪段代码，然后：

```bash
git add .
git commit -m "解决合并冲突"
```

---

## 远程协作

```bash
git remote -v                 # 查看远程仓库地址
git remote add origin url     # 添加远程仓库
git remote remove origin      # 删除远程仓库
git remote set-url origin url # 修改远程地址

git clone url                 # 克隆远程仓库
git pull                      # 拉取并合并（= fetch + merge）
git fetch                     # 只拉取，不合并（更安全）
git push origin 分支名         # 推送到远程指定分支
git push -u origin 分支名      # 首次推送并建立追踪关系
```

---

## 查看历史与差异

```bash
git log                       # 查看提交历史
git log --oneline             # 精简模式，一行一个提交
git log --graph               # 图形化显示分支
git log --oneline --graph --all  # 全分支图形历史
git log -n 5                  # 只看最近 5 条
git log --author="名字"       # 按作者筛选

git diff                      # 工作区 vs 暂存区
git diff --staged             # 暂存区 vs 仓库
git diff 分支A 分支B          # 两个分支的差异
```

---

## 撤销与回退

```bash
# 撤销工作区的修改（还没 add）
git checkout -- 文件名
git restore 文件名             # Git 2.23+ 新语法

# 撤销暂存（add 了但没 commit）
git reset HEAD 文件名
git restore --staged 文件名    # Git 2.23+ 新语法

# 回退到上一次提交（保留修改在工作区）
git reset --soft HEAD~1

# 回退到上一次提交（丢弃修改，危险！）
git reset --hard HEAD~1

# 回退到指定提交
git reset --soft 提交ID
git reset --hard 提交ID

# 创建一个"反向提交"来撤销（安全，不改历史）
git revert 提交ID
```

> **reset vs revert**：reset 改写历史（适合本地），revert 新增一次提交来撤销（适合已推送的）。

---

## 暂存工作（stash）

场景：你在开发新功能，突然要修一个 bug。先暂存当前修改，修完再恢复。

```bash
git stash                     # 暂存当前修改
git stash pop                 # 恢复暂存的修改（并删除 stash）
git stash apply               # 恢复暂存的修改（不删除 stash）
git stash list                # 查看所有暂存
git stash drop                # 删除最近一次暂存
```

---

## 标签（Tag）

用于标记版本发布点。

```bash
git tag                       # 查看所有标签
git tag v1.0                  # 打轻量标签
git tag -a v1.0 -m "第一版"   # 打带说明的标签
git push origin v1.0          # 推送标签到远程
git push origin --tags        # 推送所有标签
git tag -d v1.0               # 删除本地标签
```

---

## .gitignore

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

> 如果文件已被追踪，先 `git rm --cached 文件名` 再加入 .gitignore。

---

## GitHub 协作

### Fork + Pull Request（给别人的项目贡献代码）

1. 在 GitHub 上 **Fork** 目标仓库
2. **Clone** 你自己的 Fork
3. 创建分支，开发功能
4. **Push** 到你的 Fork
5. 在 GitHub 上发起 **Pull Request**
6. 等待审查和合并

### 团队分支模型

```
main          ← 稳定版本，不要直接改
  └── dev     ← 开发主线
       ├── feature-A  ← 张三做功能 A
       ├── feature-B  ← 李四做功能 B
       └── fix-bug    ← 王五修 bug
```

每人一个分支，做完合并回 `dev`，测试通过后合并到 `main` 发布。

---

## 面试速答

### Q: Git 的分支和 SVN 的分支有什么区别？
A: Git 分支是指针，指向某个 commit，创建分支只是创建一个指针，几乎不占空间。SVN 分支是完整目录拷贝，创建分支需要复制所有文件。所以 Git 鼓励频繁创建分支，SVN 则不敢。

### Q: git merge 和 git rebase 的区别？
A: merge 会创建一个新的合并提交，保留两条分支的历史，形成分叉。rebase 把当前分支的提交"移植"到目标分支末尾，形成一条直线的历史。rebase 历史更整洁，但会改写历史，已推送的分支不要 rebase。

### Q: git reset 和 git revert 的区别？
A: reset 是"回退"——把 HEAD 指针移到指定提交，会改写历史，适合本地未推送的提交。revert 是"反向提交"——新建一个提交来撤销之前的修改，不改写历史，适合已推送的提交。

### Q: git pull 和 git fetch 的区别？
A: fetch 只从远程拉取最新内容，不合并到本地分支，更安全。pull = fetch + merge，拉取并自动合并，更方便但可能产生冲突。

### Q: 为什么 Git 用快照而不是差异？
A: 快照让分支操作几乎是零成本（分支只是指针），回退快速（直接跳到快照），且每个快照有哈希值保证完整性。虽然比差异占空间，但 Git 用压缩优化，速度优先。
