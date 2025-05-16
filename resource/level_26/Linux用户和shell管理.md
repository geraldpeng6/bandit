# Linux用户和shell管理 👥

## 什么是用户和用户组？ 🤔

在Linux系统中，用户(User)是可以登录系统并执行操作的实体，而用户组(Group)则是用户的集合，用于简化权限管理。每个用户都有一个唯一的用户ID(UID)和一个主要用户组ID(GID)。理解用户和用户组的概念对于系统管理和安全至关重要。

## Linux用户类型 👤

Linux系统中主要有三种类型的用户：

1. **超级用户/根用户(root)** 👑 - UID为0，拥有系统的完全控制权
2. **系统用户** 🖥️ - 用于运行服务和守护进程的非交互式用户
3. **普通用户** 👨‍💼 - 可以登录系统并执行有限权限操作的常规用户

## 用户相关的重要文件 📄

### /etc/passwd 文件

`/etc/passwd`文件存储了系统中所有用户的基本信息，每行代表一个用户，格式如下：

```
用户名:密码占位符:UID:GID:注释:主目录:默认shell
```

例如：
```
alice:x:1001:1001:Alice Smith:/home/alice:/bin/bash
```

这行表示：
- 用户名：alice
- 密码占位符：x（实际密码存储在/etc/shadow中）
- UID：1001
- GID：1001
- 注释：Alice Smith
- 主目录：/home/alice
- 默认shell：/bin/bash

### /etc/shadow 文件

`/etc/shadow`文件存储用户的加密密码和密码策略信息，格式如下：

```
用户名:加密密码:上次修改密码的时间:最小密码期限:最大密码期限:密码警告期:密码不活动期:账户过期时间:保留字段
```

### /etc/group 文件

`/etc/group`文件存储组信息，格式如下：

```
组名:密码占位符:GID:组成员列表
```

例如：
```
developers:x:1002:alice,bob,charlie
```

## 用户管理命令 🛠️

### 创建用户

```bash
# 创建新用户
sudo useradd 用户名

# 创建用户并设置主目录、shell等
sudo useradd -m -d /home/用户名 -s /bin/bash -c "全名" 用户名
```

参数说明：
- `-m` - 创建用户的主目录
- `-d` - 指定主目录路径
- `-s` - 指定默认shell
- `-c` - 添加注释（通常是用户全名）

### 设置密码

```bash
# 为用户设置密码
sudo passwd 用户名
```

### 修改用户信息

```bash
# 修改用户信息
sudo usermod 选项 用户名

# 示例：更改用户的shell
sudo usermod -s /bin/zsh 用户名

# 示例：将用户添加到组
sudo usermod -aG 组名 用户名
```

常用选项：
- `-l` - 更改用户名
- `-s` - 更改默认shell
- `-d` - 更改主目录
- `-aG` - 将用户添加到附加组

### 删除用户

```bash
# 删除用户（保留主目录）
sudo userdel 用户名

# 删除用户及其主目录
sudo userdel -r 用户名
```

## 用户组管理命令 👥

### 创建用户组

```bash
# 创建新用户组
sudo groupadd 组名
```

### 修改用户组

```bash
# 修改用户组信息
sudo groupmod 选项 组名

# 示例：更改组名
sudo groupmod -n 新组名 旧组名
```

### 删除用户组

```bash
# 删除用户组
sudo groupdel 组名
```

## 查看用户和组信息 🔍

```bash
# 查看当前用户
whoami

# 查看当前用户的组
groups

# 查看特定用户的组
groups 用户名

# 查看用户信息
id 用户名

# 查看登录用户
who

# 查看所有登录用户及其活动
w
```

## 什么是Shell？ 🐚

Shell是用户与Linux系统交互的接口，它接收用户输入的命令并执行。Linux系统中有多种shell，最常见的是Bash(Bourne Again SHell)，但还有其他类型如Zsh、Fish、Ksh等。

## 常见的Shell类型 🔄

1. **Bash (Bourne Again Shell)** - 大多数Linux发行版的默认shell
2. **Zsh (Z Shell)** - 功能强大，是macOS Catalina及以上版本的默认shell
3. **Fish (Friendly Interactive Shell)** - 用户友好，具有智能自动补全功能
4. **Ksh (Korn Shell)** - 兼容Bourne Shell，增加了一些编程功能
5. **Dash** - 轻量级shell，通常用于脚本执行
6. **Tcsh** - C Shell的增强版本

## 查看和更改默认Shell 🔄

### 查看当前Shell

```bash
# 查看当前使用的shell
echo $SHELL

# 查看系统中可用的shell
cat /etc/shells
```

### 更改用户的默认Shell

