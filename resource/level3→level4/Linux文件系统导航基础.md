# Linux文件系统导航基础 🧭

## Linux文件系统结构概述 🌳

Linux文件系统采用树状结构，从根目录（`/`）开始，向下分支到各个子目录。与Windows使用不同的驱动器盘符（如C:、D:）不同，Linux将所有内容组织在单一的目录树中。

### 主要目录及其用途

| 目录 | 描述 |
|------|------|
| `/` | **根目录**：整个文件系统的起点 |
| `/bin` | **基本命令**：存放系统启动和运行所必需的基本命令 |
| `/boot` | **启动文件**：包含内核和启动加载程序（如GRUB）所需的文件 |
| `/dev` | **设备文件**：包含设备文件，如硬盘、USB设备等 |
| `/etc` | **配置文件**：系统范围的配置文件 |
| `/home` | **用户主目录**：每个用户的个人文件和设置 |
| `/lib` | **库文件**：系统启动和运行所需的共享库文件 |
| `/media` | **可移动媒体**：自动挂载的可移动设备（如CD-ROM、USB驱动器） |
| `/mnt` | **挂载点**：用于临时挂载文件系统 |
| `/opt` | **可选软件**：第三方应用程序的安装位置 |
| `/proc` | **进程信息**：包含有关系统进程和资源的信息（虚拟文件系统） |
| `/root` | **root用户主目录**：超级用户（root）的主目录 |
| `/sbin` | **系统命令**：系统管理命令，通常只有root用户可以使用 |
| `/tmp` | **临时文件**：存放临时文件，系统重启时通常会清空 |
| `/usr` | **用户程序**：包含大多数用户程序、库、文档等 |
| `/var` | **可变数据**：包含经常变化的文件，如日志、邮件等 |

## 基本导航命令 🧭

### 1. pwd - 显示当前工作目录 📍

`pwd`（print working directory）命令显示你当前所在的目录路径。

```bash
pwd
```

输出示例：
```
/home/user/documents
```

### 2. ls - 列出目录内容 📋

`ls`命令用于列出目录中的文件和子目录。

```bash
# 基本用法 - 列出当前目录内容
ls

# 列出指定目录内容
ls /etc

# 显示详细信息
ls -l

# 显示隐藏文件
ls -a

# 显示详细信息和隐藏文件
ls -la

# 以人类可读的格式显示文件大小
ls -lh

# 按修改时间排序（最新的在前）
ls -lt

# 按文件大小排序（最大的在前）
ls -lS
```

### 3. cd - 切换目录 🚶

`cd`（change directory）命令用于在文件系统中移动。

```bash
# 进入指定目录
cd /path/to/directory

# 进入用户主目录
cd 或 cd ~

# 进入上一级目录
cd ..

# 进入上两级目录
cd ../..

# 返回上一个工作目录
cd -

# 进入相对路径目录
cd documents/projects
```

## 路径表示法 🛣️

在Linux中，有两种主要的路径表示方法：

### 1. 绝对路径

绝对路径从根目录（`/`）开始，提供了文件或目录的完整路径。

```bash
cd /home/user/documents
```

无论你当前在文件系统中的哪个位置，绝对路径都会指向同一个位置。

### 2. 相对路径

相对路径是相对于当前工作目录的路径。

```bash
# 如果当前在 /home/user
cd documents  # 进入 /home/user/documents
```

### 3. 特殊路径符号

| 符号 | 描述 |
|------|------|
| `.` | 当前目录 |
| `..` | 上一级目录 |
| `~` | 当前用户的主目录 |
| `-` | 上一个工作目录 |

```bash
# 示例
cd .        # 保持在当前目录（基本上不做任何事）
cd ./scripts  # 进入当前目录下的scripts子目录
cd ..       # 移动到上一级目录
cd ../sibling  # 移动到当前目录的同级目录"sibling"
cd ~        # 移动到用户主目录
cd ~/documents  # 移动到用户主目录下的documents目录
cd -        # 切换到上一个工作目录
```

## 文件系统导航技巧 💡

### 1. Tab补全

Tab键是Linux命令行中最有用的功能之一，它可以：
- 自动补全命令和文件名
- 减少输入错误
- 提高工作效率

```bash
# 输入部分命令或路径，然后按Tab键
cd /ho[Tab]  # 自动补全为 cd /home/
```

如果有多个可能的补全选项，按两次Tab键会显示所有可能的选项。

### 2. 通配符

通配符可以帮助你一次性操作多个文件。

```bash
# * 匹配任意数量的字符
ls *.txt      # 列出所有.txt文件
cd /usr/*/share  # 匹配/usr/下任何子目录中的share目录

# ? 匹配单个字符
ls file?.txt  # 匹配file1.txt, fileA.txt等

# [] 匹配方括号中的任意一个字符
ls file[123].txt  # 匹配file1.txt, file2.txt, file3.txt
ls [a-z]*.txt     # 匹配以小写字母开头的所有.txt文件
```

### 3. 历史命令

使用上下箭头键可以浏览之前执行过的命令。

```bash
# 搜索历史命令
Ctrl+R，然后输入搜索关键词

# 显示命令历史
history

# 执行历史命令中的第N条命令
!N

# 执行上一条命令
!!
```

