# Bandit Level 5 → Level 6 攻略 🔍

## 关卡目标 🎯

本关卡的目标是在`inhere`目录中找到符合以下条件的文件，并读取其中的内容以获取下一关的密码：
- 人类可读
- 大小为1033字节
- 不可执行

## 所需知识 📚

### find命令 🔎

`find`命令是Linux中强大的文件搜索工具，可以根据各种条件（如名称、大小、权限、类型等）搜索文件。

### 文件大小单位 📏

在Linux中，文件大小可以用不同的单位表示：
- `c`：字节（bytes）
- `k`：千字节（kilobytes，1024字节）
- `M`：兆字节（megabytes，1024千字节）
- `G`：吉字节（gigabytes，1024兆字节）

### 文件权限 🔒

Linux文件权限分为三类：
- 所有者权限（user）
- 组权限（group）
- 其他用户权限（others）

每类权限又分为三种：
- 读取权限（r）
- 写入权限（w）
- 执行权限（x）

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

密码：`koReBOKuIDDepwhWk7jZC0RTdopnAYKh`

### 2. 查看当前目录中的文件 👀

登录后，使用`ls`命令查看当前目录中的文件：

```bash
ls
```

输出结果应该显示有一个名为`inhere`的目录：

```
inhere
```

### 3. 进入inhere目录 📂

使用`cd`命令进入`inhere`目录：

```bash
cd inhere
```

### 4. 查看目录内容 📋

```bash
ls
```

输出结果应该显示有多个目录，类似于：

```
maybehere00  maybehere04  maybehere08  maybehere12  maybehere16
maybehere01  maybehere05  maybehere09  maybehere13  maybehere17
maybehere02  maybehere06  maybehere10  maybehere14  maybehere18
maybehere03  maybehere07  maybehere11  maybehere15  maybehere19
```

### 5. 使用find命令查找符合条件的文件 🔍

我们可以使用`find`命令结合多个条件来查找符合要求的文件：

```bash
find -size 1033c ! -executable -type f
```

输出结果应该显示符合条件的文件路径：

```
./maybehere07/.file2
```

这表明文件位于`maybehere07`目录中，名为`.file2`。

### 6. 读取找到的文件内容 📄

使用`cat`命令读取找到的文件内容：

```bash
cat ./maybehere07/.file2
```

输出结果将显示下一关的密码：

```
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```

## 命令解释 🔍

- `find -size 1033c ! -executable -type f`：
  - `find`：搜索文件的命令
  - `-size 1033c`：指定文件大小为1033字节（c表示字节）
  - `! -executable`：指定文件不可执行（`!`表示否定）
  - `-type f`：指定搜索普通文件（而不是目录或其他特殊文件）

## 学习要点总结 📌

1. **find命令基础** - 学习了如何使用`find`命令搜索符合特定条件的文件
2. **文件大小指定** - 了解了如何在`find`命令中指定文件大小
3. **文件权限检查** - 学习了如何检查文件是否可执行
4. **组合搜索条件** - 了解了如何组合多个条件进行文件搜索

## 提示 💡

- `find`命令是Linux中最强大的文件搜索工具之一，掌握它可以大大提高工作效率
- 使用`man find`命令可以查看`find`命令的完整文档和更多选项
- 在实际工作中，可以组合更多条件来精确定位所需文件

## 下一步 ⏭️

使用获得的密码登录到Level 6：

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

密码：`DXjZPULLxYr17uwoI01bNLQbtFemEgo7`

## 相关资源 🔗

- [Linux_find命令详解](./resource/level5→level6/Linux_find命令详解.md)
- [查找特定大小的文件指南](./resource/level5→level6/查找特定大小的文件指南.md)
- [Linux文件权限详解](./resource/level5→level6/Linux文件权限详解.md)
