# Linux命令行基础 🐧

## 什么是命令行？ 🤔

命令行（Command Line）是一种基于文本的界面，用户通过输入命令来操作计算机系统。在Linux系统中，命令行界面也被称为终端（Terminal）或Shell。与图形用户界面（GUI）相比，命令行提供了更强大、更灵活的系统控制能力，是Linux系统管理和操作的核心工具。

## 为什么要学习命令行？ 🎯

1. **效率更高** - 许多任务在命令行下执行比图形界面更快
2. **自动化能力** - 可以编写脚本自动执行重复性任务
3. **远程管理** - 通过SSH远程管理服务器时必须使用命令行
4. **系统资源占用少** - 命令行界面比图形界面占用更少的系统资源
5. **强大的管道和重定向功能** - 可以组合多个命令完成复杂任务

## 基本命令结构 📝

Linux命令通常遵循以下格式：

```
命令名 [选项] [参数]
```

- **命令名**：要执行的命令
- **选项**：修改命令行为的标志，通常以`-`（短选项）或`--`（长选项）开头
- **参数**：命令操作的对象，如文件名、目录名等

例如：
```bash
ls -la /home/user
```
这里，`ls`是命令名，`-la`是选项（`-l`和`-a`的组合），`/home/user`是参数。

## 必知必会的基础命令 💡

### 1. 文件和目录操作 📂

#### ls - 列出目录内容
```bash
ls              # 列出当前目录的文件和目录
ls -l           # 以长格式显示（包含权限、所有者等详细信息）
ls -a           # 显示所有文件，包括隐藏文件（以.开头的文件）
ls -la          # 组合使用，显示所有文件的详细信息
ls -lh          # 以人类可读的格式显示文件大小
```

#### cd - 切换目录
```bash
cd /path/to/directory    # 切换到指定目录
cd ~                     # 切换到当前用户的主目录
cd ..                    # 切换到上一级目录
cd -                     # 切换到上一个工作目录
```

#### pwd - 显示当前工作目录
```bash
pwd             # 显示当前所在的完整路径
```

#### mkdir - 创建目录
```bash
mkdir directory_name     # 创建一个新目录
mkdir -p dir1/dir2/dir3  # 创建多级目录（如果父目录不存在则创建）
```

#### rm - 删除文件或目录
```bash
rm file.txt              # 删除文件
rm -r directory          # 递归删除目录及其内容
rm -f file.txt           # 强制删除文件（不提示）
rm -rf directory         # 强制递归删除目录（危险命令，慎用！）
```

#### cp - 复制文件或目录
```bash
cp source.txt destination.txt    # 复制文件
cp -r source_dir destination_dir # 递归复制目录
```

#### mv - 移动或重命名文件/目录
```bash
mv old_name.txt new_name.txt     # 重命名文件
mv file.txt /path/to/directory/  # 移动文件到指定目录
```

#### touch - 创建空文件或更新文件时间戳
```bash
touch new_file.txt       # 创建一个空文件，如果文件已存在则更新时间戳
```

### 2. 文件内容查看和编辑 📄

#### cat - 查看文件内容
```bash
cat file.txt             # 显示文件全部内容
cat -n file.txt          # 显示行号
```

#### less - 分页查看文件内容
```bash
less file.txt            # 分页查看文件，可上下滚动
# 按q退出，按空格翻页，按/进行搜索
```

#### head - 查看文件开头部分
```bash
head file.txt            # 显示文件前10行
head -n 20 file.txt      # 显示文件前20行
```

#### tail - 查看文件结尾部分
```bash
tail file.txt            # 显示文件最后10行
tail -n 20 file.txt      # 显示文件最后20行
tail -f log_file.txt     # 实时查看文件更新（常用于日志监控）
```

#### grep - 文本搜索
```bash
grep "pattern" file.txt          # 在文件中搜索指定模式
grep -i "pattern" file.txt       # 忽略大小写搜索
grep -r "pattern" directory      # 递归搜索目录中的所有文件
grep -v "pattern" file.txt       # 显示不匹配的行
```

#### nano/vim - 文本编辑器
```bash
nano file.txt            # 使用nano编辑器（对初学者友好）
vim file.txt             # 使用vim编辑器（功能强大但学习曲线陡峭）
```