## 查找文件和目录 🔍

### 1. find命令

`find`命令是一个强大的工具，用于在文件系统中搜索文件和目录。

```bash
# 基本语法
find [起始目录] [选项] [表达式]

# 在当前目录及其子目录中查找名为"file.txt"的文件
find . -name "file.txt"

# 在/home目录中查找所有.jpg文件
find /home -name "*.jpg"

# 查找大于10MB的文件
find . -size +10M

# 查找最近7天内修改过的文件
find . -mtime -7

# 查找权限为644的文件
find . -perm 644

# 查找并执行操作（删除所有.tmp文件）
find . -name "*.tmp" -exec rm {} \;
```

### 2. locate命令

`locate`命令使用预先构建的数据库来快速查找文件，比`find`命令快得多，但可能不是最新的。

```bash
# 安装locate（如果尚未安装）
sudo apt install mlocate  # Debian/Ubuntu
sudo yum install mlocate  # CentOS/RHEL

# 更新locate数据库
sudo updatedb

# 查找文件
locate filename

# 忽略大小写
locate -i filename
```

### 3. which命令

`which`命令用于查找可执行文件的位置。

```bash
which python
which ls
```

### 4. whereis命令

`whereis`命令不仅显示命令的二进制文件位置，还显示其手册页和源代码（如果有）。

```bash
whereis python
```

## 文件和目录操作 📁

### 1. 创建目录

```bash
# 创建单个目录
mkdir directory_name

# 创建多级目录
mkdir -p parent/child/grandchild

# 创建多个目录
mkdir dir1 dir2 dir3
```

### 2. 复制文件和目录

```bash
# 复制文件
cp source.txt destination.txt

# 复制目录及其内容
cp -r source_directory destination_directory

# 保留文件属性（权限、时间戳等）
cp -a source.txt destination.txt
```

### 3. 移动和重命名

```bash
# 移动文件
mv file.txt /path/to/destination/

# 重命名文件
mv oldname.txt newname.txt

# 移动并重命名
mv /path/to/file.txt /new/path/newname.txt

# 移动多个文件到目录
mv file1.txt file2.txt directory/
```

### 4. 删除文件和目录

```bash
# 删除文件
rm filename.txt

# 删除目录及其内容
rm -r directory_name

# 强制删除（不提示）
rm -f filename.txt

# 强制递归删除（危险！）
rm -rf directory_name
```

> ⚠️ **警告**：使用`rm -rf`命令时要非常小心，特别是与通配符或变量一起使用时。它可以不可逆地删除大量数据。

## 查看文件内容 📄

### 1. cat命令

显示整个文件内容。

```bash
cat filename.txt
```

### 2. less命令

分页显示文件内容，适合查看大文件。

```bash
less filename.txt
```

在`less`中的常用键：
- 空格键：向下翻页
- b：向上翻页
- q：退出
- /pattern：搜索"pattern"
- n：跳到下一个搜索结果

### 3. head命令

显示文件开头部分（默认10行）。

```bash
head filename.txt

# 显示前20行
head -n 20 filename.txt
```

### 4. tail命令

显示文件结尾部分（默认10行）。

```bash
tail filename.txt

# 显示最后20行
tail -n 20 filename.txt

# 实时监控文件变化
tail -f /var/log/syslog
```

## 文件系统导航的实用技巧 🌟

### 1. 使用别名简化导航

在`~/.bashrc`或`~/.zshrc`中添加常用目录的别名：

```bash
# 添加别名
alias docs='cd ~/Documents'
alias proj='cd ~/Projects'
alias www='cd /var/www/html'

# 使更改生效
source ~/.bashrc
```

### 2. 使用书签工具

一些工具如`pushd`和`popd`可以帮助你在目录之间快速切换：

```bash
# 将当前目录保存到堆栈
pushd .

# 切换到另一个目录
cd /var/www/html

# 返回之前保存的目录
popd
```

### 3. 使用CDPATH环境变量

设置`CDPATH`环境变量可以让你从任何位置快速访问常用目录：

```bash
# 在~/.bashrc中添加
export CDPATH=.:~:/var/www:/opt
```

这样，你可以直接使用子目录名称，而不需要完整路径。

### 4. 使用自动跳转工具

一些工具如`autojump`、`z`或`fasd`可以记住你经常访问的目录，并允许你快速跳转：

```bash
# 安装autojump（Ubuntu/Debian）
sudo apt install autojump

# 使用
j partial_directory_name
```

## 练习题 🧩

1. 如何从任何位置快速返回到你的主目录？
2. 写出至少三种查看当前目录内容的命令，并说明它们的区别。
3. 如何在不使用绝对路径的情况下，从当前目录移动到其父目录的另一个子目录？
4. 如何查找系统中所有大于100MB且在过去30天内被修改过的文件？
5. 你当前在`/home/user/projects/app`目录中，如何使用相对路径进入`/home/user/documents/notes`目录？

---

通过掌握这些Linux文件系统导航的基础知识，你将能够更加高效地在命令行中工作。记住，熟能生巧——多练习这些命令，它们很快就会成为你的第二天性！