```bash
# 更改自己的默认shell
chsh -s /bin/zsh

# 更改其他用户的默认shell（需要root权限）
sudo chsh -s /bin/zsh 用户名
```

## 限制性Shell和特殊Shell 🔒

有时，出于安全考虑，管理员可能会为某些用户设置限制性shell，限制他们可以执行的命令。

### 常见的限制性Shell

1. **rbash (Restricted Bash)** - Bash的受限版本
2. **rksh (Restricted Korn Shell)** - Korn Shell的受限版本
3. **lshell** - 提供有限命令集的shell
4. **自定义shell脚本** - 管理员可以创建自定义脚本作为用户的shell

### 设置限制性Shell

```bash
# 将用户的shell设置为受限bash
sudo usermod -s /bin/rbash 用户名
```

### 自定义Shell脚本示例

以下是一个简单的自定义shell脚本示例，它只允许用户执行特定的命令：

```bash
#!/bin/bash
# 自定义限制性shell

# 允许的命令列表
allowed_commands=("ls" "pwd" "echo" "exit")

while true; do
    echo -n "限制性shell> "
    read cmd args
    
    # 检查命令是否在允许列表中
    allowed=0
    for allowed_cmd in "${allowed_commands[@]}"; do
        if [ "$cmd" = "$allowed_cmd" ]; then
            allowed=1
            break
        fi
    done
    
    if [ $allowed -eq 1 ]; then
        # 执行允许的命令
        $cmd $args
    elif [ "$cmd" = "exit" ]; then
        # 允许退出
        exit 0
    else
        echo "错误：命令 '$cmd' 不允许使用"
    fi
done
```

将此脚本保存为可执行文件（例如`/usr/local/bin/restrictedshell`），然后将用户的默认shell设置为此脚本：

```bash
sudo usermod -s /usr/local/bin/restrictedshell 用户名
```

## 绕过限制性Shell的方法（仅用于教育目的）🔓

了解限制性shell的潜在绕过方法对于系统管理员评估安全性很重要：

1. **利用允许的命令执行其他命令**
   ```bash
   # 如果允许使用less/more/vi等命令
   less /etc/passwd
   # 在less中按 ! 可以执行shell命令
   !bash
   ```

2. **利用编程语言解释器**
   ```bash
   # 如果允许使用python/perl等
   python -c 'import os; os.system("/bin/bash")'
   ```

3. **利用环境变量**
   ```bash
   # 如果PATH可修改
   export PATH=/bin:/usr/bin:$PATH
   ```

## 安全最佳实践 🛡️

1. **最小权限原则** - 只给用户必要的权限
2. **定期审核用户账户** - 删除不再需要的账户
3. **强密码策略** - 使用强密码并定期更改
4. **限制root访问** - 使用sudo而不是直接使用root账户
5. **监控用户活动** - 使用日志和审计工具
6. **正确配置限制性shell** - 确保没有简单的绕过方法

## 用户管理实用技巧 💡

### 批量创建用户

```bash
# 使用脚本批量创建用户
for 用户名 in user1 user2 user3; do
    sudo useradd -m -s /bin/bash $用户名
    echo "用户 $用户名 已创建"
done
```

### 临时禁用账户

```bash
# 锁定用户账户
sudo passwd -l 用户名

# 解锁用户账户
sudo passwd -u 用户名
```

### 设置账户过期时间

```bash
# 设置账户在特定日期过期
sudo usermod -e 2023-12-31 用户名

# 设置密码在90天后过期
sudo chage -M 90 用户名
```

### 查看密码策略

```bash
# 查看用户的密码策略
sudo chage -l 用户名
```

## 练习题 🎯

1. 创建一个名为"student"的新用户，设置其主目录为"/home/student"，默认shell为bash。
2. 创建一个名为"developers"的新用户组，并将"student"用户添加到该组。
3. 为"student"用户设置一个限制性shell，只允许执行ls、pwd和echo命令。
4. 查看系统中所有UID大于1000的普通用户。
5. 设置"student"用户的密码在30天后过期，并要求用户在首次登录时更改密码。

## 小贴士 💡

- 使用`sudo`执行需要特权的命令，而不是切换到root用户
- 创建新用户时，使用`-m`选项自动创建主目录
- 使用`visudo`命令编辑sudoers文件，而不是直接编辑
- 定期备份`/etc/passwd`、`/etc/shadow`和`/etc/group`文件
- 使用`getent`命令查询用户和组信息，它会考虑所有配置的名称服务

通过掌握Linux用户和shell管理，你可以更好地控制系统访问权限，提高系统安全性，并为不同用户提供适合其需求的工作环境。这些知识对于系统管理员和安全专业人员来说是必不可少的基础技能。
