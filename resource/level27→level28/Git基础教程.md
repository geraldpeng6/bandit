# Git基础教程 📦

## 什么是Git？ 🤔

Git是一个分布式版本控制系统，由Linux之父Linus Torvalds于2005年创建，用于管理Linux内核的开发。Git允许多人协作开发项目，跟踪文件的变化，并在需要时恢复到之前的版本。与集中式版本控制系统不同，Git让每个开发者都拥有完整的代码仓库副本，可以在本地进行大多数操作，不需要持续连接到中央服务器。

## 为什么使用Git？ 🌟

1. **版本控制** - 跟踪文件的变化历史，随时可以回到之前的版本
2. **协作开发** - 多人可以同时处理同一项目的不同部分
3. **分支管理** - 可以创建独立的工作空间（分支），不影响主代码
4. **代码审查** - 便于审查他人的代码变更
5. **备份** - 代码存储在多个地方，降低数据丢失风险

## Git的基本概念 📚

### 仓库（Repository）

仓库是Git用来存储项目文件和元数据的地方。有两种类型的仓库：
- **本地仓库** - 存储在你的计算机上
- **远程仓库** - 存储在服务器上（如GitHub、GitLab等）

### 工作区、暂存区和版本库

Git项目有三个主要区域：
- **工作区（Working Directory）** - 你实际编辑文件的地方
- **暂存区（Staging Area）** - 准备提交的变更的临时存储区
- **版本库（Repository）** - 提交后的文件存储区

### 提交（Commit）

提交是Git中的基本操作单位，它记录了特定时间点的项目状态。每个提交都有：
- 唯一的标识符（SHA-1哈希值）
- 提交消息（描述变更内容）
- 作者信息
- 时间戳
- 指向父提交的引用

### 分支（Branch）

分支是独立的开发线，允许你在不影响主代码的情况下进行开发。主分支通常称为`master`或`main`。

## 安装Git 💿

### Windows

