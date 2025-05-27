# Linux终端和环境变量详解 🖥️

## 什么是终端？ 🤔

终端(Terminal)是用户与Linux系统交互的文本界面。在早期计算机时代，终端是一种物理设备，连接到中央计算机。现代Linux系统中的"终端"通常是指终端模拟器(Terminal Emulator)，它模拟了传统终端的功能，提供命令行界面(CLI)让用户输入命令与系统交互。

## 终端与Shell的区别 🔄

很多人混淆终端和Shell的概念，它们其实是不同的：

- **终端(Terminal)** 📟 - 是输入命令和显示输出的界面
- **Shell** 🐚 - 是解释和执行命令的程序

可以这样理解：终端是Shell的容器，Shell在终端中运行。常见的Shell包括Bash、Zsh、Fish等。

## 常见的终端模拟器 💻

不同的Linux发行版和桌面环境提供了各种终端模拟器：

1. **GNOME Terminal** - GNOME桌面环境的默认终端
2. **Konsole** - KDE桌面环境的默认终端
3. **xterm** - X Window系统的标准终端
4. **rxvt/urxvt** - 轻量级终端，资源占用少
5. **Terminator** - 支持分屏的高级终端
6. **Alacritty** - GPU加速的现代终端
7. **Kitty** - 另一个GPU加速的终端，功能丰富

## 终端基本操作 🛠️

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+C` | 中断当前命令 |
| `Ctrl+D` | 发送EOF信号，通常用于退出shell |
| `Ctrl+L` | 清屏 |
| `Ctrl+A` | 移动光标到行首 |
| `Ctrl+E` | 移动光标到行尾 |
| `Ctrl+U` | 删除光标前的所有字符 |
| `Ctrl+K` | 删除光标后的所有字符 |
| `Ctrl+W` | 删除光标前的一个单词 |
| `Ctrl+R` | 搜索命令历史 |
| `Tab` | 自动补全命令或文件名 |
| `Up/Down` | 浏览命令历史 |

### 终端颜色和格式

终端支持ANSI转义序列来控制文本颜色和格式：

```bash
# 文本颜色
echo -e "\e[31m红色文本\e[0m"
echo -e "\e[32m绿色文本\e[0m"
echo -e "\e[33m黄色文本\e[0m"
echo -e "\e[34m蓝色文本\e[0m"

# 文本格式
echo -e "\e[1m粗体文本\e[0m"
echo -e "\e[4m下划线文本\e[0m"
```

### 终端大小和分辨率

终端窗口的大小会影响某些程序的显示方式，特别是文本查看器和编辑器：

```bash
# 查看当前终端大小（行数和列数）
stty size

# 调整终端大小（在某些终端模拟器中）
resize
```

## 什么是环境变量？ 🌍

环境变量是存储在操作系统中的命名值，可以影响程序的行为。它们在Shell会话和从Shell启动的程序中可用，用于配置系统行为和程序运行环境。

## 常见的环境变量 📋

| 变量名 | 描述 |
|--------|------|
| `PATH` | 定义Shell查找可执行文件的目录列表 |
| `HOME` | 当前用户的主目录 |
| `USER` | 当前登录用户的用户名 |
| `SHELL` | 当前用户的默认Shell |
| `LANG` | 系统语言和区域设置 |
| `TERM` | 终端类型 |
| `PS1` | 主提示符格式 |
| `PS2` | 次提示符格式 |
| `EDITOR` | 默认文本编辑器 |
| `DISPLAY` | X服务器显示地址 |
| `PWD` | 当前工作目录 |
| `OLDPWD` | 上一个工作目录 |
| `HISTSIZE` | 命令历史记录的大小 |
| `HISTFILE` | 存储命令历史的文件 |

## 查看环境变量 👀

```bash
# 查看所有环境变量
env
printenv

# 查看特定环境变量
echo $PATH
printenv PATH

# 查看所有变量（包括Shell变量）
set
```

## 设置环境变量 ⚙️

### 临时设置（当前会话有效）

```bash
# 设置环境变量
export 变量名=值

