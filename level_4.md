# Bandit Level 3 → Level 4 攻略 🕵️

## 关卡目标 🎯

本关卡的目标是找到存储在`inhere`目录中的隐藏文件，并读取其中的内容以获取下一关的密码。

## 所需知识 📚

### Linux隐藏文件 🙈

在Linux系统中，以点（`.`）开头的文件和目录被视为"隐藏"的。这些文件在使用普通的`ls`命令时不会显示，需要使用特殊选项才能看到它们。

### ls命令的常用选项 📋

- `ls -a`：显示所有文件，包括隐藏文件（以`.`开头的文件）
- `ls -l`：使用长格式列出文件，显示详细信息（权限、所有者、大小等）
- `ls -h`：以人类可读的格式显示文件大小（KB、MB等）
- `ls -al`：组合选项，显示所有文件的详细信息

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

密码：`UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK`

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

### 4. 尝试查看目录内容（可能看不到隐藏文件）❓

如果我们使用普通的`ls`命令：

```bash
ls
```

可能不会显示任何内容，因为文件是隐藏的。

### 5. 使用ls -a查看所有文件（包括隐藏文件）🔍

```bash
ls -al
```

输出结果应该类似于：

```
total 12
drwxr-xr-x 2 root    root    4096 Oct 16  2018 .
drwxr-xr-x 3 root    root    4096 Oct 16  2018 ..
-rw-r----- 1 bandit4 bandit3   33 Oct 16  2018 .hidden
```

这里我们可以看到一个名为`.hidden`的隐藏文件。

### 6. 读取隐藏文件内容 📄

使用`cat`命令读取`.hidden`文件的内容：

```bash
cat .hidden
```

输出结果将显示下一关的密码：

```
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```

## 命令解释 🔍

- `cd inhere`：切换到名为"inhere"的目录
- `ls -al`：
  - `-a`选项显示所有文件，包括隐藏文件
  - `-l`选项使用长格式列出文件，显示详细信息
- `cat .hidden`：显示名为".hidden"的文件内容

## 学习要点总结 📌

1. **隐藏文件概念** - 学习了Linux中隐藏文件的概念（以`.`开头的文件）
2. **ls命令高级用法** - 了解了`ls`命令的各种选项，特别是`-a`和`-l`
3. **目录导航** - 练习了使用`cd`命令在目录之间导航

## 提示 💡

- 在Linux系统中，`.`表示当前目录，`..`表示上一级目录
- 许多配置文件在Linux中都是隐藏文件，例如`.bashrc`、`.profile`等
- 使用`ls -la`或`ls -al`是查看目录完整内容的常用方法

## 下一步 ⏭️

使用获得的密码登录到Level 4：

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

密码：`pIwrPrtPN36QITSp3EQaw936yaFoFgAB`

## 相关资源 🔗

- [Linux ls命令基础教程](./resource/level_1/linux_ls命令基础教程.md)
- [Linux隐藏文件和目录详解](./resource/level_4/Linux隐藏文件和目录详解.md)
- [Linux文件系统导航基础](./resource/level_4/Linux文件系统导航基础.md)