1. 从[Git官网](https://git-scm.com/download/win)下载安装程序
2. 运行安装程序，按照向导完成安装
3. 安装完成后，可以使用Git Bash或命令提示符运行Git命令

### macOS

使用Homebrew安装：
```bash
brew install git
```

或从[Git官网](https://git-scm.com/download/mac)下载安装程序

### Linux (Ubuntu/Debian)

```bash
sudo apt-get update
sudo apt-get install git
```

### Linux (CentOS/Fedora)

```bash
sudo yum install git
```

## 基本配置 ⚙️

安装Git后，首先需要设置用户信息：

```bash
# 设置用户名
git config --global user.name "你的名字"

# 设置邮箱
git config --global user.email "你的邮箱@example.com"

# 查看配置
git config --list
```

## 基本操作 🛠️

### 创建仓库

```bash
# 在当前目录初始化新仓库
git init

# 克隆现有仓库
git clone 仓库URL
```

### 添加和提交文件

```bash
# 查看文件状态
git status

# 添加文件到暂存区
git add 文件名
git add .  # 添加所有变更

# 提交变更
git commit -m "提交说明"

# 查看提交历史
git log
```

### 分支操作

```bash
# 查看分支
git branch

# 创建新分支
git branch 分支名

# 切换分支
git checkout 分支名
# 或使用新命令（Git 2.23+）
git switch 分支名

# 创建并切换到新分支
git checkout -b 分支名
# 或使用新命令
git switch -c 分支名

# 合并分支
git merge 分支名

# 删除分支
git branch -d 分支名
```

### 远程仓库操作

```bash
# 添加远程仓库
git remote add origin 仓库URL

# 查看远程仓库
git remote -v

# 从远程仓库获取更新
git fetch origin

# 从远程仓库拉取更新并合并
git pull origin 分支名

# 推送到远程仓库
git push origin 分支名
```

## 常用Git工作流 🔄

### 基本工作流

1. 克隆仓库或初始化新仓库
2. 创建新分支进行开发
3. 修改文件
4. 添加和提交变更
5. 推送到远程仓库
6. 创建合并请求（Pull Request）
7. 代码审查后合并到主分支

### 示例：克隆并提交更改

```bash
# 克隆仓库
git clone https://github.com/用户名/仓库名.git

# 进入仓库目录
cd 仓库名

# 创建新分支
git checkout -b feature-x

# 修改文件...

# 添加变更
git add .

# 提交变更
git commit -m "添加功能X"

# 推送到远程仓库
git push origin feature-x
```

## 高级Git功能 🚀

### 暂存工作（Stashing）

当你需要切换分支但不想提交当前工作时：

```bash
# 暂存当前工作
git stash

# 查看暂存列表
git stash list

# 应用最近的暂存
git stash apply

# 应用并删除最近的暂存
git stash pop

# 删除暂存
git stash drop
```

### 撤销变更

```bash
# 撤销工作区的修改
git checkout -- 文件名

# 撤销暂存区的修改
git reset HEAD 文件名

# 撤销提交（创建新提交）
git revert 提交ID

# 修改最近的提交
git commit --amend
```

### 重写历史

```bash
# 交互式变基
git rebase -i HEAD~3  # 重写最近3个提交

# 压缩提交
git rebase -i HEAD~3  # 然后将pick改为squash

# 重置分支指针
git reset --soft HEAD~1  # 保留工作区和暂存区变更
git reset --mixed HEAD~1  # 保留工作区变更（默认）
git reset --hard HEAD~1  # 丢弃所有变更
```

### 标签

```bash
# 创建轻量标签
git tag v1.0.0

# 创建附注标签
git tag -a v1.0.0 -m "版本1.0.0发布"

# 查看标签
git tag

# 推送标签到远程
git push origin v1.0.0
git push origin --tags  # 推送所有标签
```

## Git与SSH 🔐

### 为什么使用SSH

使用SSH密钥进行Git认证比使用密码更安全、更方便，尤其是在频繁与远程仓库交互时。

### 生成SSH密钥

```bash
# 生成新的SSH密钥对
ssh-keygen -t rsa -b 4096 -C "你的邮箱@example.com"

# 查看公钥
cat ~/.ssh/id_rsa.pub
```

### 添加SSH密钥到Git服务

1. 复制公钥内容
2. 在Git服务（如GitHub、GitLab）的设置中添加SSH密钥
3. 测试连接：`ssh -T git@github.com`

## Git配置技巧 💡

### 常用别名

```bash
# 设置别名
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### 忽略文件

创建`.gitignore`文件，列出不需要跟踪的文件和目录：

```
# 编译输出
*.o
*.exe
*.dll

# 日志文件
*.log

# 临时文件
*.tmp
*.swp

# 目录
node_modules/
dist/
build/
```

## 常见问题解答 ❓

### 问题：如何解决合并冲突？

当Git无法自动合并变更时，会产生冲突。解决步骤：

1. 运行`git status`查看冲突文件
2. 打开冲突文件，寻找`<<<<<<<`、`=======`和`>>>>>>>`标记
3. 编辑文件解决冲突
4. 添加解决后的文件：`git add 文件名`
5. 完成合并：`git commit`

### 问题：如何撤销已推送的提交？

```bash
# 创建新提交撤销变更（推荐）
git revert 提交ID

# 强制推送（谨慎使用）
git reset --hard 提交ID
git push -f origin 分支名
```

### 问题：如何清理仓库中的大文件？

```bash
# 查找大文件
git rev-list --objects --all | grep "$(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -10 | awk '{print $1}')"

# 从历史中移除文件
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch 大文件路径' --prune-empty --tag-name-filter cat -- --all

# 清理和回收空间
git reflog expire --expire=now --all
git gc --prune=now
```

## 实用Git命令 🔧

### 查看特定文件的历史

```bash
# 查看文件的变更历史
git log --follow -p 文件名

# 查看谁修改了文件的每一行
git blame 文件名
```

### 查找引入Bug的提交

```bash
# 二分查找
git bisect start
git bisect bad  # 当前版本有问题
git bisect good 早期提交ID  # 这个版本正常
# Git会检出中间版本，测试后标记为good或bad
# 重复直到找到第一个有问题的提交
git bisect reset  # 完成后重置
```

### 保存临时变更

```bash
# 创建临时提交
git commit -m "WIP: 临时保存"

# 或使用stash
git stash save "临时保存的工作"
```

## 练习题 🎯

1. 创建一个新的Git仓库，添加一个README.md文件，并提交。
2. 创建一个新分支，修改README.md文件，然后合并回主分支。
3. 模拟合并冲突并解决它：在两个不同的分支中修改同一文件的同一行。
4. 使用`git log`查看提交历史，并找出特定提交的详细信息。
5. 创建一个`.gitignore`文件，配置忽略特定类型的文件。

## 小贴士 💡

- 经常提交小的、相关的变更，而不是一次提交大量修改
- 编写清晰、描述性的提交消息
- 使用分支进行功能开发和实验
- 在推送前先拉取最新变更
- 定期备份你的Git仓库
- 学习使用图形化Git客户端（如GitKraken、SourceTree）可以帮助理解Git操作

通过掌握Git的基础知识，你将能够更有效地管理代码，与团队协作，并保持项目的版本历史。随着经验的积累，你可以探索更多高级功能，进一步提高工作效率。
