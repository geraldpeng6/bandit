# Bandit Level 29 → Level 30 攻略 🔀

## 关卡目标 🎯

本关卡的目标是克隆一个Git仓库，该仓库位于`ssh://bandit29-git@localhost/home/bandit29-git/repo`，并从中找到下一关的密码。与前两关不同，这次密码可能不在主分支上，我们需要查看其他分支。

## 所需知识 📚

### Git分支 🔀

Git分支是指向提交对象的可变指针。主分支通常称为`master`或`main`，但仓库可以有多个分支，用于开发不同的功能或版本。

### Git分支操作 🔄

我们需要了解如何列出所有分支、切换分支以及查看分支内容。

### Git远程分支 🌐

远程分支是对远程仓库中分支的引用。我们需要了解如何查看和访问远程分支。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit29@bandit.labs.overthewire.org -p 2220
```

密码：`bbc96594b4e001778eee9975372716b2`

### 2. 创建临时目录 📂

首先，我们需要创建一个临时目录来克隆Git仓库：

```bash
mkdir -p /tmp/bandit29_git
cd /tmp/bandit29_git
```

### 3. 克隆Git仓库 📦

现在我们可以使用`git clone`命令克隆仓库：

```bash
git clone ssh://bandit29-git@localhost/home/bandit29-git/repo
```

当提示输入密码时，输入bandit29的密码：`bbc96594b4e001778eee9975372716b2`

### 4. 查看克隆的仓库 👀

克隆完成后，我们可以进入仓库目录并查看其内容：

```bash
cd repo
ls -la
```

输出结果应该类似于：

```
total 16
drwxr-sr-x 3 bandit29 root 4096 May 16 13:20 .
drwxr-sr-x 3 bandit29 root 4096 May 16 13:20 ..
drwxr-sr-x 8 bandit29 root 4096 May 16 13:20 .git
-rw-r--r-- 1 bandit29 root  131 May 16 13:20 README.md
```

我们可以看到仓库中有一个README.md文件。让我们查看它的内容：

```bash
cat README.md
```

输出结果可能类似于：

```
# Bandit Notes
Some notes for level30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```

我们可以看到密码没有直接显示，而是提示"no passwords in production!"（生产环境中没有密码！）。这表明密码可能在其他分支上，例如开发分支。

### 5. 查看所有分支 🔀

我们可以使用`git branch`命令查看本地分支：

```bash
git branch
```

输出结果应该只显示主分支：

```
* master
```

但是，我们还需要查看远程分支：

```bash
git branch -a
```

输出结果应该显示所有分支，包括远程分支：

```
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

我们可以看到除了主分支外，还有`dev`和`sploits-dev`等远程分支。

### 6. 切换到开发分支 🔄

由于提示说"no passwords in production!"，我们可以尝试切换到开发分支（`dev`）：

```bash
git checkout dev
```

输出结果应该类似于：

```
Branch dev set up to track remote branch dev from origin.
Switched to a new branch 'dev'
```

### 7. 查看开发分支的内容 👀

现在我们可以查看开发分支上的README.md文件：

```bash
cat README.md
```

输出结果应该显示下一关的密码：

```
# Bandit Notes
Some notes for level30 of bandit.

## credentials

- username: bandit30
- password: 5b90576bedb2cc04c86a9e924ce42faf
```

在开发分支上，密码是可见的。

## 命令解释 🔍

- `git branch`：
  - 列出本地分支
- `git branch -a`：
  - `-a`：列出所有分支，包括远程分支
- `git checkout dev`：
  - `git checkout`：切换分支
  - `dev`：目标分支

## 学习要点总结 📌

1. **Git分支基础** - 学习了如何使用`git branch`命令查看分支
2. **远程分支查看** - 了解了如何使用`git branch -a`命令查看远程分支
3. **分支切换** - 学习了如何使用`git checkout`命令切换分支
4. **分支内容查看** - 了解了如何查看不同分支上的文件内容

## 提示 💡

- Git分支是Git的核心功能之一，用于并行开发不同的功能或版本
- 在实际开发中，通常会有多个分支，如`master`/`main`（主分支）、`dev`（开发分支）、`feature/*`（功能分支）等
- 远程分支通常以`remotes/origin/`为前缀，表示它们来自远程仓库
- 使用`git checkout`命令可以切换分支，也可以用于检出特定的提交或文件版本

## 下一步 ⏭️

使用获得的密码登录到Level 30：

```bash
ssh bandit30@bandit.labs.overthewire.org -p 2220
```

密码：`5b90576bedb2cc04c86a9e924ce42faf`

## 相关资源 🔗

- [Git分支基础](https://git-scm.com/book/zh/v2/Git-分支-分支简介)
- [Git远程分支](https://git-scm.com/book/zh/v2/Git-分支-远程分支)
- [Git分支管理](https://git-scm.com/book/zh/v2/Git-分支-分支管理)
