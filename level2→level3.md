# Bandit Level 2 → Level 3 攻略 📄

## 关卡目标 🎯

本关卡的目标是找到存储在主目录中名为`spaces in this filename`的文件，并读取其中的内容以获取下一关的密码。

## 所需知识 📚

### 处理包含空格的文件名 🔤

在Linux命令行中，空格通常用作参数分隔符。当文件名包含空格时，如果不进行特殊处理，命令会将其解释为多个不同的参数，从而导致错误。

处理包含空格的文件名有几种方法：
1. 使用引号（单引号或双引号）将文件名括起来
2. 在空格前使用反斜杠`\`进行转义
3. 使用Tab键自动补全文件名

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
```

密码：`CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9`

### 2. 查看当前目录中的文件 👀

登录后，使用`ls`命令查看当前目录中的文件：

```bash
ls
```

输出结果应该显示有一个名为`spaces in this filename`的文件：

```
spaces in this filename
```

### 3. 尝试直接读取文件（会失败）❌

如果我们尝试直接使用`cat spaces in this filename`命令：

```bash
cat spaces in this filename
```

这将不会正确工作，因为`cat`命令会将其解释为四个不同的参数：
- `spaces`（一个文件名）
- `in`（一个文件名）
- `this`（一个文件名）
- `filename`（一个文件名）

系统会尝试显示四个不同文件的内容，而不是一个包含空格的文件名。

### 4. 正确读取包含空格的文件名 ✅

有几种方法可以正确读取包含空格的文件名：

方法1：使用单引号

```bash
cat 'spaces in this filename'
```

方法2：使用双引号

```bash
cat "spaces in this filename"
```

方法3：使用反斜杠转义空格

```bash
cat spaces\ in\ this\ filename
```

所有这些方法都会显示文件内容，即下一关的密码：

```
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

## 命令解释 🔍

- `cat 'spaces in this filename'`：单引号告诉shell将引号内的所有内容视为单个参数
- `cat "spaces in this filename"`：双引号的作用类似于单引号，但允许变量扩展
- `cat spaces\ in\ this\ filename`：反斜杠`\`告诉shell将紧随其后的空格视为文件名的一部分，而不是参数分隔符

## 学习要点总结 📌

1. **空格处理** - 学习了如何处理包含空格的文件名
2. **引号使用** - 了解了单引号和双引号在shell中的作用
3. **转义字符** - 学习了使用反斜杠转义特殊字符的方法

## 提示 💡

- 使用Tab键自动补全是处理复杂文件名的最简单方法：输入文件名的前几个字符，然后按Tab键
- 在实际工作中，尽量避免在文件名中使用空格，可以使用下划线`_`或连字符`-`代替

## 下一步 ⏭️

使用获得的密码登录到Level 3：

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

密码：`UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK`

## 相关资源 🔗

- [Google搜索"文件名中的空格"](https://www.google.com/search?q=spaces+in+filename)
- [Linux文件命名最佳实践](./resource/level2→level3/Linux文件命名最佳实践.md)
- [Bash引号和转义指南](./resource/level2→level3/Bash引号和转义指南.md)
