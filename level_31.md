# Bandit Level 30 → Level 31 攻略 🏷️

## 关卡目标 🎯

本关卡的目标是克隆一个Git仓库，该仓库位于`ssh://bandit30-git@localhost/home/bandit30-git/repo`，并从中找到下一关的密码。与前几关不同，这次密码可能不在分支上，而是在Git标签（tag）中。

## 所需知识 📚

### Git标签 🏷️

Git标签是指向特定提交的指针，通常用于标记发布版本或重要的里程碑。与分支不同，标签是固定的，不会随着新提交而移动。

### Git标签操作 🔍

我们需要了解如何列出所有标签、查看标签内容以及获取标签指向的提交。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit30@bandit.labs.overthewire.org -p 2220
```

密码：`5b90576bedb2cc04c86a9e924ce42faf`

### 2. 创建临时目录 📂

首先，我们需要创建一个临时目录来克隆Git仓库：

```bash
mkdir -p /tmp/bandit30_git
cd /tmp/bandit30_git
```

### 3. 克隆Git仓库 📦

现在我们可以使用`git clone`命令克隆仓库：

```bash
git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
```

当提示输入密码时，输入bandit30的密码：`5b90576bedb2cc04c86a9e924ce42faf`

### 4. 查看克隆的仓库 👀

克隆完成后，我们可以进入仓库目录并查看其内容：

```bash
cd repo
ls -la
```

输出结果应该类似于：

```
total 16
drwxr-sr-x 3 bandit30 root 4096 May 16 13:30 .
drwxr-sr-x 3 bandit30 root 4096 May 16 13:30 ..
drwxr-sr-x 8 bandit30 root 4096 May 16 13:30 .git
-rw-r--r-- 1 bandit30 root   30 May 16 13:30 README.md
```

我们可以看到仓库中有一个README.md文件。让我们查看它的内容：

```bash
cat README.md
```

输出结果可能类似于：

```
just an epmty file... muahaha
```

这个文件似乎没有提供有用的信息。我们需要尝试其他方法。

### 5. 查看Git日志 📜

我们可以使用`git log`命令查看仓库的提交历史：

```bash
git log
```

输出结果可能不包含有用的信息。

### 6. 查看分支 🔀

我们可以查看所有分支：

```bash
git branch -a
```

输出结果可能只显示主分支，没有其他分支。

### 7. 查看标签 🏷️

由于我们在分支和提交历史中没有找到有用的信息，我们可以尝试查看Git标签：

```bash
git tag
```

输出结果可能显示一个名为`secret`的标签：

```
secret
```

### 8. 查看标签内容 🔍

我们可以使用`git show`命令查看标签的内容：

```bash
git show secret
```

输出结果应该显示下一关的密码：

```
47e603bb428404d265f59c42920d81e5
```

## 命令解释 🔍

- `git tag`：
  - 列出所有标签
- `git show secret`：
  - `git show`：显示对象的内容
  - `secret`：要显示的标签名称

## 学习要点总结 📌

1. **Git标签基础** - 学习了如何使用`git tag`命令查看标签
2. **标签内容查看** - 了解了如何使用`git show`命令查看标签内容
3. **Git对象探索** - 学习了如何探索Git仓库中的不同类型的对象
4. **问题解决思路** - 了解了在一种方法不起作用时如何尝试其他方法

## 提示 💡

- Git标签通常用于标记发布版本或重要的里程碑
- 标签有两种类型：轻量标签（lightweight tag）和附注标签（annotated tag）
- 轻量标签只是指向特定提交的指针，而附注标签是存储在Git数据库中的完整对象
- 在实际开发中，标签通常用于标记软件的发布版本，如v1.0、v2.0等

## 下一步 ⏭️

使用获得的密码登录到Level 31：

```bash
ssh bandit31@bandit.labs.overthewire.org -p 2220
```

密码：`47e603bb428404d265f59c42920d81e5`

## 相关资源 🔗

- [Git标签基础](./resource/level_31/Git标签基础.md)
- [Git对象模型](./resource/level_31/Git对象模型.md)
- [Git标签最佳实践](./resource/level_31/Git标签最佳实践.md)
