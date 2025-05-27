# Bandit Level 6 → Level 7 攻略 🌍

## 关卡目标 🎯

本关卡的目标是在服务器上找到符合以下条件的文件，并读取其中的内容以获取下一关的密码：
- 属于用户bandit7
- 属于用户组bandit6
- 大小为33字节

## 所需知识 📚

### find命令的高级用法 🔎

除了前一关学习的基本用法外，`find`命令还可以根据文件的所有者和所属组进行搜索。

### 文件所有权 👤

在Linux中，每个文件都有一个所有者（用户）和一个所属组。这些信息可以通过`ls -l`命令查看，也可以作为`find`命令的搜索条件。

### 权限错误处理 ⚠️

在Linux中，普通用户可能没有权限访问某些目录或文件。当使用`find`命令搜索整个文件系统时，会遇到许多"Permission denied"（权限被拒绝）的错误消息。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

密码：`DXjZPULLxYr17uwoI01bNLQbtFemEgo7`

### 2. 使用find命令在整个服务器上搜索符合条件的文件 🔍

我们需要在整个文件系统中搜索，而不仅仅是当前目录，所以从根目录`/`开始搜索：

```bash
find / -user bandit7 -group bandit6 -size 33c
```

这个命令会产生大量的"Permission denied"错误消息，但在这些消息中，我们可以找到符合条件的文件：

```
find: '/run/lvm': Permission denied
find: '/run/screen/S-bandit1': Permission denied
...（更多权限错误）...
/var/lib/dpkg/info/bandit7.password
...（更多权限错误）...
```

从输出中我们可以看到，文件`/var/lib/dpkg/info/bandit7.password`符合我们的搜索条件。

### 3. 读取找到的文件内容 📄

使用`cat`命令读取找到的文件内容：

```bash
cat /var/lib/dpkg/info/bandit7.password
```

输出结果将显示下一关的密码：

```
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```

## 命令解释 🔍

- `find / -user bandit7 -group bandit6 -size 33c`：
  - `find`：搜索文件的命令
  - `/`：从根目录开始搜索（搜索整个文件系统）
  - `-user bandit7`：指定文件所有者为bandit7
  - `-group bandit6`：指定文件所属组为bandit6
  - `-size 33c`：指定文件大小为33字节（c表示字节）

## 学习要点总结 📌

1. **全系统搜索** - 学习了如何在整个文件系统中搜索文件
2. **所有权搜索** - 了解了如何根据文件的所有者和所属组搜索文件
3. **权限错误处理** - 学习了如何在大量权限错误消息中找到有用信息
4. **组合搜索条件** - 进一步练习了组合多个条件进行文件搜索

## 提示 💡

- 使用`2>/dev/null`可以将错误消息重定向到"黑洞"，使输出更清晰：
  ```bash
  find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
  ```
- 在Linux中，`/var/lib/dpkg/info/`目录通常包含与软件包相关的信息
- 当搜索整个文件系统时，预期会遇到许多权限错误，这是正常的

## 下一步 ⏭️

使用获得的密码登录到Level 7：

```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
```

密码：`HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs`

## 相关资源 🔗

- [Linux_find命令详解](./resource/level_6/Linux_find命令详解.md)
- [Linux文件所有权详解](./resource/level6→level7/Linux文件所有权详解.md)
- [Linux错误重定向指南](./resource/level6→level7/Linux错误重定向指南.md)
