# Bandit Level 32 → Level 33 攻略 🔠

## 关卡目标 🎯

本关卡的目标是绕过一个将所有输入转换为大写的特殊shell（UPPERCASE SHELL），以获取对普通shell的访问，然后找到下一关的密码。

## 所需知识 📚

### Shell变量和特殊变量 🔤

在shell中，有一些特殊变量，如`$0`、`$1`、`$2`等。`$0`表示当前shell或脚本的名称。

### Shell转义和绕过 🔄

我们需要了解如何绕过shell的限制，特别是当shell对输入进行处理时。

### 环境变量 🌐

环境变量是在shell会话中定义的变量，可以被shell和其启动的程序访问。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```

密码：`56a9bf19c63d650ce78e6ec0354ee45e`

### 2. 探索UPPERCASE SHELL 🔍

登录后，我们会看到一个欢迎消息，表明我们正在使用UPPERCASE SHELL：

```
WELCOME TO THE UPPERCASE SHELL
>>
```

在这个shell中，我们输入的所有命令都会被转换为大写，然后执行。例如，如果我们输入`ls`，它会被转换为`LS`，这不是一个有效的命令：

```
>> ls
sh: 1: LS: not found
```

### 3. 尝试使用特殊变量 🔤

在shell中，`$0`是一个特殊变量，表示当前shell或脚本的名称。由于这是一个变量，而不是我们输入的命令，它可能不会被转换为大写：

```
>> $0
```

如果成功，这应该会启动一个新的shell，绕过UPPERCASE SHELL的限制：

```
$
```

现在我们有了一个普通的shell提示符，表明我们已经成功绕过了UPPERCASE SHELL。

### 4. 验证当前用户 🧪

我们可以使用`whoami`命令验证我们当前的用户身份：

```bash
$ whoami
```

输出结果应该显示我们是bandit33用户：

```
bandit33
```

这表明我们已经成功升级到bandit33用户。

### 5. 查找密码 🔑

现在我们可以查看bandit33用户的密码文件：

```bash
$ cat /etc/bandit_pass/bandit33
```

输出结果应该显示bandit33的密码：

```
c9c3199ddf4121b10cf581a98d51caee
```

### 6. 探索bandit33的主目录（可选）🏠

我们也可以查看bandit33用户的主目录，看看是否有其他有趣的文件：

```bash
$ ls -la /home/bandit33
```

输出结果可能显示一个README.txt文件：

```
total 24
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
-rw-------  1 bandit33 bandit33  430 Oct 16  2018 README.txt
```

我们可以查看README.txt文件的内容：

```bash
$ cat /home/bandit33/README.txt
```

输出结果可能包含关于游戏结束的信息：

```
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep checking for an updated version of this game!

If you have ideas for any new levels, please let us know!
```

这表明我们已经完成了所有关卡！

## 命令解释 🔍

- `$0`：
  - 特殊变量，表示当前shell或脚本的名称
- `whoami`：
  - 显示当前用户的用户名
- `cat /etc/bandit_pass/bandit33`：
  - `cat`：显示文件内容
  - `/etc/bandit_pass/bandit33`：bandit33用户的密码文件

## 学习要点总结 📌

1. **Shell特殊变量** - 学习了shell中的特殊变量，如`$0`
2. **Shell绕过技巧** - 了解了如何绕过shell的限制
3. **权限和用户切换** - 学习了如何在不同用户之间切换
4. **文件权限和访问** - 复习了文件权限和访问控制

## 提示 💡

- Shell中的特殊变量（如`$0`、`$1`、`$2`等）在安全上下文中需要特别注意
- 在设计安全系统时，应该考虑所有可能的绕过方法
- 在实际系统中，应该限制用户对敏感文件的访问
- 了解shell的工作原理对于系统管理和安全测试非常重要

## 下一步 ⏭️

恭喜你完成了所有Bandit关卡！你可以尝试其他OverTheWire游戏，如Natas、Leviathan等。

## 相关资源 🔗

- [Bash特殊变量](./resource/level32→level33/Bash特殊变量.md)
- [Linux用户和权限管理](./resource/level32→level33/Linux用户和权限管理.md)
- [Shell脚本安全最佳实践](./resource/level32→level33/Shell脚本安全最佳实践.md)
