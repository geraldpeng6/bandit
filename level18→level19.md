# Bandit Level 18 → Level 19 攻略 🧩

## 关卡目标 🎯

本关卡的目标是获取存储在主目录中`readme`文件中的密码。然而，有一个挑战：当你登录时，会立即被踢出（登出）。

## 所需知识 📚

### .bashrc文件修改 🔧

在这个关卡中，管理员修改了用户的`.bashrc`文件，使其在登录时立即执行`logout`命令，导致用户无法正常使用shell。

### SSH命令执行 🖥️

SSH不仅可以用于登录到远程服务器，还可以直接在远程服务器上执行命令，而无需进入交互式shell。这是绕过`.bashrc`文件中的`logout`命令的关键。

## 详细解题步骤 📝

### 1. 尝试正常登录（会失败）❌

如果我们尝试正常登录：

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

输入密码`kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`后，我们会看到类似以下的消息，然后立即被踢出：

```
Byebye !
Connection to bandit.labs.overthewire.org closed.
```

这是因为`.bashrc`文件被修改，在登录时立即执行了`logout`命令。

### 2. 使用SSH直接执行命令 🔍

我们可以使用SSH的命令执行功能，在登录时直接执行命令，而不进入交互式shell：

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat ~/readme"
```

输入密码后，SSH会在远程服务器上执行`cat ~/readme`命令，然后显示结果：

```
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```

这就是下一关的密码。

### 3. 使用其他方法（可选）🔄

除了使用`cat`命令外，我们还可以使用其他命令查看文件内容：

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "ls -la"
```

这会显示主目录中的所有文件，包括隐藏文件：

```
total 24
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r-----  1 bandit19 bandit18 3549 Oct 16  2018 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
-rw-r-----  1 bandit19 bandit18   33 Oct 16  2018 readme
```

然后我们可以查看`readme`文件的内容：

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

### 4. 使用其他Shell（可选）🐚

另一种方法是使用SSH的`-t`选项强制分配一个伪终端，并指定一个不同的shell：

```bash
ssh -t bandit18@bandit.labs.overthewire.org -p 2220 /bin/sh
```

这会使用`/bin/sh`而不是默认的shell（通常是`/bin/bash`），从而绕过`.bashrc`文件。

或者，我们可以直接启动一个bash shell，但指定不加载任何启动文件：

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "bash --noprofile --norc"
```

这会启动一个bash shell，但不加载`.profile`、`.bash_profile`或`.bashrc`文件。

## 命令解释 🔍

- `ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat ~/readme"`：
  - `ssh`：SSH客户端命令
  - `bandit18@bandit.labs.overthewire.org`：用户名和主机
  - `-p 2220`：指定SSH端口
  - `"cat ~/readme"`：要在远程服务器上执行的命令
- `ssh -t bandit18@bandit.labs.overthewire.org -p 2220 /bin/sh`：
  - `-t`：强制分配一个伪终端
  - `/bin/sh`：要在远程服务器上执行的shell

## 学习要点总结 📌

1. **SSH命令执行** - 学习了如何使用SSH直接在远程服务器上执行命令
2. **Shell启动文件** - 了解了`.bashrc`等shell启动文件的作用
3. **绕过限制** - 学习了如何绕过shell限制
4. **多种Shell** - 了解了不同类型的shell及其启动方式

## 提示 💡

- SSH的命令执行功能非常有用，可以用于执行简单的远程命令而无需完整的登录会话
- 在实际工作中，了解shell启动文件（如`.bashrc`、`.profile`）的作用非常重要
- 当遇到shell限制时，尝试使用不同的shell或启动选项可能会有所帮助
- 在安全环境中，应该谨慎修改shell启动文件，以避免类似本关卡中的问题

## 下一步 ⏭️

使用获得的密码登录到Level 19：

```bash
ssh bandit19@bandit.labs.overthewire.org -p 2220
```

密码：`IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x`

## 相关资源 🔗

- [SSH命令行选项详解](./resource/level18→level19/SSH命令行选项详解.md)
- [Bash启动文件详解](./resource/level18→level19/Bash启动文件详解.md)
- [Linux shell种类介绍](./resource/level18→level19/Linux_shell种类介绍.md)
