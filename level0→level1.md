# Bandit Level 0 → Level 1 攻略 🔍

## 关卡目标 🎯

本关卡的目标是找到存储在主目录中名为`readme`的文件，并读取其中的内容以获取下一关的密码。

## 所需知识 📚

### Linux文件系统基础 📂

在Linux中，文件和目录组织成一个树状结构。每个用户都有一个"主目录"（home directory），这是用户登录后的默认位置。

### 基本Linux命令 💻

本关卡需要了解以下基本命令：

- `ls`（list，列出）：列出目录中的文件和子目录
- `cd`（change directory，改变目录）：切换当前工作目录
- `cat`（concatenate，连接）：显示文件内容
- `file`：确定文件类型
- `du`（disk usage，磁盘使用）：估计文件空间使用量
- `find`：在目录层次结构中搜索文件

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的信息登录服务器：

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

密码：`bandit0`

### 2. 查看当前目录中的文件 👀

登录后，我们首先需要查看当前目录中有哪些文件。使用`ls`命令：

```bash
ls
```

输出结果应该显示有一个名为`readme`的文件：

```
readme
```

### 3. 读取readme文件内容 📄

使用`cat`命令查看文件内容：

```bash
cat readme
```

输出结果将显示下一关的密码：

```
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

这个字符串就是进入Level 1的密码！

## 命令解释 🔍

- `ls`：列出当前目录中的文件和目录，不带任何选项时以简单格式显示
- `cat readme`：显示名为"readme"的文件内容

## 学习要点总结 📌

1. **文件查看基础** - 学习了如何使用`ls`命令列出目录内容
2. **文件内容读取** - 学习了如何使用`cat`命令查看文件内容
3. **Linux导航** - 了解了Linux文件系统的基本导航方法

## 提示 💡

- 养成记录每一关密码的习惯，可以在本地创建一个文本文件来保存它们
- 密码不会自动保存，如果你没有自己记录，下次需要重新从Level 0开始
- 密码可能会定期更改，建议记录解决每个挑战的方法

## 下一步 ⏭️

使用获得的密码登录到Level 1：

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

密码：`boJ9jbbUNNfktd78OOpsqOltutMc3MY1`

## 相关资源 🔗

- [Linux ls命令基础教程](./resource/level0→level1/linux_ls命令基础教程.md)
- [Linux cat命令基础教程](./resource/level0→level1/linux_cat命令基础教程.md)
- [Linux文件系统层次结构标准](./resource/level0→level1/linux文件系统层次结构标准.md)
