# Bandit Level 26 → Level 27 攻略 👑

## 关卡目标 🎯

本关卡的目标是使用位于主目录中的setuid二进制文件`bandit27-do`，以bandit27用户的身份执行命令，从而读取`/etc/bandit_pass/bandit27`文件获取下一关的密码。

## 所需知识 📚

### Vi编辑器和Shell 📝

由于我们在上一关中使用了特殊方法登录到bandit26，我们需要继续使用Vi编辑器来执行命令。

### setuid权限 👑

与之前的关卡类似，setuid（Set User ID）是一种特殊的文件权限，允许用户以文件所有者的身份执行该文件。

### 命令执行 🖥️

我们需要了解如何在受限环境中执行命令。

## 详细解题步骤 📝

### 1. 登录到bandit26 🔐

首先，我们需要按照上一关的方法登录到bandit26。确保将终端窗口调整得非常小，然后使用SSH密钥登录：

```bash
ssh -i bandit26.sshkey bandit26@localhost
```

当`more`命令暂停显示`text.txt`文件内容时，按`v`键进入Vi编辑器。

### 2. 在Vi中获取Shell 🐚

在Vi编辑器中，我们可以设置shell并启动它：

```
:set shell=/bin/bash
:shell
```

这会启动一个bash shell，使我们能够以bandit26用户的身份执行命令。

### 3. 查看当前目录中的文件 👀

现在我们可以查看当前目录中的文件：

```bash
ls -la
```

输出结果应该类似于：

```
total 36
drwxr-xr-x  3 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rwsr-x---  1 bandit27 bandit26 7296 Oct 16  2018 bandit27-do
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .ssh
-rw-r-----  1 bandit26 bandit26  258 Oct 16  2018 text.txt
```

注意`bandit27-do`文件的权限：`-rwsr-x---`。`s`表示这是一个setuid程序，它会以其所有者（bandit27）的身份运行。

### 4. 尝试执行setuid程序 🔍

我们可以尝试直接执行`bandit27-do`程序，看看它的用法：

```bash
./bandit27-do
```

输出结果可能类似于：

```
Run a command as another user.
  Example: ./bandit27-do id
```

这表明我们需要提供一个命令作为参数，该命令将以bandit27用户的身份执行。

### 5. 使用setuid程序查看当前用户 🧪

我们可以使用`id`命令验证setuid程序是否真的以bandit27用户的身份运行：

```bash
./bandit27-do id
```

输出结果应该显示bandit27的用户ID：

```
uid=11027(bandit27) gid=11026(bandit26) groups=11026(bandit26)
```

这确认了`bandit27-do`程序确实以bandit27用户的身份运行。

### 6. 使用setuid程序读取密码文件 🔑

现在我们可以使用`bandit27-do`程序以bandit27用户的身份读取`/etc/bandit_pass/bandit27`文件：

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

输出结果应该显示bandit27的密码：

```
3ba3118a22e93127a4ed485be72ef5ea
```

## 命令解释 🔍

- `:set shell=/bin/bash`：
  - `:`：在Vi中进入命令模式
  - `set shell=/bin/bash`：设置shell为`/bin/bash`
- `:shell`：
  - `:`：在Vi中进入命令模式
  - `shell`：启动一个shell
- `./bandit27-do cat /etc/bandit_pass/bandit27`：
  - `./bandit27-do`：执行当前目录中的`bandit27-do`程序
  - `cat /etc/bandit_pass/bandit27`：要以bandit27用户身份执行的命令，用于读取密码文件

## 学习要点总结 📌

1. **Vi编辑器命令** - 复习了如何在Vi编辑器中执行命令和启动shell
2. **setuid权限** - 复习了setuid权限的概念和用途
3. **特权命令执行** - 学习了如何在受限环境中使用setuid程序执行特权命令
4. **文件权限识别** - 复习了如何识别文件的setuid权限

## 提示 💡

- 在受限环境中，setuid程序是获取特权的常用方法
- Vi/Vim是功能强大的文本编辑器，在系统管理和安全测试中非常有用
- 在实际系统中，应该限制setuid程序的数量和功能，以减少潜在的安全风险
- 了解如何在不同的环境和条件下获取shell是一项重要的技能

## 下一步 ⏭️

使用获得的密码登录到Level 27：

```bash
ssh bandit27@bandit.labs.overthewire.org -p 2220
```

密码：`3ba3118a22e93127a4ed485be72ef5ea`

## 相关资源 🔗

- [Vi/Vim编辑器命令详解](https://www.tutorialspoint.com/vim/vim_commands.htm)
- [Linux文件权限详解](https://www.linux.com/training-tutorials/understanding-linux-file-permissions/)
- [setuid、setgid和sticky bit详解](https://www.geeksforgeeks.org/setuid-setgid-and-sticky-bits-in-linux-file-permissions/)
