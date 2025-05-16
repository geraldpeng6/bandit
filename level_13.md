# Bandit Level 12 → Level 13 攻略 🗜️

## 关卡目标 🎯

本关卡的目标是从文件`data.txt`中获取密码，该文件是一个十六进制转储（hexdump）文件，经过了多次压缩。我们需要先将其还原为原始文件，然后通过多次解压缩获取密码。

## 所需知识 📚

### 十六进制转储（Hexdump）📊

十六进制转储是一种以十六进制格式显示二进制数据的方法。它通常用于查看无法直接显示的二进制文件内容。

### 文件压缩和归档 🗜️

Linux支持多种压缩和归档格式，每种格式都有对应的工具：
- gzip/gunzip：处理.gz文件
- bzip2/bunzip2：处理.bz2文件
- tar：处理.tar文件（归档，不压缩）
- xz/unxz：处理.xz文件

### 文件类型识别 🔍

`file`命令可以通过检查文件内容来确定文件类型，这对于处理未知文件非常有用。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

密码：`5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu`

### 2. 创建临时工作目录 📂

由于我们需要创建多个文件，最好在`/tmp`目录下创建一个临时工作目录：

```bash
mkdir /tmp/mybandit12
cd /tmp/mybandit12
```

### 3. 复制数据文件到工作目录 📋

```bash
cp ~/data.txt .
```

### 4. 查看文件内容 📄

```bash
cat data.txt
```

输出结果应该显示十六进制数据，这是一个十六进制转储文件。

### 5. 将十六进制转储还原为二进制文件 🔄

使用`xxd`命令的`-r`选项将十六进制转储还原为二进制文件：

```bash
xxd -r data.txt > data1
```

### 6. 检查文件类型 🔍

使用`file`命令检查文件类型：

```bash
file data1
```

输出结果应该类似于：

```
data1: gzip compressed data, was "data2.bin", last modified: ...
```

这表明文件是一个gzip压缩文件。

### 7. 解压缩gzip文件 📦

首先，我们需要将文件重命名为正确的扩展名，然后使用`gunzip`命令解压缩：

```bash
mv data1 data1.gz
gunzip data1.gz
```

这将生成一个名为`data1`的文件。

### 8. 重复检查文件类型和解压缩 🔄

我们需要重复检查文件类型并解压缩，直到获取最终的文本文件：

```bash
file data1
```

输出可能显示这是一个bzip2压缩文件：

```
data1: bzip2 compressed data, block size = 900k
```

解压缩bzip2文件：

```bash
mv data1 data1.bz2
bunzip2 data1.bz2
```

继续检查文件类型并解压缩：

```bash
file data1
```

输出可能显示这是一个gzip压缩文件：

```
data1: gzip compressed data, was "data4.bin", ...
```

解压缩gzip文件：

```bash
mv data1 data1.gz
gunzip data1.gz
```

继续检查文件类型：

```bash
file data1
```

输出可能显示这是一个tar归档文件：

```
data1: POSIX tar archive (GNU)
```

解压缩tar文件：

```bash
mv data1 data1.tar
tar -xf data1.tar
```

这将提取一个名为`data5.bin`的文件。

### 9. 继续解压缩直到获取文本文件 📝

继续重复上述步骤，检查文件类型并使用相应的命令解压缩，直到最终获取一个ASCII文本文件。

最终，你应该得到一个包含密码的文本文件：

```bash
cat data9
```

输出结果应该显示下一关的密码：

```
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```

## 命令解释 🔍

- `xxd -r data.txt > data1`：
  - `xxd -r`：将十六进制转储还原为二进制数据
  - `> data1`：将输出重定向到文件`data1`
- `file data1`：确定文件`data1`的类型
- `mv data1 data1.gz`：将文件重命名为正确的扩展名
- `gunzip data1.gz`：解压缩gzip文件
- `bunzip2 data1.bz2`：解压缩bzip2文件
- `tar -xf data1.tar`：解压缩tar归档文件

## 学习要点总结 📌

1. **十六进制转储** - 学习了如何使用`xxd`命令处理十六进制转储
2. **文件类型识别** - 了解了如何使用`file`命令识别文件类型
3. **多种压缩格式** - 学习了如何处理不同的压缩和归档格式
4. **临时目录使用** - 了解了如何使用`/tmp`目录进行临时文件操作

## 提示 💡

- 在Linux中，文件扩展名只是为了方便人类识别，系统实际上是通过检查文件内容来确定文件类型
- `file`命令是确定未知文件类型的强大工具
- 处理压缩文件时，确保使用正确的工具和选项
- 在实际工作中，可能会遇到多层压缩的文件，掌握这些解压缩技能非常有用

## 下一步 ⏭️

使用获得的密码登录到Level 13：

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

密码：`8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL`

## 相关资源 🔗

- [十六进制转储 - 维基百科](https://zh.wikipedia.org/wiki/十六进制转储)
- [Linux文件压缩和解压缩命令](https://www.tecmint.com/linux-compress-decompress-extract-files/)
- [Linux file命令详解](https://www.geeksforgeeks.org/file-command-in-linux-with-examples/)
