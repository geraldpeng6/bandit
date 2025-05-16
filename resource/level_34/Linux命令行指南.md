# Linux命令行指南 🐧

## 什么是命令行？ 🤔

命令行（Command Line）是一种基于文本的界面，允许用户通过输入命令来与计算机系统交互。在Linux系统中，命令行界面通常通过终端（Terminal）程序访问，用户在其中输入命令，系统执行这些命令并返回结果。

与图形用户界面（GUI）相比，命令行界面（CLI）提供了更强大、更灵活的系统控制能力，特别是对于系统管理、自动化任务和高级操作而言。掌握命令行是成为高效Linux用户和系统管理员的关键。

## 为什么学习命令行？ 🌟

1. **效率**：许多任务在命令行中执行比使用图形界面更快
2. **自动化**：可以轻松创建脚本自动执行重复任务
3. **远程管理**：可以通过SSH远程管理服务器
4. **资源占用少**：命令行工具通常比图形工具占用更少的系统资源
5. **精确控制**：提供对系统的精细控制
6. **问题排查**：在系统故障时，命令行通常仍然可用
7. **职业发展**：在IT和开发领域，命令行技能是必不可少的

## 基础命令 📚

### 导航命令

#### pwd - 显示当前工作目录

```bash
pwd
```

输出示例：
```
/home/user
```

#### ls - 列出目录内容

```bash
# 基本用法
ls

# 显示详细信息
ls -l

# 显示隐藏文件
ls -a

# 组合选项
ls -la

# 显示人类可读的文件大小
ls -lh
```

输出示例（`ls -la`）：
```
total 32
drwxr-xr-x  4 user user 4096 Jan 10 12:34 .
drwxr-xr-x 20 root root 4096 Jan  1 00:00 ..
-rw-------  1 user user  220 Jan  1 00:00 .bash_logout
-rw-------  1 user user 3526 Jan  1 00:00 .bashrc
drwxr-xr-x  2 user user 4096 Jan  5 15:43 Documents
-rw-r--r--  1 user user 8980 Jan  8 21:15 example.txt
drwxr-xr-x  2 user user 4096 Jan  3 10:21 Pictures
```

#### cd - 更改目录

```bash
# 进入指定目录
cd /path/to/directory

# 进入用户主目录
cd 或 cd ~

# 进入上级目录
cd ..

# 进入上次访问的目录
cd -
```

### 文件操作命令

#### cat - 查看文件内容

```bash
# 显示文件内容
cat filename.txt

# 显示多个文件
cat file1.txt file2.txt

# 显示行号
cat -n filename.txt
```

#### less - 分页查看文件

```bash
less filename.txt
```

在`less`中的常用快捷键：
- `空格键`：向下翻页
- `b`：向上翻页
- `/pattern`：搜索"pattern"
- `n`：重复上次搜索
- `q`：退出

#### head/tail - 查看文件开头/结尾

```bash
# 显示文件前10行
head filename.txt

# 显示文件前20行
head -n 20 filename.txt

# 显示文件最后10行
tail filename.txt

# 显示文件最后20行
tail -n 20 filename.txt

# 实时查看文件更新
tail -f logfile.txt
```

#### touch - 创建空文件或更新时间戳

```bash
# 创建新文件
touch newfile.txt

# 更新已存在文件的时间戳
touch existingfile.txt
```

#### mkdir - 创建目录

```bash
# 创建单个目录
mkdir directory

# 创建多个目录
mkdir dir1 dir2 dir3

# 创建嵌套目录
mkdir -p parent/child/grandchild
```

#### cp - 复制文件或目录

```bash
# 复制文件
cp source.txt destination.txt

# 复制目录及其内容
cp -r sourcedir destinationdir

# 保留文件属性
cp -a source.txt destination.txt
```

#### mv - 移动或重命名文件