# 示例
export EDITOR=vim
export PATH=$PATH:/新/路径
```

### 永久设置（对所有会话有效）

要永久设置环境变量，需要将设置命令添加到Shell配置文件中：

**对于Bash用户：**
- `~/.bashrc` - 交互式非登录Shell
- `~/.bash_profile`、`~/.profile` - 登录Shell

**对于Zsh用户：**
- `~/.zshrc`

示例（添加到配置文件）：
```bash
export EDITOR=vim
export PATH=$PATH:/新/路径
```

修改配置文件后，需要重新加载配置或重新登录才能生效：
```bash
source ~/.bashrc  # 重新加载Bash配置
```

### 系统级环境变量

要为所有用户设置环境变量，可以修改以下文件：
- `/etc/environment` - 系统范围的环境变量
- `/etc/profile` - 所有用户的登录Shell
- `/etc/profile.d/*.sh` - 分模块的系统配置

## 特殊的环境变量 🌟

### PATH变量

`PATH`变量定义了Shell查找可执行文件的目录列表，目录之间用冒号分隔：

```bash
# 查看当前PATH
echo $PATH

# 添加目录到PATH（临时）
export PATH=$PATH:/新/目录

# 添加目录到PATH的开头（临时）
export PATH=/新/目录:$PATH
```

### TERM变量

`TERM`变量定义了终端类型，影响终端的功能和行为：

```bash
# 查看当前终端类型
echo $TERM

# 设置终端类型
export TERM=xterm-256color
```

常见的TERM值：
- `xterm` - 基本X终端
- `xterm-256color` - 支持256色的X终端
- `linux` - Linux控制台
- `screen` - GNU Screen终端
- `tmux` - Tmux终端

### PS1变量（提示符）

`PS1`变量定义了命令提示符的格式：

```bash
# 设置简单提示符
export PS1="[\u@\h \W]\\$ "
```

常用的PS1转义序列：
- `\u` - 当前用户名
- `\h` - 主机名
- `\w` - 当前工作目录（完整路径）
- `\W` - 当前工作目录（仅目录名）
- `\t` - 当前时间（24小时制）
- `\d` - 当前日期
- `\$` - 如果是root用户显示#，否则显示$
- `\[` 和 `\]` - 包围非打印字符（如颜色代码）

彩色提示符示例：
```bash
export PS1="\[\e[32m\]\u@\h\[\e[0m\]:\[\e[34m\]\w\[\e[0m\]\\$ "
```

## 环境变量的作用域 🔍

环境变量有不同的作用域：

1. **Shell变量** - 仅在当前Shell中可用
2. **环境变量** - 在当前Shell及其子进程中可用
3. **系统环境变量** - 对所有用户和进程可用

```bash
# 设置Shell变量（仅当前Shell可见）
变量名=值

# 将Shell变量提升为环境变量（子进程可见）
export 变量名

# 取消环境变量的导出
export -n 变量名

# 删除变量
unset 变量名
```

## 环境变量与程序行为 🔄

许多程序使用环境变量来调整其行为：

### 示例1：语言和区域设置

```bash
# 设置系统语言为英文
export LANG=en_US.UTF-8

# 设置系统语言为中文
export LANG=zh_CN.UTF-8
```

### 示例2：代理设置

```bash
# 设置HTTP代理
export http_proxy=http://代理服务器:端口
export https_proxy=http://代理服务器:端口

# 设置不使用代理的地址
export no_proxy=localhost,127.0.0.1
```

### 示例3：程序特定变量

```bash
# Git配置
export GIT_EDITOR=vim

# Java配置
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk
export PATH=$PATH:$JAVA_HOME/bin
```

## 终端和环境变量的高级用法 🚀

### 1. 终端多路复用器

终端多路复用器允许在一个终端窗口中运行多个终端会话：

1. **tmux** - 功能强大的终端多路复用器
   ```bash
   # 安装tmux
   sudo apt install tmux  # Debian/Ubuntu
   sudo yum install tmux  # CentOS/RHEL
   
   # 启动tmux
   tmux
   
   # 基本操作（默认前缀键是Ctrl+b）
   # Ctrl+b c - 创建新窗口
   # Ctrl+b , - 重命名当前窗口
   # Ctrl+b n - 切换到下一个窗口
   # Ctrl+b p - 切换到上一个窗口
   # Ctrl+b % - 垂直分割窗格
   # Ctrl+b " - 水平分割窗格
   # Ctrl+b 方向键 - 在窗格间移动
   # Ctrl+b d - 分离会话
   ```

2. **screen** - 另一个终端多路复用器
   ```bash
   # 安装screen
   sudo apt install screen  # Debian/Ubuntu
   sudo yum install screen  # CentOS/RHEL
   
   # 启动screen
   screen
   
   # 基本操作（默认前缀键是Ctrl+a）
   # Ctrl+a c - 创建新窗口
   # Ctrl+a A - 重命名当前窗口
   # Ctrl+a n - 切换到下一个窗口
   # Ctrl+a p - 切换到上一个窗口
   # Ctrl+a S - 水平分割窗口
   # Ctrl+a | - 垂直分割窗口
   # Ctrl+a tab - 在窗格间切换
   # Ctrl+a d - 分离会话
   ```

### 2. 环境变量子进程继承

子进程会继承父进程的环境变量，但不会继承Shell变量：

```bash
# 测试环境变量继承
TEST_ENV=123
export TEST_EXPORT=456

bash -c 'echo $TEST_ENV'  # 不会显示任何内容
bash -c 'echo $TEST_EXPORT'  # 显示456
```

### 3. 使用envsubst替换模板中的环境变量

```bash
# 创建模板文件
cat > template.txt << EOF
用户: $USER
主目录: $HOME
终端: $TERM
EOF

# 使用环境变量替换模板
envsubst < template.txt
```

## 常见问题解答 ❓

### 问题：为什么我设置的环境变量在新终端中不可用？

**原因**：环境变量设置可能只是临时的，或者没有添加到正确的配置文件中。

**解决方案**：将环境变量设置添加到适当的Shell配置文件（如`~/.bashrc`或`~/.profile`）。

### 问题：如何在脚本中使用环境变量？

**解决方案**：直接在脚本中使用`$变量名`引用环境变量：

```bash
#!/bin/bash
echo "当前用户: $USER"
echo "主目录: $HOME"
```

### 问题：如何临时更改终端大小以影响程序行为？

**解决方案**：大多数终端模拟器允许通过拖动窗口边缘调整大小。对于远程终端，可以使用`stty`命令：

```bash
# 设置终端为24行80列
stty rows 24 columns 80
```

## 练习题 🎯

1. 创建一个名为`MY_VAR`的环境变量，值为你的名字，并验证它在新的Shell会话中是否可用。
2. 修改你的`PATH`变量，添加`~/bin`目录，并创建一个简单的脚本放在该目录中，验证是否可以直接运行。
3. 自定义你的命令提示符(`PS1`)，使其显示当前时间、用户名和工作目录，并使用不同的颜色。
4. 创建一个脚本，列出所有以`H`开头的环境变量及其值。
5. 使用`TERM`变量，尝试将其设置为不同的值（如`xterm`、`linux`、`vt100`），观察终端行为的变化。

## 小贴士 💡

- 使用`env`命令启动程序可以为其设置特定的环境变量：`env VAR=value command`
- 在脚本开头使用`#!/usr/bin/env bash`而不是`#!/bin/bash`可以使脚本更具可移植性
- 使用`~/.bash_aliases`文件（在`~/.bashrc`中引入）可以更好地组织你的别名
- 定期备份你的Shell配置文件，特别是在进行重大更改之前
- 使用`printenv | sort`可以按字母顺序查看所有环境变量

通过掌握Linux终端和环境变量的知识，你可以更有效地使用命令行，自定义你的工作环境，并更好地理解Linux系统的工作原理。这些知识对于系统管理、脚本编写和日常Linux使用都非常重要。