### 3. 系统信息和管理 🖥️

#### uname - 显示系统信息
```bash
uname -a                 # 显示所有系统信息
```

#### whoami - 显示当前用户
```bash
whoami                   # 显示当前登录的用户名
```

#### date - 显示或设置系统日期和时间
```bash
date                     # 显示当前日期和时间
```

#### df - 显示磁盘空间使用情况
```bash
df -h                    # 以人类可读格式显示磁盘使用情况
```

#### free - 显示内存使用情况
```bash
free -h                  # 以人类可读格式显示内存使用情况
```

#### top - 显示进程信息
```bash
top                      # 实时显示系统进程信息
# 按q退出
```

#### ps - 显示进程状态
```bash
ps aux                   # 显示所有进程详细信息
```

#### kill - 终止进程
```bash
kill PID                 # 终止指定PID的进程
kill -9 PID              # 强制终止进程
```

### 4. 用户和权限管理 👥

#### chmod - 修改文件权限
```bash
chmod 755 file.txt       # 设置文件权限为rwxr-xr-x
chmod +x script.sh       # 添加执行权限
```

#### chown - 修改文件所有者
```bash
chown user:group file.txt    # 修改文件的所有者和组
```

#### sudo - 以超级用户权限执行命令
```bash
sudo command             # 以管理员权限执行命令
```

### 5. 网络相关命令 🌐

#### ping - 测试网络连接
```bash
ping google.com          # 测试与指定主机的连接
ping -c 4 google.com     # 发送4个数据包后停止
```

#### ifconfig/ip - 显示或配置网络接口
```bash
ifconfig                 # 显示所有网络接口信息
ip addr show             # 现代Linux系统推荐使用ip命令
```

#### wget - 下载文件
```bash
wget URL                 # 从指定URL下载文件
```

#### ssh - 安全远程登录
```bash
ssh user@hostname        # 连接到远程主机
```

## 命令行技巧 💪

### 1. 命令历史和搜索
- 使用上下箭头键浏览之前使用过的命令
- `history` 命令查看命令历史
- `Ctrl+R` 搜索命令历史

### 2. Tab补全
- 按Tab键自动补全命令、文件名和目录名
- 连按两次Tab显示所有可能的补全选项

### 3. 管道和重定向
```bash
# 管道：将一个命令的输出作为另一个命令的输入
command1 | command2

# 重定向：将命令输出保存到文件
command > file.txt       # 覆盖文件内容
command >> file.txt      # 追加到文件末尾

# 从文件输入
command < file.txt
```

### 4. 通配符
- `*` 匹配任意多个字符
- `?` 匹配单个字符
- `[abc]` 匹配方括号中的任意一个字符

例如：
```bash
ls *.txt                 # 列出所有.txt文件
rm file?.txt             # 删除file1.txt, file2.txt等
```

### 5. 命令别名
```bash
alias ll='ls -la'        # 创建命令别名
```

## 常见问题与解决方案 ❓

### 1. 命令未找到
```
command not found
```
解决方法：
- 检查命令拼写是否正确
- 确认命令是否已安装
- 检查命令是否在PATH环境变量中

### 2. 权限被拒绝
```
Permission denied
```
解决方法：
- 使用`sudo`以管理员权限运行命令
- 使用`chmod`修改文件权限

### 3. 文件或目录不存在
```
No such file or directory
```
解决方法：
- 检查路径是否正确
- 使用`ls`确认文件是否存在
- 检查文件名大小写（Linux区分大小写）

## 练习题 🧩

1. 如何创建一个名为`test`的目录，并在其中创建一个名为`hello.txt`的文件？
2. 如何查看`/var/log/syslog`文件的最后20行？
3. 如何查找系统中所有名称包含"conf"的文件？
4. 如何将`ls -la`命令的输出保存到名为`directory_content.txt`的文件中？
5. 如何查看当前系统的内存使用情况？

---

通过掌握这些基础命令，你已经可以开始在Linux系统中进行基本的操作了。随着经验的积累，你将能够更加高效地使用命令行完成各种任务！记住，最好的学习方法是实践，所以请尽可能多地在实际环境中使用这些命令。🚀
