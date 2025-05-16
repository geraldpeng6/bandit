# Bandit Level 27 → Level 28 攻略 📦

## 关卡目标 🎯

本关卡的目标是克隆一个Git仓库，该仓库位于`ssh://bandit27-git@localhost/home/bandit27-git/repo`，并从中找到下一关的密码。

## 所需知识 📚

### Git基础 📦

Git是一个分布式版本控制系统，用于跟踪文件的变化。在这个关卡中，我们需要了解如何克隆Git仓库。

### SSH认证 🔐

我们需要使用SSH协议克隆Git仓库，这涉及到SSH认证。

### 临时目录使用 📂

由于我们可能没有在主目录中创建文件的权限，我们需要使用临时目录来克隆仓库。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit27@bandit.labs.overthewire.org -p 2220
```

密码：`3ba3118a22e93127a4ed485be72ef5ea`

### 2. 创建临时目录 📂

首先，我们需要创建一个临时目录来克隆Git仓库：

```bash
mkdir -p /tmp/bandit27_git
cd /tmp/bandit27_git
```

### 3. 克隆Git仓库 📦

现在我们可以使用`git clone`命令克隆仓库。由于仓库需要认证，我们需要提供用户名和密码。根据题目描述，用户名是`bandit27-git`，密码与当前级别（bandit27）的密码相同：

```bash
git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
```

当提示输入密码时，输入bandit27的密码：`3ba3118a22e93127a4ed485be72ef5ea`

输出结果应该类似于：

```
Cloning into 'repo'...
Could not create directory '/home/bandit27/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit27/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit27-git@localhost's password:
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
```

### 4. 查看克隆的仓库 👀

克隆完成后，我们可以进入仓库目录并查看其内容：

```bash
cd repo
ls -la
```

输出结果应该类似于：

```
total 16
drwxr-sr-x 3 bandit27 root 4096 May 16 13:00 .
drwxr-sr-x 3 bandit27 root 4096 May 16 13:00 ..
drwxr-sr-x 8 bandit27 root 4096 May 16 13:00 .git
-rw-r--r-- 1 bandit27 root   68 May 16 13:00 README
```

我们可以看到仓库中有一个README文件。让我们查看它的内容：

```bash
cat README
```

输出结果应该显示下一关的密码：

```
The password to the next level is: 0ef186ac70e04ea33b4c1853d2526fa2
```

## 命令解释 🔍

- `mkdir -p /tmp/bandit27_git`：
  - `mkdir`：创建目录
  - `-p`：如果需要，创建父目录
  - `/tmp/bandit27_git`：要创建的目录
- `git clone ssh://bandit27-git@localhost/home/bandit27-git/repo`：
  - `git clone`：克隆Git仓库
  - `ssh://bandit27-git@localhost/home/bandit27-git/repo`：仓库的URL，使用SSH协议
- `cd repo`：
  - `cd`：切换目录
  - `repo`：目标目录
- `cat README`：
  - `cat`：显示文件内容
  - `README`：要显示内容的文件

## 学习要点总结 📌

1. **Git基础** - 学习了如何使用`git clone`命令克隆仓库
2. **SSH认证** - 了解了如何使用SSH协议进行Git操作
3. **临时目录使用** - 学习了如何使用临时目录进行操作
4. **仓库浏览** - 了解了如何浏览Git仓库的内容

## 提示 💡

- Git是一个功能强大的版本控制系统，广泛用于软件开发和协作
- 在实际工作中，通常会使用SSH密钥而不是密码进行Git认证，这更安全也更方便
- 临时目录（`/tmp`）通常用于存储临时文件，在系统重启后可能会被清除
- Git仓库包含一个`.git`目录，其中存储了版本控制信息

## 下一步 ⏭️

使用获得的密码登录到Level 28：

```bash
ssh bandit28@bandit.labs.overthewire.org -p 2220
```

密码：`0ef186ac70e04ea33b4c1853d2526fa2`

## 相关资源 🔗

- [Git基础教程](./resource/level_28/Git基础教程.md)
- [SSH协议详解](./resource/level_28/SSH协议详解.md)
- [Git与SSH配置](./resource/level_28/Git与SSH配置.md)
