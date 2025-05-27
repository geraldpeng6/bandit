# Bandit Level 28 → Level 29 攻略 🔄

## 关卡目标 🎯

本关卡的目标是克隆一个Git仓库，该仓库位于`ssh://bandit28-git@localhost/home/bandit28-git/repo`，并从中找到下一关的密码。与上一关不同，这次密码可能不会直接显示，我们需要查看Git的历史记录。

## 所需知识 📚

### Git基础 📦

与上一关类似，我们需要了解如何克隆Git仓库。

### Git历史和日志 📜

我们需要了解如何查看Git仓库的提交历史和日志，以找到可能被修改或删除的信息。

### Git差异比较 🔄

我们需要了解如何比较不同提交之间的差异，以找到被修改的内容。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit28@bandit.labs.overthewire.org -p 2220
```

密码：`0ef186ac70e04ea33b4c1853d2526fa2`

### 2. 创建临时目录 📂

首先，我们需要创建一个临时目录来克隆Git仓库：

```bash
mkdir -p /tmp/bandit28_git
cd /tmp/bandit28_git
```

### 3. 克隆Git仓库 📦

现在我们可以使用`git clone`命令克隆仓库：

```bash
git clone ssh://bandit28-git@localhost/home/bandit28-git/repo
```

当提示输入密码时，输入bandit28的密码：`0ef186ac70e04ea33b4c1853d2526fa2`

### 4. 查看克隆的仓库 👀

克隆完成后，我们可以进入仓库目录并查看其内容：

```bash
cd repo
ls -la
```

输出结果应该类似于：

```
total 16
drwxr-sr-x 3 bandit28 root 4096 May 16 13:10 .
drwxr-sr-x 3 bandit28 root 4096 May 16 13:10 ..
drwxr-sr-x 8 bandit28 root 4096 May 16 13:10 .git
-rw-r--r-- 1 bandit28 root  111 May 16 13:10 README.md
```

我们可以看到仓库中有一个README.md文件。让我们查看它的内容：

```bash
cat README.md
```

输出结果可能类似于：

```
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```

我们可以看到密码被隐藏了。这表明我们需要查看Git的历史记录，看看密码是否在之前的提交中可见。

### 5. 查看Git日志 📜

我们可以使用`git log`命令查看仓库的提交历史：

```bash
git log
```

输出结果应该类似于：

```
commit edd935d60906b33f0619605abd1689808ccdd5ee
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    fix info leak

commit c086d11a00c0648d095d04c089786efef5e01264
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    add missing data

commit de2ebe2d5fd1598cd547f4d56247e053be3fdc38
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    initial commit of README.md
```

我们可以看到有三个提交，其中最新的提交消息是"fix info leak"（修复信息泄漏）。这表明可能在之前的提交中泄漏了一些信息，然后在最新的提交中被修复（隐藏）了。

### 6. 查看提交差异 🔄

我们可以使用`git show`命令查看特定提交的详细信息，包括更改的内容：

```bash
git show edd935d60906b33f0619605abd1689808ccdd5ee
```

输出结果应该类似于：

```
commit edd935d60906b33f0619605abd1689808ccdd5ee
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    fix info leak

diff --git a/README.md b/README.md
index 3f7cee8..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: bbc96594b4e001778eee9975372716b2
+- password: xxxxxxxxxx
```

在这个差异中，我们可以看到密码从`bbc96594b4e001778eee9975372716b2`被修改为`xxxxxxxxxx`。这就是我们要找的下一关的密码。

## 命令解释 🔍

- `git log`：
  - 显示提交日志
- `git show edd935d60906b33f0619605abd1689808ccdd5ee`：
  - `git show`：显示提交的详细信息
  - `edd935d60906b33f0619605abd1689808ccdd5ee`：要显示的提交的哈希值

## 学习要点总结 📌

1. **Git历史查看** - 学习了如何使用`git log`命令查看提交历史
2. **提交详情查看** - 了解了如何使用`git show`命令查看提交的详细信息
3. **差异分析** - 学习了如何分析提交之间的差异
4. **信息恢复** - 了解了如何从Git历史中恢复被删除或修改的信息

## 提示 💡

- Git保存了完整的历史记录，即使信息被删除或修改，也可以通过查看历史记录找回
- 在实际工作中，如果不小心提交了敏感信息（如密码、密钥），简单地在后续提交中删除是不够的，因为信息仍然存在于历史记录中
- 对于真正需要从Git历史中删除的敏感信息，应该使用`git filter-branch`或BFG Repo-Cleaner等工具
- 养成良好的习惯，不要将敏感信息直接提交到版本控制系统中

## 下一步 ⏭️

使用获得的密码登录到Level 29：

```bash
ssh bandit29@bandit.labs.overthewire.org -p 2220
```

密码：`bbc96594b4e001778eee9975372716b2`

## 相关资源 🔗

- [Git日志和历史](./resource/level28→level29/Git日志和历史.md)
- [Git差异比较](./resource/level28→level29/Git差异比较.md)
- [从Git历史中删除敏感数据](./resource/level28→level29/从Git历史中删除敏感数据.md)
