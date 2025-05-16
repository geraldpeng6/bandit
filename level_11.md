# Bandit Level 10 → Level 11 攻略 🔐

## 关卡目标 🎯

本关卡的目标是在文件`data.txt`中找到密码，该文件包含Base64编码的数据。我们需要解码这些数据以获取下一关的密码。

## 所需知识 📚

### Base64编码 🔢

Base64是一种将二进制数据转换为ASCII字符的编码方式，常用于在纯文本环境中传输二进制数据。它使用A-Z、a-z、0-9和+/这64个字符来表示数据，因此得名Base64。

Base64编码的特点：
- 编码后的数据比原始数据大约增加33%
- 编码后的数据只包含可打印的ASCII字符
- 编码后的数据通常以等号（=）结尾作为填充

### base64命令 🔄

Linux提供了`base64`命令用于编码和解码Base64数据。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
```

密码：`truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk`

### 2. 查看当前目录中的文件 👀

登录后，使用`ls`命令查看当前目录中的文件：

```bash
ls
```

输出结果应该显示有一个名为`data.txt`的文件：

```
data.txt
```

### 3. 查看文件内容 📄

使用`cat`命令查看文件内容：

```bash
cat data.txt
```

输出结果应该显示Base64编码的数据，类似于：

```
VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==
```

### 4. 使用base64命令解码数据 🔓

我们可以使用`base64`命令的`-d`选项解码数据：

```bash
cat data.txt | base64 -d
```

或者：

```bash
base64 -d data.txt
```

输出结果应该显示解码后的数据，包含下一关的密码：

```
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```

从输出中我们可以看到，密码是`IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR`。

## 命令解释 🔍

- `cat data.txt | base64 -d`：
  - `cat data.txt`：显示`data.txt`文件的内容
  - `|`：管道操作符，将`cat`命令的输出作为`base64`命令的输入
  - `base64 -d`：使用Base64解码数据（`-d`选项表示解码）

## 学习要点总结 📌

1. **Base64编码基础** - 学习了Base64编码的概念和特点
2. **数据解码** - 了解了如何使用`base64`命令解码Base64编码的数据
3. **命令组合** - 进一步练习了使用管道组合多个命令
4. **编码识别** - 学习了如何识别Base64编码的数据

## 提示 💡

- Base64编码的数据通常以等号（=）结尾作为填充
- Base64编码常用于电子邮件附件、网页中的图片嵌入和其他需要在纯文本环境中传输二进制数据的场景
- 在Linux中，除了`base64`命令外，还可以使用其他工具如`openssl`进行Base64编码和解码

## 下一步 ⏭️

使用获得的密码登录到Level 11：

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

密码：`IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR`

## 相关资源 🔗

- [Base64 - 维基百科](./resource/level_11/Base64维基百科.md)
- [Linux base64命令详解](./resource/level_11/Linux_base64命令详解.md)
- [Base64编码和解码原理](./resource/level_11/Base64编码和解码原理.md)
