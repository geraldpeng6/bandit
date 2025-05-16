# Bandit Level 19 → Level 20 攻略 👑

## 关卡目标 🎯

本关卡的目标是使用位于主目录中的setuid二进制文件`bandit20-do`，以bandit20用户的身份执行命令，从而读取`/etc/bandit_pass/bandit20`文件获取下一关的密码。

## 所需知识 📚

### setuid权限 👑

setuid（Set User ID）是一种特殊的文件权限，允许用户以文件所有者的身份执行该文件。当一个可执行文件设置了setuid权限时，它会以其所有者的权限运行，而不是以执行它的用户的权限运行。

在文件权限中，setuid权限通常显示为`s`，位于所有者的执行权限位置，例如：`-rwsr-xr-x`。

### 文件权限查看 👀

可以使用`ls -l`命令查看文件的详细权限信息。

### 命令执行 🖥️

setuid程序通常允许以特定的方式执行命令，例如以另一个用户的身份执行命令。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit19@bandit.labs.overthewire.org -p 2220
```

密码：`IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x`

### 2. 查看当前目录中的文件 👀

登录后，使用`ls`命令查看当前目录中的文件：

```bash
ls -la
```

输出结果应该类似于：

```
total 28
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rwsr-x---  1 bandit20 bandit19 7296 Oct 16  2018 bandit20-do
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
```

注意`bandit20-do`文件的权限：`-rwsr-x---`。`s`表示这是一个setuid程序，它会以其所有者（bandit20）的身份运行。

### 3. 尝试执行setuid程序 🔍

我们可以尝试直接执行`bandit20-do`程序，看看它的用法：

```bash
./bandit20-do
```

输出结果可能类似于：

```
Run a command as another user.
  Example: ./bandit20-do id
```

这表明我们需要提供一个命令作为参数，该命令将以bandit20用户的身份执行。

### 4. 使用setuid程序查看当前用户 🧪

我们可以使用`id`命令验证setuid程序是否真的以bandit20用户的身份运行：

```bash
./bandit20-do id
```

输出结果应该显示bandit20的用户ID：

```
uid=11020(bandit20) gid=11019(bandit19) groups=11019(bandit19)
```

这确认了`bandit20-do`程序确实以bandit20用户的身份运行。

### 5. 使用setuid程序读取密码文件 🔑

现在我们可以使用`bandit20-do`程序以bandit20用户的身份读取`/etc/bandit_pass/bandit20`文件：

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

输出结果应该显示bandit20的密码：

```
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```

## 命令解释 🔍

- `ls -la`：
  - `ls`：列出目录内容
  - `-l`：使用长格式显示
  - `-a`：显示所有文件，包括隐藏文件
- `./bandit20-do cat /etc/bandit_pass/bandit20`：
  - `./bandit20-do`：执行当前目录中的`bandit20-do`程序
  - `cat /etc/bandit_pass/bandit20`：要以bandit20用户身份执行的命令，用于读取密码文件

## 学习要点总结 📌

1. **setuid权限** - 学习了setuid权限的概念和用途
2. **权限识别** - 了解了如何识别文件的setuid权限
3. **特权命令执行** - 学习了如何使用setuid程序以其他用户的身份执行命令
4. **密码文件访问** - 了解了Linux系统中密码文件的存储位置和访问控制

## 提示 💡

- setuid是一种强大但潜在危险的机制，在实际系统中应谨慎使用
- 在Linux系统中，只有root用户可以更改文件的所有者
- setuid程序通常用于允许普通用户执行需要特权的特定任务
- 在安全环境中，应该限制setuid程序的数量和功能，以减少潜在的安全风险

## 下一步 ⏭️

使用获得的密码登录到Level 20：

```bash
ssh bandit20@bandit.labs.overthewire.org -p 2220
```

密码：`GbKksEFF4yrVs6il55v6gwY5aVje5f0j`

## 相关资源 🔗

- [Linux文件权限详解](./resource/level_20/Linux文件权限详解.md)
- [setuid、setgid和sticky bit详解](./resource/level_20/setuid_setgid和sticky_bit详解.md)
- [Linux安全最佳实践](./resource/level_20/Linux安全最佳实践.md)