```bash
# 移动文件
mv file.txt /path/to/destination/

# 重命名文件
mv oldname.txt newname.txt

# 移动并重命名
mv /path/oldname.txt /newpath/newname.txt
```

#### rm - 删除文件或目录

```bash
# 删除文件
rm filename.txt

# 删除目录及其内容
rm -r directory

# 强制删除（不提示）
rm -f filename.txt

# 强制递归删除
rm -rf directory
```

⚠️ **警告**：`rm -rf`命令非常危险，特别是与通配符或根目录一起使用时。使用前务必仔细检查。

#### ln - 创建链接

```bash
# 创建硬链接
ln source.txt hardlink

# 创建软链接（符号链接）
ln -s source.txt symlink
```

### 文件内容操作

#### grep - 搜索文本

```bash
# 在文件中搜索模式
grep "pattern" filename.txt

# 递归搜索目录
grep -r "pattern" directory/

# 显示匹配行号
grep -n "pattern" filename.txt

# 忽略大小写
grep -i "pattern" filename.txt

# 显示不匹配的行
grep -v "pattern" filename.txt
```

#### find - 查找文件

```bash
# 按名称查找文件
find /path -name "filename"

# 使用通配符
find /path -name "*.txt"

# 按类型查找（f=文件，d=目录）
find /path -type f
find /path -type d

# 按大小查找
find /path -size +10M  # 大于10MB的文件
find /path -size -1M   # 小于1MB的文件

# 按修改时间查找
find /path -mtime -7   # 7天内修改的文件
```

#### sort - 排序文本

```bash
# 基本排序
sort filename.txt

# 数字排序
sort -n numbers.txt

# 反向排序
sort -r filename.txt

# 按特定字段排序
sort -k 2 data.txt
```

#### wc - 计数

```bash
# 计算行数、单词数和字节数
wc filename.txt

# 只计算行数
wc -l filename.txt

# 只计算单词数
wc -w filename.txt

# 只计算字节数
wc -c filename.txt
```

### 系统信息命令

#### uname - 显示系统信息

```bash
# 显示所有信息
uname -a

# 显示内核名称
uname -s

# 显示内核版本
uname -r
```

#### df - 显示磁盘空间使用情况

```bash
# 显示所有文件系统
df

# 使用人类可读格式
df -h
```

#### free - 显示内存使用情况

```bash
# 基本用法
free

# 使用人类可读格式
free -h

# 以MB为单位显示
free -m
```

#### top - 显示进程信息

```bash
top
```

在`top`中的常用快捷键：
- `q`：退出
- `k`：杀死进程
- `r`：重新设置进程优先级
- `f`：配置显示字段
- `1`：显示所有CPU核心

#### ps - 显示进程状态

```bash
# 显示当前用户的进程
ps

# 显示所有进程
ps -e 或 ps -A

# 显示详细信息
ps -ef

# 显示进程树
ps -ejH
```

### 用户和权限命令

#### whoami - 显示当前用户

```bash
whoami
```

#### id - 显示用户ID和组ID

```bash
id
id username
```

#### chmod - 更改文件权限

```bash
# 使用数字模式
chmod 755 filename  # rwxr-xr-x

# 使用符号模式
chmod u+x filename  # 给所有者添加执行权限
chmod g-w filename  # 移除组的写入权限
chmod o=r filename  # 设置其他用户只读权限
chmod a+x filename  # 给所有人添加执行权限
```

#### chown - 更改文件所有者

```bash
# 更改所有者
chown user filename

# 更改所有者和组
chown user:group filename
```

#### sudo - 以超级用户身份执行命令

```bash
sudo command
```

## 管道和重定向 📊

### 重定向

#### 输出重定向

```bash
# 将输出重定向到文件（覆盖）
command > file.txt

# 将输出追加到文件
command >> file.txt

# 将错误重定向到文件
command 2> errors.txt

# 将标准输出和错误都重定向到文件
command > file.txt 2>&1
# 或在新版bash中
command &> file.txt
```

