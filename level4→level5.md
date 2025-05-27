# Bandit Level 4 → Level 5 攻略 🔎

## 关卡目标 🎯

本关卡的目标是在`inhere`目录中找到唯一一个人类可读的文件，并读取其中的内容以获取下一关的密码。

## 所需知识 📚

### 文件类型识别 📋

在Linux中，文件的扩展名并不总是能准确反映文件的实际类型。Linux提供了`file`命令，可以通过检查文件内容来确定文件的实际类型。

### 人类可读文件 📝

"人类可读"通常指的是包含文本内容的文件，而不是二进制数据或其他格式的文件。在`file`命令的输出中，人类可读文件通常被标识为"ASCII text"或类似的描述。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

密码：`pIwrPrtPN36QITSp3EQaw936yaFoFgAB`

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

输出结果应该显示有多个文件，名称类似于：

```
-file00  -file02  -file04  -file06  -file08
-file01  -file03  -file05  -file07  -file09
```

### 5. 使用file命令检查所有文件的类型 🔍

我们可以使用`file`命令结合通配符来检查所有文件的类型：

```bash
file ./-*
```

输出结果应该类似于：

```
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```

从输出中我们可以看到，只有`./-file07`被标识为"ASCII text"，这表明它是唯一的人类可读文件。

### 6. 读取人类可读文件的内容 📄

使用`cat`命令读取`./-file07`文件的内容：

```bash
cat ./-file07
```

输出结果将显示下一关的密码：

```
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```

## 命令解释 🔍

- `file ./-*`：
  - `file`命令用于确定文件类型
  - `./-*`是一个通配符模式，匹配当前目录中所有以`-`开头的文件
  - 使用`./`前缀是为了正确处理以`-`开头的文件名（与Level 1类似）

## 学习要点总结 📌

1. **文件类型识别** - 学习了如何使用`file`命令确定文件的实际类型
2. **通配符使用** - 了解了如何使用通配符（如`*`）一次处理多个文件
3. **ASCII与二进制** - 理解了人类可读文件（ASCII文本）与非人类可读文件（二进制数据）的区别

## 提示 💡

- 在Linux中，文件的内容决定了文件的类型，而不是文件名或扩展名
- `file`命令是确定未知文件类型的有用工具
- 使用通配符可以大大提高命令行操作的效率

## 下一步 ⏭️

使用获得的密码登录到Level 5：

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

密码：`koReBOKuIDDepwhWk7jZC0RTdopnAYKh`

## 相关资源 🔗

- [Linux_file命令详解](./resource/level4→level5/Linux_file命令详解.md)
- [Linux通配符使用指南](./resource/level4→level5/Linux通配符使用指南.md)
- [ASCII与二进制文件的区别](./resource/level4→level5/ASCII与二进制文件的区别.md)
