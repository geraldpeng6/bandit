# Bash脚本编程指南 📜

## 什么是Bash脚本？ 🤔

Bash（Bourne Again SHell）脚本是一种用于自动化任务的脚本语言，它是Linux和Unix系统中最常用的shell脚本语言。Bash脚本可以执行命令、处理文件、操作文本、控制流程等，使得复杂的任务可以自动化执行，提高工作效率。

## 基础知识 🌱

### 创建第一个脚本

创建一个简单的Bash脚本非常容易。首先，使用文本编辑器创建一个文件，例如`hello.sh`：

```bash
#!/bin/bash
# 这是一个简单的Bash脚本
echo "Hello, World!"
```

第一行`#!/bin/bash`被称为"shebang"或"hashbang"，它告诉系统这个脚本应该使用哪个解释器来执行。

### 执行脚本

要执行脚本，首先需要给它添加执行权限：

```bash
chmod +x hello.sh
```

然后，可以通过以下方式执行脚本：

```bash
./hello.sh
```

或者，也可以直接使用bash命令执行脚本，而不需要执行权限：

```bash
bash hello.sh
```

## 变量 📦

### 定义和使用变量

在Bash中，可以通过以下方式定义变量：

```bash
name="John"
age=30
```

注意，等号两边不能有空格。

使用变量时，需要在变量名前加上`$`符号：

```bash
echo "My name is $name and I am $age years old."
```

也可以使用`${变量名}`的形式，这在变量名与其他文本连接时特别有用：

```bash
echo "My name is ${name}Doe."
```

### 命令替换

可以将命令的输出赋值给变量：

```bash
current_date=$(date)
echo "Current date and time: $current_date"

# 或者使用反引号（不推荐，因为嵌套时容易混淆）
current_time=`date +%H:%M:%S`
echo "Current time: $current_time"
```

### 环境变量

Bash有许多预定义的环境变量，如`$HOME`、`$PATH`、`$USER`等：

```bash
echo "Home directory: $HOME"
echo "Current user: $USER"
echo "Shell: $SHELL"
```

### 特殊变量

Bash还有一些特殊变量，用于脚本的参数和状态：

| 变量 | 描述 |
|------|------|
| `$0` | 脚本名称 |
| `$1`, `$2`, ... | 脚本的第1个、第2个参数，以此类推 |
| `$#` | 参数的数量 |
| `$@` | 所有参数（作为单独的单词） |
| `$*` | 所有参数（作为一个单词） |
| `$?` | 上一个命令的退出状态（0表示成功，非0表示失败） |
| `$$` | 当前脚本的进程ID |

示例：

```bash
#!/bin/bash
echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Number of arguments: $#"
echo "All arguments: $@"
echo "Process ID: $$"
```

## 输入和输出 🔄

### 用户输入

使用`read`命令可以获取用户输入：

```bash
echo "What is your name?"
read name
echo "Hello, $name!"

# 带提示的读取
read -p "Enter your age: " age
echo "You are $age years old."

# 读取多个变量
read -p "Enter your first and last name: " first_name last_name
echo "Your first name is $first_name and your last name is $last_name."
```

### 输出重定向

可以将命令的输出重定向到文件：

```bash
# 覆盖文件内容
echo "Hello, World!" > output.txt

# 追加到文件
echo "Another line." >> output.txt

# 重定向标准错误
command 2> error.log

# 同时重定向标准输出和标准错误
command > output.txt 2>&1
```

### 输入重定向

可以从文件读取输入：

```bash
# 从文件读取输入
cat < input.txt

# 使用here文档（多行字符串）
cat << EOF
This is a multi-line
string that will be
passed to the cat command.
EOF
```

## 条件语句 🔀

### if语句

基本的if语句语法：

```bash
if [ condition ]; then
    # 如果条件为真，执行这里的命令
    commands
elif [ another_condition ]; then
    # 如果另一个条件为真，执行这里的命令
    commands
else
    # 如果所有条件都为假，执行这里的命令
    commands
fi
```

