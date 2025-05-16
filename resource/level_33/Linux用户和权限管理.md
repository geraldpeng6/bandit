# Linux用户和权限管理详解 🔐

## 什么是Linux用户和权限系统？ 🤔

Linux是一个多用户操作系统，它通过用户和权限系统来控制对系统资源的访问。这个系统确保每个用户只能访问他们有权限访问的文件和目录，保护系统和用户数据的安全。

理解Linux的用户和权限系统对于系统管理、安全维护和日常操作都至关重要。

## 用户和组 👥

### 用户类型

Linux系统中主要有三种类型的用户：

1. **超级用户（root）** 👑 - UID为0，拥有系统的完全控制权
2. **系统用户** 🖥️ - 用于运行服务和守护进程的非交互式用户
3. **普通用户** 👨‍💼 - 可以登录系统并执行有限权限操作的常规用户

### 用户相关文件

Linux系统使用几个关键文件来存储用户信息：

#### /etc/passwd

`/etc/passwd`文件存储用户账户信息，每行代表一个用户，格式如下：

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

#### /etc/shadow

`/etc/shadow`文件存储加密的用户密码和密码策略信息，格式如下：

```
用户名:加密密码:上次修改密码的时间:最小密码期限:最大密码期限:密码警告期:密码不活动期:账户过期时间:保留字段
```

这个文件只有root用户可以读取，提供了额外的安全层。

#### /etc/group

`/etc/group`文件存储组信息，格式如下：

```
组名:密码占位符:GID:组成员列表
```

例如：
```
developers:x:1002:alice,bob,charlie
```

### 用户管理命令

#### 创建用户

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

#### 修改用户

```bash
# 修改用户信息
sudo usermod 选项 用户名

# 示例：更改用户的shell
sudo usermod -s /bin/zsh 用户名

# 示例：将用户添加到组
sudo usermod -aG 组名 用户名
```

#### 删除用户

```bash
# 删除用户（保留主目录）
sudo userdel 用户名

# 删除用户及其主目录
sudo userdel -r 用户名
```

#### 设置密码

```bash
# 为用户设置密码
sudo passwd 用户名
```

### 组管理命令

#### 创建组

```bash
# 创建新组
sudo groupadd 组名
```

#### 修改组

```bash
# 修改组信息
sudo groupmod 选项 组名

# 示例：更改组名
sudo groupmod -n 新组名 旧组名
```

#### 删除组

```bash
# 删除组
sudo groupdel 组名
```

## 文件权限系统 📄

### 基本权限

Linux文件系统使用一个简单但强大的权限模型，每个文件和目录都有三组基本权限：

1. **所有者权限（User）** - 适用于文件/目录的所有者
2. **组权限（Group）** - 适用于文件/目录所属组的成员
3. **其他用户权限（Others）** - 适用于所有其他用户

每组权限包含三种基本权限类型：

1. **读取（r）** - 允许读取文件内容或列出目录内容
2. **写入（w）** - 允许修改文件或在目录中创建/删除文件
3. **执行（x）** - 允许执行文件或访问目录

### 查看文件权限

使用`ls -l`命令查看文件权限：

```bash
ls -l 文件名
```

输出示例：
```
-rwxr-xr-- 1 alice developers 2048 Jan 15 14:30 script.sh
```

权限字段`-rwxr-xr--`的含义：
- 第1个字符：文件类型（`-`表示普通文件，`d`表示目录）
- 第2-4个字符：所有者权限（`rwx`表示读、写、执行）
- 第5-7个字符：组权限（`r-x`表示读、执行，无写入权限）
- 第8-10个字符：其他用户权限（`r--`表示只读）

### 修改文件权限

#### chmod命令

`chmod`命令用于修改文件或目录的权限：

```bash
# 符号模式
chmod [u|g|o|a][+|-|=][r|w|x] 文件名

# 数字模式
chmod 权限数字 文件名
```

符号模式示例：
```bash
# 给所有者添加执行权限
chmod u+x script.sh

# 移除组的写入权限
chmod g-w script.sh

# 给所有用户设置读取权限
chmod a=r file.txt
```

数字模式中，每种权限对应一个数字：
- 读取（r）= 4
- 写入（w）= 2
- 执行（x）= 1

通过将这些数字相加，可以表示不同的权限组合：
```bash
# 设置权限为：所有者(rwx=7)，组(r-x=5)，其他用户(r--=4)
chmod 754 script.sh
```

#### chown命令

`chown`命令用于更改文件或目录的所有者和/或组：

```bash
# 更改所有者
sudo chown 用户名 文件名

# 更改所有者和组
sudo chown 用户名:组名 文件名

# 只更改组
sudo chown :组名 文件名
```

示例：
```bash
# 将文件所有权更改为alice
sudo chown alice script.sh

# 将文件所有权更改为alice，组更改为developers
sudo chown alice:developers script.sh
```

#### chgrp命令

`chgrp`命令用于更改文件或目录的组：

```bash
sudo chgrp 组名 文件名
```

### 特殊权限位

除了基本的读、写、执行权限外，Linux还提供了三种特殊权限位：

#### SUID (Set User ID)

当应用于可执行文件时，SUID权限允许用户以文件所有者的身份运行该程序。

```bash
# 设置SUID
chmod u+s 文件名
# 或使用数字模式
chmod 4755 文件名
```

SUID权限在`ls -l`输出中显示为`s`，替代所有者的执行位：
```
-rwsr-xr-x
```

#### SGID (Set Group ID)

当应用于可执行文件时，SGID权限允许用户以文件所属组的权限运行该程序。当应用于目录时，在该目录中创建的新文件会继承该目录的组所有权。

