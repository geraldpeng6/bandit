# Bandit Level 25 → Level 26 攻略 🔤

## 关卡目标 🎯

本关卡的目标是使用提供的SSH密钥登录到bandit26，但有一个挑战：bandit26用户使用的不是标准shell，而是一个特殊的shell，会在登录后立即退出。我们需要找到一种方法绕过这个限制，成功登录到bandit26。

## 所需知识 📚

### SSH密钥认证 🔐

与之前的关卡类似，我们需要使用SSH密钥进行认证。

### Shell和登录过程 🐚

在Linux中，用户的默认shell定义在`/etc/passwd`文件中。当用户登录时，系统会启动该shell。

### 文本查看器和编辑器 📄

我们需要了解文本查看器（如`more`、`less`）和编辑器（如`vim`、`nano`）的基本用法。

### 终端大小和分页 📏

文本查看器的行为会受到终端窗口大小的影响，特别是在显示大文件时。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit25@bandit.labs.overthewire.org -p 2220
```

密码：`uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG`

### 2. 查看SSH密钥 🔑

登录后，我们可以查看当前目录中的文件：

```bash
ls -la
```

输出结果应该显示有一个SSH私钥文件：

```
total 32
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rw-r-----  1 bandit25 bandit25 1679 Oct 16  2018 bandit26.sshkey
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
```

我们可以查看SSH私钥文件的内容：

```bash
cat bandit26.sshkey
```

这个文件包含了用于登录bandit26的SSH私钥。

### 3. 查看bandit26用户的shell 🔍

在尝试登录之前，我们应该查看bandit26用户使用的shell：

```bash
cat /etc/passwd | grep bandit26
```

输出结果应该类似于：

```
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

这表明bandit26用户使用的shell是`/usr/bin/showtext`，而不是标准的`/bin/bash`。我们需要查看这个自定义shell的内容：

```bash
cat /usr/bin/showtext
```

输出结果应该类似于：

```bash
#!/bin/sh
export TERM=linux
more ~/text.txt
exit 0
```

这个脚本做了三件事：
1. 设置终端类型为`linux`
2. 使用`more`命令显示`~/text.txt`文件的内容
3. 执行`exit 0`命令退出shell

现在我们知道为什么登录bandit26后会立即退出：因为shell执行了`exit 0`命令。但是，在退出之前，它会使用`more`命令显示一个文件。这给了我们一个机会。

### 4. 利用more命令的特性 📏

`more`命令是一个分页器，当文件内容超过一屏时，它会暂停并等待用户输入。如果我们能让`text.txt`文件的内容超过一屏，`more`命令就会暂停，这样我们就可以在shell退出之前与之交互。

我们可以通过调整终端窗口的大小来实现这一点。具体来说，我们需要将终端窗口调整得非常小（只有几行高），这样`text.txt`文件的内容就会超过一屏。

### 5. 尝试登录bandit26 🔄

现在我们可以尝试登录bandit26，但在登录之前，先将终端窗口调整得非常小：

```bash
ssh -i bandit26.sshkey bandit26@localhost
```

如果终端窗口足够小，`more`命令会暂停并显示类似于以下的内容：

```
  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
```

此时，`more`命令处于暂停状态，等待用户输入。我们可以按`v`键进入`vi`编辑器。

### 6. 使用vi编辑器 ✏️

在`vi`编辑器中，我们可以执行命令来读取文件或启动shell。首先，我们可以尝试读取bandit26的密码：

```
:e /etc/bandit_pass/bandit26
```

这会显示bandit26的密码：

```
5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z
```

但我们的目标是获得一个交互式shell。我们可以在`vi`中执行shell命令：

```
:set shell=/bin/bash
:shell
```

这会启动一个bash shell，使我们能够以bandit26用户的身份执行命令。

### 7. 验证身份 🧪

现在我们可以验证我们确实以bandit26用户的身份登录：

```bash
whoami
```

输出结果应该显示：

```
bandit26
```

## 命令解释 🔍

- `cat /etc/passwd | grep bandit26`：
  - `cat /etc/passwd`：显示`/etc/passwd`文件的内容
  - `|`：管道操作符，将前一个命令的输出作为后一个命令的输入
  - `grep bandit26`：搜索包含"bandit26"的行
- `ssh -i bandit26.sshkey bandit26@localhost`：
  - `ssh`：SSH客户端命令
  - `-i bandit26.sshkey`：指定要使用的私钥文件
  - `bandit26@localhost`：以bandit26用户身份登录到本地主机
- `:e /etc/bandit_pass/bandit26`：
  - `:`：在vi中进入命令模式
  - `e /etc/bandit_pass/bandit26`：编辑指定的文件
- `:set shell=/bin/bash`：
  - `:`：在vi中进入命令模式
  - `set shell=/bin/bash`：设置shell为`/bin/bash`
- `:shell`：
  - `:`：在vi中进入命令模式
  - `shell`：启动一个shell

## 学习要点总结 📌

1. **自定义Shell** - 学习了如何识别和分析自定义shell
2. **文本查看器特性** - 了解了`more`命令的特性和行为
3. **终端大小影响** - 学习了终端窗口大小如何影响程序行为
4. **Vi编辑器命令** - 了解了如何在vi编辑器中执行命令和启动shell

## 提示 💡

- 在实际系统中，可以通过修改`/etc/passwd`文件来更改用户的默认shell
- `more`、`less`等分页器的行为会受到终端窗口大小和`TERM`环境变量的影响
- Vi/Vim是功能强大的文本编辑器，可以执行各种命令，包括读取文件和启动shell
- 在安全环境中，应该谨慎配置用户的shell，避免使用可能被绕过的限制性shell

## 下一步 ⏭️

现在我们已经成功登录到bandit26，可以继续进行下一关的挑战。

## 相关资源 🔗

- [Linux用户和shell管理](./resource/level_26/Linux用户和shell管理.md)
- [Vi/Vim编辑器命令详解](./resource/level_26/Vi_Vim编辑器命令详解.md)
- [Linux终端和环境变量](./resource/level_26/Linux终端和环境变量.md)