条件测试可以使用`test`命令或方括号`[ ]`（它们是等价的）：

```bash
# 检查文件是否存在
if [ -f "file.txt" ]; then
    echo "file.txt exists."
fi

# 检查目录是否存在
if [ -d "/tmp" ]; then
    echo "/tmp directory exists."
fi

# 检查变量是否为空
if [ -z "$var" ]; then
    echo "Variable is empty."
fi

# 检查两个字符串是否相等
if [ "$str1" = "$str2" ]; then
    echo "Strings are equal."
fi

# 数值比较
if [ "$num1" -eq "$num2" ]; then
    echo "Numbers are equal."
fi
```

### 文件测试操作符

| 操作符 | 描述 |
|--------|------|
| `-e file` | 如果文件存在，则为真 |
| `-f file` | 如果文件存在且是常规文件，则为真 |
| `-d file` | 如果文件存在且是目录，则为真 |
| `-r file` | 如果文件存在且可读，则为真 |
| `-w file` | 如果文件存在且可写，则为真 |
| `-x file` | 如果文件存在且可执行，则为真 |
| `-s file` | 如果文件存在且大小大于0，则为真 |

### 字符串比较操作符

| 操作符 | 描述 |
|--------|------|
| `str1 = str2` | 如果字符串相等，则为真 |
| `str1 != str2` | 如果字符串不相等，则为真 |
| `-z str` | 如果字符串长度为0，则为真 |
| `-n str` | 如果字符串长度不为0，则为真 |

### 数值比较操作符

| 操作符 | 描述 |
|--------|------|
| `num1 -eq num2` | 如果数值相等，则为真 |
| `num1 -ne num2` | 如果数值不相等，则为真 |
| `num1 -lt num2` | 如果num1小于num2，则为真 |
| `num1 -le num2` | 如果num1小于或等于num2，则为真 |
| `num1 -gt num2` | 如果num1大于num2，则为真 |
| `num1 -ge num2` | 如果num1大于或等于num2，则为真 |

### 逻辑操作符

| 操作符 | 描述 |
|--------|------|
| `! expr` | 如果表达式为假，则为真 |
| `expr1 -a expr2` | 如果expr1和expr2都为真，则为真（AND） |
| `expr1 -o expr2` | 如果expr1或expr2为真，则为真（OR） |

### 现代条件测试

Bash还提供了`[[ ]]`双方括号形式，它提供了更多功能和更少的陷阱：

```bash
# 使用正则表达式匹配
if [[ "$string" =~ ^[0-9]+$ ]]; then
    echo "String contains only digits."
fi

# 使用&&和||进行逻辑操作
if [[ -f "file.txt" && -r "file.txt" ]]; then
    echo "file.txt exists and is readable."
fi
```

## 循环语句 🔄

### for循环

基本的for循环语法：

```bash
for variable in list; do
    commands
done
```

示例：

```bash
# 遍历列表
for name in John Mary Steve; do
    echo "Hello, $name!"
done

# 遍历文件
for file in *.txt; do
    echo "Processing $file..."
done

# 遍历数字范围
for i in {1..5}; do
    echo "Number: $i"
done

# C风格的for循环
for ((i=1; i<=5; i++)); do
    echo "Count: $i"
done
```

### while循环

while循环在条件为真时重复执行命令：

```bash
while [ condition ]; do
    commands
done
```

示例：

```bash
# 计数器
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done

# 读取文件的每一行
while read line; do
    echo "Line: $line"
done < input.txt
```

### until循环

until循环在条件为假时重复执行命令（与while相反）：

```bash
until [ condition ]; do
    commands
done
```

示例：

```bash
count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    ((count++))
done
```

### 循环控制

- `break`：跳出当前循环
- `continue`：跳过当前迭代，继续下一次迭代
- `exit`：退出脚本