```bash
# 设置SGID
chmod g+s 文件或目录
# 或使用数字模式
chmod 2755 文件或目录
```

SGID权限在`ls -l`输出中显示为`s`，替代组的执行位：
```
-rwxr-sr-x
```

#### Sticky Bit

Sticky Bit主要用于目录。当目录设置了Sticky Bit，只有文件的所有者、目录的所有者或root用户才能删除或重命名该目录中的文件。

```bash
# 设置Sticky Bit
chmod +t 目录
# 或使用数字模式
chmod 1777 目录
```

Sticky Bit在`ls -l`输出中显示为`t`，替代其他用户的执行位：
```
drwxrwxrwt
```

## 访问控制列表（ACL） 📋

基本的Unix权限模型有时不够灵活。访问控制列表（ACL）提供了更细粒度的权限控制，允许为特定用户或组设置权限。

### 查看ACL

```bash
# 查看文件的ACL
getfacl 文件名
```

输出示例：
```
# file: 文件名
# owner: alice
# group: developers
user::rwx
user:bob:r--
group::r-x
mask::r-x
other::r--
```

### 设置ACL

```bash
# 为特定用户设置ACL
setfacl -m u:用户名:权限 文件名

# 为特定组设置ACL
setfacl -m g:组名:权限 文件名

# 设置默认ACL（应用于目录中新创建的文件）
setfacl -d -m u:用户名:权限 目录
```

示例：
```bash
# 给bob用户添加对文件的读写权限
setfacl -m u:bob:rw- 文件名

# 给developers组添加对目录的读写执行权限
setfacl -m g:developers:rwx 目录名
```

### 删除ACL

```bash
# 删除特定用户的ACL
setfacl -x u:用户名 文件名

# 删除特定组的ACL
setfacl -x g:组名 文件名

# 删除所有ACL
setfacl -b 文件名
```

## 实际应用场景 🛠️

### 1. 设置共享目录

创建一个多用户共享目录，所有用户都可以读写，但只有所有者可以删除文件：

```bash
# 创建目录
mkdir /shared

# 设置所有权
sudo chown alice:users /shared

# 设置权限（rwxrwx--t）
sudo chmod 1770 /shared
```

### 2. 设置安全的Web服务器目录

为Web服务器设置适当的权限：

```bash
# 设置Web根目录的所有权
sudo chown -R www-data:www-data /var/www/html

# 设置目录权限
sudo find /var/www/html -type d -exec chmod 755 {} \;

# 设置文件权限
sudo find /var/www/html -type f -exec chmod 644 {} \;
```

### 3. 创建具有受限权限的用户

创建一个只能访问特定目录的用户：

```bash
# 创建用户
sudo useradd -m -s /bin/bash restricted_user

# 创建专用目录
sudo mkdir /data/restricted

# 设置所有权
sudo chown restricted_user:restricted_user /data/restricted

# 设置权限
sudo chmod 700 /data/restricted
```

## 安全最佳实践 🛡️

1. **最小权限原则** - 只给用户和程序必要的最小权限
2. **定期审核权限** - 定期检查文件和目录的权限设置
3. **使用组管理权限** - 通过组而不是个别用户管理权限
4. **保护系统文件** - 确保系统文件有适当的权限
5. **限制SUID/SGID程序** - 最小化具有SUID/SGID权限的程序数量
6. **使用sudo而非root登录** - 使用sudo执行特权命令，而不是直接以root身份登录

## 常见问题解答 ❓

### 问题：如何查找具有特定权限的文件？

**解决方案**：使用`find`命令：

```bash
# 查找SUID文件
find / -perm -4000 -type f 2>/dev/null

# 查找所有人都可写的文件
find / -perm -o+w -type f 2>/dev/null
```

### 问题：如何递归更改目录权限？

**解决方案**：使用`chmod`的`-R`选项：

```bash
# 递归更改目录及其内容的权限
chmod -R 755 目录

# 只更改目录权限，不更改文件权限
find 目录 -type d -exec chmod 755 {} \;
find 目录 -type f -exec chmod 644 {} \;
```

### 问题：如何防止用户修改自己的密码？

**解决方案**：锁定用户账户：

```bash
# 锁定用户账户
sudo passwd -l 用户名

# 解锁用户账户
sudo passwd -u 用户名
```

## 练习题 🎯

1. 创建一个名为`project`的目录，设置权限使所有者有完全控制权，组成员可以读取和执行，其他用户无权访问。
2. 创建一个脚本文件，设置权限使其只能由所有者执行，并使用SUID使其以root权限运行。
3. 创建一个共享目录，所有用户都可以在其中创建文件，但只有文件的创建者可以删除自己的文件。
4. 使用ACL为特定用户提供对文件的读取权限，而不更改基本权限。
5. 编写一个脚本，查找系统中所有具有SUID权限的文件并列出它们。

## 小贴士 💡

- 使用`umask`命令设置新创建文件和目录的默认权限
- 记住数字权限：4(读)+2(写)+1(执行)
- 使用`id`命令查看用户的UID、GID和组成员资格
- 在更改重要系统文件的权限前先备份
- 使用`sudo -l`查看当前用户可以执行的sudo命令
- 对于临时权限需求，考虑使用`sudo`而不是永久更改权限
- 使用`find`命令定期审核文件权限

通过掌握Linux的用户和权限管理，你可以更好地保护系统安全，控制资源访问，并为多用户环境创建适当的权限结构。这些知识对于系统管理员、安全专业人员和高级Linux用户都是必不可少的。
