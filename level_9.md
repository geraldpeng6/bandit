# Bandit Level 8 → Level 9 攻略 🔄

## 关卡目标 🎯

本关卡的目标是在文件`data.txt`中找到唯一只出现一次的行，该行包含下一关的密码。

## 所需知识 📚

### 文本处理命令 🔄

Linux提供了多种文本处理命令，可以用于排序、去重、计数等操作：

- `sort`：对文本行进行排序
- `uniq`：报告或忽略重复的行
- `wc`：计算行数、单词数和字符数

### 管道（Pipe）操作 📊

管道操作符`|`允许将一个命令的输出作为另一个命令的输入，这是Linux命令行的强大特性之一。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
```

密码：`cvX2JJa4CFALtqS87jk27qwqGhBM9plV`

### 2. 查看当前目录中的文件 👀

登录后，使用`ls`命令查看当前目录中的文件：

```bash
ls
```

输出结果应该显示有一个名为`data.txt`的文件：

```
data.txt
```

### 3. 查看文件内容（可选，但不推荐）❌

我们可以尝试使用`cat`命令查看文件内容，但由于文件可能包含大量重复行，这不是一个好方法：

```bash
cat data.txt
```

### 4. 使用sort和uniq命令找到唯一的行 🔍

要找到只出现一次的行，我们可以使用`sort`命令对文件内容进行排序，然后使用`uniq -u`命令只显示唯一的行：

```bash
sort data.txt | uniq -u
```

输出结果应该显示唯一的行，即下一关的密码：

```
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

## 命令解释 🔍

- `sort data.txt | uniq -u`：
  - `sort data.txt`：对`data.txt`文件的内容按行进行字典序排序
  - `|`：管道操作符，将`sort`命令的输出作为`uniq`命令的输入
  - `uniq -u`：只显示唯一的行（只出现一次的行）
    - `uniq`命令默认只比较相邻的行，所以在使用前需要先用`sort`命令对文件进行排序

## 学习要点总结 📌

1. **文本排序** - 学习了如何使用`sort`命令对文本进行排序
2. **唯一行识别** - 了解了如何使用`uniq`命令识别唯一的行
3. **管道操作** - 学习了如何使用管道操作符`|`组合多个命令
4. **命令组合** - 体验了通过组合简单命令来解决复杂问题的Linux哲学

## 提示 💡

- `uniq`命令只比较相邻的行，所以在使用前通常需要先用`sort`命令对文件进行排序
- `uniq`命令的常用选项包括：
  - `-c`：显示每行出现的次数
  - `-d`：只显示重复的行
  - `-u`：只显示唯一的行（只出现一次的行）
- 在实际工作中，`sort`和`uniq`命令常用于日志分析和数据处理

## 下一步 ⏭️

使用获得的密码登录到Level 9：

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

密码：`UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR`

## 相关资源 🔗

- [如何找到存储在文件"data.txt"中的特定文本，并且它只出现一次](https://stackoverflow.com/questions/12782827/how-to-find-the-particular-text-stored-in-the-file-data-txt-and-it-occurs-only)
- [Linux sort命令详解](https://www.geeksforgeeks.org/sort-command-linuxunix-examples/)
- [Linux uniq命令详解](https://www.geeksforgeeks.org/uniq-command-in-linux-with-examples/)
- [Linux管道操作详解](https://www.geeksforgeeks.org/piping-in-unix-or-linux/)