#### 输入重定向

```bash
# 从文件读取输入
command < file.txt
```

### 管道

管道（`|`）将一个命令的输出作为另一个命令的输入：

```bash
# 将ls的输出传递给grep
ls -la | grep ".txt"

# 多个管道
cat file.txt | grep "pattern" | sort | uniq
```

## 文本编辑器 ✏️

### nano - 简单的文本编辑器

```bash
# 打开或创建文件
nano filename.txt
```

常用快捷键（`^`表示Ctrl键）：
- `^G`：显示帮助
- `^O`：保存文件
- `^X`：退出
- `^K`：剪切行
- `^U`：粘贴文本
- `^W`：搜索文本

### vim - 高级文本编辑器

```bash
# 打开或创建文件
vim filename.txt
```

vim有三种主要模式：
1. **普通模式**：默认模式，用于导航和执行命令
2. **插入模式**：用于编辑文本，按`i`进入
3. **命令模式**：用于执行保存、退出等操作，按`:`进入

常用命令：
- `:w`：保存文件
- `:q`：退出
- `:wq`或`ZZ`：保存并退出
- `:q!`：不保存强制退出
- `/pattern`：搜索"pattern"
- `n`：下一个匹配
- `dd`：删除行
- `yy`：复制行
- `p`：粘贴

## 网络命令 🌐

### ping - 测试网络连接

```bash
# 基本用法
ping example.com

# 指定次数
ping -c 4 example.com
```

### ifconfig/ip - 显示网络接口信息

```bash
# 使用ifconfig（较旧）
ifconfig

# 使用ip（较新）
ip addr show
```

### netstat/ss - 显示网络连接

```bash
# 使用netstat（较旧）
netstat -tuln

# 使用ss（较新）
ss -tuln
```

### wget - 下载文件

```bash
# 下载文件
wget https://example.com/file.zip

# 指定输出文件名
wget -O output.zip https://example.com/file.zip

# 后台下载
wget -b https://example.com/largefile.iso
```

### curl - 传输数据

```bash
# 获取网页内容
curl https://example.com

# 下载文件
curl -o file.zip https://example.com/file.zip

# 发送POST请求
curl -X POST -d "data" https://example.com/api
```

### ssh - 安全远程登录

```bash
# 连接到远程服务器
ssh username@hostname

# 指定端口
ssh -p 2222 username@hostname

# 使用密钥文件
ssh -i key.pem username@hostname
```

## 进程管理 ⚙️

### ps - 查看进程

```bash
# 显示所有进程
ps aux
```

### kill - 终止进程

```bash
# 通过PID终止进程
kill PID

# 强制终止
kill -9 PID
```

### jobs - 显示后台作业

```bash
jobs
```

### bg - 将作业放到后台

```bash
bg %job_number
```

### fg - 将后台作业放到前台

```bash
fg %job_number
```

### nohup - 使命令在退出终端后继续运行

```bash
nohup command &
```

## 压缩和归档 📦

### tar - 归档文件

```bash
# 创建归档
tar -cf archive.tar file1 file2 directory

# 解压归档
tar -xf archive.tar

# 创建gzip压缩归档
tar -czf archive.tar.gz directory

# 解压gzip压缩归档
tar -xzf archive.tar.gz

# 创建bzip2压缩归档
tar -cjf archive.tar.bz2 directory

# 解压bzip2压缩归档
tar -xjf archive.tar.bz2
```

### zip/unzip - 创建和解压zip文件

```bash
# 创建zip文件
zip -r archive.zip directory

# 解压zip文件
unzip archive.zip
```

### gzip/gunzip - gzip压缩

```bash
# 压缩文件
gzip file.txt  # 创建file.txt.gz并删除原文件

# 解压文件
gunzip file.txt.gz
```

## 实用技巧 💡

### 命令历史

```bash
# 显示命令历史
history

# 执行历史中的特定命令
!123  # 执行历史中编号为123的命令

# 执行上一个命令
!!

# 执行上一个以特定字符开头的命令
!ls
```

