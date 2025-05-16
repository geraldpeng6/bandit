# Bandit Level 11 → Level 12 攻略 🔄

## 关卡目标 🎯

本关卡的目标是在文件`data.txt`中找到密码，该文件中的所有小写字母（a-z）和大写字母（A-Z）都已经被旋转13位（ROT13）。我们需要解码这些数据以获取下一关的密码。

## 所需知识 📚

### ROT13加密 🔄

ROT13（Rotate by 13 places）是一种简单的字母替换密码，它将字母表中的每个字母替换为其后的第13个字母。例如：
- A → N
- B → O
- ...
- Z → M

ROT13的特点：
- 只对字母进行变换，数字和符号保持不变
- 由于英文字母表有26个字母，所以应用ROT13两次会得到原始文本
- ROT13是一种非常简单的加密方式，主要用于隐藏文本，而不是真正的安全加密

### tr命令 🔄

Linux提供了`tr`命令用于转换或删除字符。它可以用于实现ROT13解码。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

密码：`IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR`

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

输出结果应该显示ROT13加密的数据，类似于：

```
Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh
```

### 4. 使用tr命令解码ROT13数据 🔓

我们可以使用`tr`命令将每个字母替换为其前13个或后13个字母（对于ROT13来说是等价的）：

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

输出结果应该显示解码后的数据，包含下一关的密码：

```
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```

从输出中我们可以看到，密码是`5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu`。

## 命令解释 🔍

- `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`：
  - `cat data.txt`：显示`data.txt`文件的内容
  - `|`：管道操作符，将`cat`命令的输出作为`tr`命令的输入
  - `tr 'A-Za-z' 'N-ZA-Mn-za-m'`：将字母A-Z替换为N-ZA-M，将字母a-z替换为n-za-m
    - 这实际上是将每个字母向前或向后旋转13位，从而实现ROT13解码

## 学习要点总结 📌

1. **ROT13加密基础** - 学习了ROT13加密的概念和特点
2. **字符转换** - 了解了如何使用`tr`命令进行字符转换
3. **简单密码学** - 接触了基本的密码学概念和简单的加密/解密方法
4. **命令组合** - 进一步练习了使用管道组合多个命令

## 提示 💡

- ROT13是一种非常简单的加密方式，不应该用于保护敏感信息
- 在Linux中，`tr`命令是一个强大的字符转换工具，可以用于各种文本处理任务
- 除了`tr`命令外，还可以使用其他工具如Python脚本或在线工具进行ROT13解码

## 下一步 ⏭️

使用获得的密码登录到Level 12：

```bash
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

密码：`5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu`

## 相关资源 🔗

- [ROT13 - 维基百科](./resource/level_12/ROT13维基百科.md)
- [Linux tr命令详解](./resource/level_12/Linux_tr命令详解.md)
- [在Linux命令行解码ROT13](./resource/level_12/在Linux命令行解码ROT13.md)
