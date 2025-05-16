# Bandit Level 9 → Level 10 攻略 📊

## 关卡目标 🎯

本关卡的目标是在文件`data.txt`中找到人类可读的字符串，这些字符串以多个等号（`=`）字符开头，其中包含下一关的密码。

## 所需知识 📚

### 二进制文件与文本文件 📊

文件可以包含二进制数据（非文本数据）和文本数据的混合。在这种情况下，直接使用`cat`命令可能会显示乱码或不可读的字符。

### strings命令 🔤

`strings`命令用于从二进制文件中提取可打印的字符序列（文本字符串）。这对于检查二进制文件或包含非文本数据的文件非常有用。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

密码：`UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR`

### 2. 查看当前目录中的文件 👀

登录后，使用`ls`命令查看当前目录中的文件：

```bash
ls
```

输出结果应该显示有一个名为`data.txt`的文件：

```
data.txt
```

### 3. 尝试查看文件内容（可能显示乱码）❌

如果我们尝试使用`cat`命令查看文件内容：

```bash
cat data.txt
```

我们可能会看到大量乱码和不可读的字符，因为文件包含二进制数据。

### 4. 使用strings命令提取可读字符串 🔍

我们可以使用`strings`命令提取文件中的可读字符串：

```bash
strings data.txt
```

这会显示文件中所有可打印的字符序列，但可能仍然有很多输出。

### 5. 使用grep命令过滤以等号开头的行 🔎

我们可以将`strings`命令的输出通过管道传递给`grep`命令，只显示以多个等号开头的行：

```bash
strings data.txt | grep "=="
```

输出结果应该类似于：

```
========== the
========== password
========== isa
========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```

从输出中我们可以看到，密码是`truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk`。

## 命令解释 🔍

- `strings data.txt | grep "=="`：
  - `strings data.txt`：从`data.txt`文件中提取可打印的字符序列
  - `|`：管道操作符，将`strings`命令的输出作为`grep`命令的输入
  - `grep "=="`：只显示包含`==`的行

## 学习要点总结 📌

1. **二进制文件处理** - 学习了如何从二进制文件中提取可读文本
2. **strings命令** - 了解了`strings`命令的用途和基本用法
3. **命令组合** - 进一步练习了使用管道组合多个命令
4. **文本过滤** - 学习了如何使用`grep`命令过滤特定模式的文本

## 提示 💡

- `strings`命令默认只提取至少4个字符的序列，可以使用`-n`选项更改这个最小长度
- 在实际工作中，`strings`命令常用于检查二进制文件、可执行文件或其他非文本文件中的文本内容
- 组合使用`strings`和`grep`命令是分析二进制文件的常用技巧

## 下一步 ⏭️

使用获得的密码登录到Level 10：

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
```

密码：`truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk`

## 相关资源 🔗

- [Linux strings命令教程](./resource/level_10/Linux_strings命令教程.md)
- [Linux中的二进制文件与文本文件](./resource/level_10/Linux中的二进制文件与文本文件.md)
- [使用grep和正则表达式](./resource/level_10/使用grep和正则表达式.md)
