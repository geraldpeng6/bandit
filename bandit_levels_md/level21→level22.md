# Bandit Level 21 → Level 22 攻略 ⏱️

## 关卡目标 🎯

本关卡的目标是找出系统中的一个定时任务（cron job），该任务会以bandit22用户的身份执行，并通过分析该任务来获取下一关的密码。

## 所需知识 📚

### Cron定时任务 ⏱️

Cron是Linux系统中的一个定时任务调度程序，它允许用户在指定的时间或时间间隔执行命令或脚本。Cron任务通常存储在`/etc/cron.d/`目录中，或者通过`crontab`命令进行管理。

### 系统日志和配置文件 📋

Linux系统中的日志和配置文件通常存储在`/var/log/`和`/etc/`目录中。了解如何查看和分析这些文件对于系统管理和故障排除非常重要。

### Shell脚本 📜

Shell脚本是一系列命令的集合，可以自动执行重复性任务。在Linux中，shell脚本通常使用bash、sh或其他shell解释器执行。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit21@bandit.labs.overthewire.org -p 2220
```

密码：`gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr`

### 2. 查看cron任务 👀

首先，我们需要查看系统中的cron任务。cron任务通常存储在`/etc/cron.d/`目录中：

```bash
ls -la /etc/cron.d/
```

输出结果应该类似于：

```
total 24
drwxr-xr-x  2 root root 4096 Jul 11  2020 .
drwxr-xr-x 87 root root 4096 May 14  2020 ..
-rw-r--r--  1 root root  102 Oct  7  2017 .placeholder
-rw-r--r--  1 root root  189 Oct 16  2018 atop
-rw-r--r--  1 root root  120 Oct 16  2018 cronjob_bandit22
-rw-r--r--  1 root root  122 Oct 16  2018 cronjob_bandit23
-rw-r--r--  1 root root  120 Oct 16  2018 cronjob_bandit24
```

我们看到有几个与bandit用户相关的cron任务，包括`cronjob_bandit22`、`cronjob_bandit23`和`cronjob_bandit24`。由于我们正在寻找bandit22的密码，我们应该查看`cronjob_bandit22`文件：

```bash
cat /etc/cron.d/cronjob_bandit22
```

输出结果应该类似于：

```
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

这表明系统会在每次重启时以及每分钟以bandit22用户的身份执行`/usr/bin/cronjob_bandit22.sh`脚本，并将所有输出重定向到`/dev/null`（即丢弃所有输出）。

### 3. 查看cron脚本 📜

现在我们需要查看`/usr/bin/cronjob_bandit22.sh`脚本的内容：

```bash
cat /usr/bin/cronjob_bandit22.sh
```

输出结果应该类似于：

```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

这个脚本做了两件事：
1. 将`/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`文件的权限设置为644（所有者可读写，组和其他用户只读）
2. 将bandit22的密码写入该文件

### 4. 查看密码文件 🔑

现在我们知道bandit22的密码存储在`/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`文件中，我们可以直接查看该文件：

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

输出结果应该显示bandit22的密码：

```
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
```

## 命令解释 🔍

- `ls -la /etc/cron.d/`：
  - `ls`：列出目录内容
  - `-l`：使用长格式显示
  - `-a`：显示所有文件，包括隐藏文件
  - `/etc/cron.d/`：要列出内容的目录
- `cat /etc/cron.d/cronjob_bandit22`：
  - `cat`：显示文件内容
  - `/etc/cron.d/cronjob_bandit22`：要显示内容的文件
- `cat /usr/bin/cronjob_bandit22.sh`：
  - `cat`：显示文件内容
  - `/usr/bin/cronjob_bandit22.sh`：要显示内容的文件
- `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`：
  - `cat`：显示文件内容
  - `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`：要显示内容的文件

## 学习要点总结 📌

1. **Cron定时任务** - 学习了cron定时任务的基本概念和配置
2. **系统文件结构** - 了解了Linux系统中重要的目录和文件
3. **Shell脚本分析** - 学习了如何分析shell脚本的功能
4. **文件权限** - 复习了文件权限的概念和表示方法

## 提示 💡

- Cron任务是Linux系统中自动化任务的重要工具
- `/tmp`目录是存储临时文件的地方，通常所有用户都有读写权限
- 在实际系统中，应该谨慎设置cron任务的权限，避免安全风险
- 使用`crontab -l`命令可以查看当前用户的cron任务
- Cron任务的格式为：`分 时 日 月 周 命令`，其中`*`表示"每"

## 下一步 ⏭️

使用获得的密码登录到Level 22：

```bash
ssh bandit22@bandit.labs.overthewire.org -p 2220
```

密码：`Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI`

## 相关资源 🔗

- [Linux Cron定时任务详解](./resource/level21→level22/Linux_Cron定时任务详解.md)
- [Linux文件权限详解](./resource/level21→level22/Linux文件权限详解.md)
- [Bash脚本编程指南](./resource/level21→level22/Bash脚本编程指南.md)