```bash
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        echo "Skipping 5"
        continue
    fi
    if [ $i -eq 8 ]; then
        echo "Breaking at 8"
        break
    fi
    echo "Number: $i"
done
```

## 函数 🧩

### 定义和调用函数

函数的定义和调用：

```bash
# 定义函数
function greet {
    echo "Hello, $1!"
}

# 另一种定义方式
say_goodbye() {
    echo "Goodbye, $1!"
}

# 调用函数
greet "John"
say_goodbye "Mary"
```

### 返回值

函数可以使用`return`语句返回一个数值（0-255）：

```bash
function check_file {
    if [ -f "$1" ]; then
        return 0  # 成功
    else
        return 1  # 失败
    fi
}

check_file "example.txt"
if [ $? -eq 0 ]; then
    echo "File exists."
else
    echo "File does not exist."
fi
```

对于更复杂的返回值，可以使用全局变量或命令替换：

```bash
function get_sum {
    result=$(($1 + $2))
    echo $result
}

sum=$(get_sum 5 3)
echo "The sum is: $sum"
```

### 局部变量

使用`local`关键字可以定义局部变量，它们只在函数内部可见：

```bash
function test_scope {
    local local_var="I am local"
    global_var="I am global"
    echo "Inside function: $local_var, $global_var"
}

test_scope
echo "Outside function: $global_var"
echo "Outside function: $local_var"  # 这将不会显示任何内容，因为local_var是局部变量
```

## 数组 📚

### 定义和访问数组

Bash支持索引数组和关联数组（类似于其他语言中的字典或哈希表）：

```bash
# 定义索引数组
fruits=("Apple" "Banana" "Cherry")

# 访问数组元素
echo "First fruit: ${fruits[0]}"
echo "Second fruit: ${fruits[1]}"

# 获取所有元素
echo "All fruits: ${fruits[@]}"

# 获取数组长度
echo "Number of fruits: ${#fruits[@]}"

# 定义关联数组（Bash 4.0+）
declare -A user
user["name"]="John"
user["age"]=30

# 访问关联数组元素
echo "User name: ${user[name]}"
echo "User age: ${user[age]}"

# 获取所有键
echo "All keys: ${!user[@]}"
```

### 数组操作

```bash
# 添加元素
fruits+=("Dragon fruit")

# 遍历数组
for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done

# 遍历带索引的数组
for i in "${!fruits[@]}"; do
    echo "Index $i: ${fruits[$i]}"
done

# 删除元素
unset fruits[1]

# 切片
echo "Slice: ${fruits[@]:1:2}"  # 从索引1开始，取2个元素
```

## 字符串操作 📝

### 字符串长度

```bash
str="Hello, World!"
echo "Length: ${#str}"  # 输出: 13
```

### 子字符串提取

```bash
str="Hello, World!"
echo "Substring: ${str:7:5}"  # 输出: World
```

### 字符串替换

```bash
str="Hello, World!"
echo "Replace first: ${str/o/O}"  # 输出: HellO, World!
echo "Replace all: ${str//o/O}"   # 输出: HellO, WOrld!
```

### 字符串匹配和删除

```bash
filename="document.txt"
echo "Remove suffix: ${filename%.txt}"  # 输出: document
echo "Remove prefix: ${filename#doc}"   # 输出: ument.txt
```

## 错误处理 🚫

### 检查命令执行状态

每个命令执行后都会返回一个状态码，0表示成功，非0表示失败：

```bash
command
if [ $? -ne 0 ]; then
    echo "Command failed."
    exit 1
fi
```

### 设置错误处理选项

```bash
# 遇到错误时退出脚本
set -e

# 使用未定义的变量时报错
set -u

# 管道中的任何命令失败时报错
set -o pipefail

# 调试模式，显示执行的每个命令
set -x
```

### 捕获错误