### 命令替换

```bash
# 使用反引号
echo "Today is `date`"

# 使用$()（推荐）
echo "Today is $(date)"
```

### 别名

```bash
# 创建临时别名
alias ll='ls -la'

# 在~/.bashrc中添加永久别名
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc
```

### Tab补全

按Tab键可以：
- 补全命令和文件名
- 显示可能的补全选项（按两次Tab）

### 快捷键

- `Ctrl+C`：中断当前命令
- `Ctrl+Z`：暂停当前命令，将其放入后台
- `Ctrl+D`：退出当前shell
- `Ctrl+L`：清屏
- `Ctrl+A`：移动光标到行首
- `Ctrl+E`：移动光标到行尾
- `Ctrl+U`：删除光标前的所有字符
- `Ctrl+K`：删除光标后的所有字符
- `Ctrl+W`：删除光标前的单词
- `Ctrl+R`：搜索命令历史

## 脚本编写基础 📜

### 创建和执行脚本

```bash
# 创建脚本文件
nano myscript.sh

# 添加shebang行和命令
#!/bin/bash
echo "Hello, World!"
date
whoami

# 设置执行权限
chmod +x myscript.sh

# 执行脚本
./myscript.sh
```

### 变量

```bash
# 定义变量
NAME="John"

# 使用变量
echo "Hello, $NAME"

# 命令结果赋值给变量
CURRENT_DIR=$(pwd)
echo "You are in $CURRENT_DIR"
```

### 条件语句

```bash
#!/bin/bash
# if语句
if [ "$1" = "hello" ]; then
    echo "Hello to you too!"
elif [ "$1" = "bye" ]; then
    echo "Goodbye!"
else
    echo "I don't understand"
fi
```

### 循环

```bash
#!/bin/bash
# for循环
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# while循环
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

## 常见问题解答 ❓

### 问题：如何查找特定命令的用法？

**解决方案**：
```bash
# 使用man命令查看手册
man command

# 使用--help选项
command --help

# 使用info命令
info command
```

### 问题：如何查找文件中的特定文本？

**解决方案**：
```bash
# 在单个文件中搜索
grep "text" filename

# 在目录中递归搜索
grep -r "text" directory/

# 忽略大小写
grep -i "text" filename
```

### 问题：如何在不知道确切名称的情况下查找文件？

**解决方案**：
```bash
# 使用find命令
find /path -name "*partial*"

# 使用locate命令（需要更新数据库）
updatedb
locate partial
```

## 练习题 🎯

1. 创建一个名为`practice`的目录，在其中创建三个空文件，然后列出目录内容。
2. 编写一个脚本，接受一个目录路径作为参数，并计算该目录中所有文本文件的总行数。
3. 使用管道和过滤命令，找出系统中占用内存最多的5个进程。
4. 创建一个脚本，自动备份指定目录，将其压缩并添加日期戳。
5. 使用命令行下载一个网页，并提取其中的所有链接。

## 小贴士 💡

- 使用`man`命令学习新命令的用法
- 创建常用命令的别名，提高效率
- 学习使用Tab补全功能，减少输入错误
- 使用`history`命令和`Ctrl+R`快速找到之前使用过的命令
- 使用`&&`连接命令，只有前一个命令成功时才执行后一个命令
- 使用`||`连接命令，只有前一个命令失败时才执行后一个命令
- 定期备份重要的配置文件
- 在执行危险命令前，先使用`echo`测试命令参数
- 学习一种文本编辑器（如vim或nano）的基本用法
- 使用`script`命令记录终端会话，便于日后参考

通过掌握这些Linux命令行基础知识，你将能够更高效地使用Linux系统，自动化日常任务，并为更高级的Linux技能打下坚实的基础。命令行是Linux强大功能的入口，投入时间学习它将会带来丰厚的回报。
