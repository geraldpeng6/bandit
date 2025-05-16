# Bandit Level 22 → Level 23 攻略 🔄

## 关卡目标 🎯

本关卡的目标是分析一个以bandit23用户身份运行的cron任务，理解其功能，并利用这些信息获取下一关的密码。

## 所需知识 📚

### Cron定时任务 ⏱️

与上一关类似，我们需要了解cron定时任务的基本概念和配置。

### Shell脚本分析 📜

我们需要分析shell脚本的功能，理解其执行逻辑。

### Bash变量和命令替换 🔄

Bash脚本中的变量使用`$`符号引用，命令替换使用`` `command` ``或`$(command)`语法。

### 哈希函数 🔐

哈希函数是将任意长度的输入转换为固定长度输出的函数。在Linux中，常用的哈希命令包括`md5sum`、`sha1sum`、`sha256sum`等。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit22@bandit.labs.overthewire.org -p 2220
```

密码：`Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI`

### 2. 查看cron任务 👀

首先，我们需要查看与bandit23相关的cron任务：

```bash
cat /etc/cron.d/cronjob_bandit23
```

输出结果应该类似于：

```
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```

这表明系统会在每次重启时以及每分钟以bandit23用户的身份执行`/usr/bin/cronjob_bandit23.sh`脚本。

### 3. 查看cron脚本 📜

现在我们需要查看`/usr/bin/cronjob_bandit23.sh`脚本的内容：

```bash
cat /usr/bin/cronjob_bandit23.sh
```

输出结果应该类似于：

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

这个脚本做了以下几件事：
1. 使用`whoami`命令获取当前用户名，并将结果存储在`myname`变量中
2. 构造一个字符串`I am user $myname`，计算其MD5哈希值，并提取哈希值的第一部分，将结果存储在`mytarget`变量中
3. 将当前用户的密码文件复制到`/tmp/$mytarget`文件中

### 4. 模拟脚本执行 🔄

当脚本以bandit23用户身份运行时，`myname`变量的值将是"bandit23"。我们可以手动计算`mytarget`变量的值：

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

输出结果应该是一个MD5哈希值：

```
8ca319486bfbbc3663ea0fbe81326349
```

### 5. 查看密码文件 🔑

现在我们知道bandit23的密码存储在`/tmp/8ca319486bfbbc3663ea0fbe81326349`文件中，我们可以直接查看该文件：

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

输出结果应该显示bandit23的密码：

```
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```

## 命令解释 🔍

- `cat /etc/cron.d/cronjob_bandit23`：
  - `cat`：显示文件内容
  - `/etc/cron.d/cronjob_bandit23`：要显示内容的文件
- `cat /usr/bin/cronjob_bandit23.sh`：
  - `cat`：显示文件内容
  - `/usr/bin/cronjob_bandit23.sh`：要显示内容的文件
- `echo I am user bandit23 | md5sum | cut -d ' ' -f 1`：
  - `echo I am user bandit23`：输出字符串"I am user bandit23"
  - `|`：管道操作符，将前一个命令的输出作为后一个命令的输入
  - `md5sum`：计算MD5哈希值
  - `cut -d ' ' -f 1`：使用空格作为分隔符，提取第一个字段（MD5哈希值）
- `cat /tmp/8ca319486bfbbc3663ea0fbe81326349`：
  - `cat`：显示文件内容
  - `/tmp/8ca319486bfbbc3663ea0fbe81326349`：要显示内容的文件

## 学习要点总结 📌

1. **Shell脚本分析** - 学习了如何分析和理解shell脚本的执行逻辑
2. **Bash变量** - 了解了Bash变量的使用方法
3. **命令替换** - 学习了如何使用命令替换获取命令的输出
4. **管道和过滤** - 复习了使用管道组合多个命令和使用过滤工具提取特定信息

## 提示 💡

- 在分析脚本时，可以逐行理解其功能，必要时可以手动执行部分命令验证理解
- MD5是一种常用的哈希算法，但在安全应用中已不再推荐使用，因为它存在碰撞风险
- 在实际系统中，应该避免将敏感信息（如密码）存储在公共可访问的位置（如`/tmp`目录）
- 使用`echo`和管道组合可以快速测试命令的输出，这是一种常用的调试技巧

## 下一步 ⏭️

使用获得的密码登录到Level 23：

```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
```

密码：`jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n`

## 相关资源 🔗

- [Bash变量详解](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameters.html)
- [Linux命令替换](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html)
- [MD5哈希算法](https://en.wikipedia.org/wiki/MD5)
- [Linux管道和过滤命令](https://www.geeksforgeeks.org/piping-in-unix-or-linux/)