```bash
# 定义错误处理函数
function handle_error {
    echo "Error occurred at line $1"
    exit 1
}

# 设置错误处理陷阱
trap 'handle_error $LINENO' ERR
```

## 调试技巧 🐞

### 使用echo进行调试

```bash
echo "Debug: variable=$variable"
```

### 使用set -x进行跟踪

```bash
set -x  # 开始跟踪
commands
set +x  # 停止跟踪
```

### 使用trap进行调试

```bash
trap 'echo "Line $LINENO: $BASH_COMMAND"' DEBUG
```

## 最佳实践 ✨

1. **使用有意义的变量名**：变量名应该清晰地表示其用途。

2. **添加注释**：解释复杂的逻辑和非显而易见的代码。

3. **使用函数组织代码**：将相关功能组织到函数中，提高可读性和可维护性。

4. **处理错误**：检查命令的返回状态，并适当处理错误。

5. **使用引号**：始终将变量和命令替换用双引号括起来，以避免空格和特殊字符的问题。

6. **使用`set -e`**：在脚本开头使用`set -e`，使脚本在遇到错误时自动退出。

7. **使用`set -u`**：使用`set -u`检测未定义的变量。

8. **使用`[[ ]]`而不是`[ ]`**：双方括号提供了更多功能和更少的陷阱。

9. **使用长选项**：使用`--long-option`而不是`-l`，使脚本更易读。

10. **使用`$(...)`而不是反引号**：命令替换使用`$(...)`更清晰，特别是在嵌套时。

## 实用示例 📋

### 备份脚本

```bash
#!/bin/bash
# 简单的备份脚本

# 配置
source_dir="/path/to/source"
backup_dir="/path/to/backup"
date_format=$(date +%Y%m%d_%H%M%S)
backup_file="backup_${date_format}.tar.gz"

# 创建备份
echo "Creating backup of $source_dir..."
tar -czf "${backup_dir}/${backup_file}" "$source_dir"

# 检查是否成功
if [ $? -eq 0 ]; then
    echo "Backup created successfully: ${backup_dir}/${backup_file}"
else
    echo "Backup failed!"
    exit 1
fi

# 删除旧备份（保留最新的5个）
cd "$backup_dir" || exit 1
ls -t backup_*.tar.gz | tail -n +6 | xargs -r rm

echo "Backup process completed."
```

### 系统监控脚本

```bash
#!/bin/bash
# 简单的系统监控脚本

log_file="/var/log/system_monitor.log"

# 记录日期和时间
echo "=== System Monitor Report: $(date) ===" >> "$log_file"

# 检查磁盘使用情况
echo "Disk Usage:" >> "$log_file"
df -h | grep -v "tmpfs" >> "$log_file"

# 检查内存使用情况
echo -e "\nMemory Usage:" >> "$log_file"
free -h >> "$log_file"

# 检查负载平均值
echo -e "\nLoad Average:" >> "$log_file"
uptime >> "$log_file"

# 检查最近的登录
echo -e "\nRecent Logins:" >> "$log_file"
last | head -5 >> "$log_file"

echo -e "\nMonitoring completed.\n" >> "$log_file"
```

## 练习题 🧩

1. 编写一个脚本，接受一个目录路径作为参数，并列出该目录中的所有文件（不包括目录）。

2. 编写一个脚本，计算1到给定数字的和。

3. 编写一个脚本，检查给定的文件是否存在，如果存在，显示其大小；如果不存在，创建一个空文件。

4. 编写一个脚本，接受任意数量的数字作为参数，并计算它们的平均值。

5. 编写一个脚本，查找并删除指定目录中超过30天未修改的文件。

---

通过掌握Bash脚本编程，你可以自动化各种任务，提高工作效率，并解决复杂的系统管理问题。无论是简单的文件操作还是复杂的系统监控，Bash脚本都是Linux/Unix用户的强大工具。
