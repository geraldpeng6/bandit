# Bandit Level 20 → Level 21 攻略 🔄

## 关卡目标 🎯

本关卡的目标是使用位于主目录中的setuid二进制文件`suconnect`，该程序会连接到本地主机上的指定端口。我们需要先设置一个监听指定端口的服务器，该服务器会发送当前级别的密码，然后`suconnect`程序会返回下一关的密码。

## 所需知识 📚

### 网络套接字和端口 🌐

网络套接字是网络通信的端点，由IP地址和端口号组成。端口是一个16位的数字，用于标识特定的网络服务或应用程序。

### 网络监听和连接 🔌

在网络通信中，服务器通常会监听（listen）特定的端口，等待客户端连接。客户端则会主动连接（connect）到服务器的特定端口。

### 后台进程 🔙

在Linux中，可以使用`&`符号将命令放在后台运行，这样可以在同一个终端中同时执行多个命令。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit20@bandit.labs.overthewire.org -p 2220
```

密码：`GbKksEFF4yrVs6il55v6gwY5aVje5f0j`

### 2. 查看当前目录中的文件 👀

登录后，使用`ls`命令查看当前目录中的文件：

```bash
ls -la
```

输出结果应该类似于：

```
total 32
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
-rwsr-x---  1 bandit21 bandit20 12088 Oct 16  2018 suconnect
```

注意`suconnect`文件的权限：`-rwsr-x---`。`s`表示这是一个setuid程序，它会以其所有者（bandit21）的身份运行。

### 3. 尝试执行suconnect程序 🔍

我们可以尝试直接执行`suconnect`程序，看看它的用法：

```bash
./suconnect
```

输出结果可能类似于：

```
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
```

这表明我们需要提供一个端口号作为参数，程序会连接到本地主机的该端口。如果它从另一端接收到正确的密码，它会传回下一个密码。

### 4. 获取当前级别的密码 🔑

我们需要知道当前级别（bandit20）的密码，以便发送给`suconnect`程序：

```bash
cat /etc/bandit_pass/bandit20
```

输出结果应该显示bandit20的密码：

```
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```

### 5. 设置监听服务器 🌐

我们需要设置一个监听特定端口的服务器，当`suconnect`程序连接到该端口时，我们会发送当前级别的密码。我们可以使用`nc`（netcat）命令来实现这一点。

首先，选择一个未使用的端口，例如12345：

```bash
nc -l 12345 <<< "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" &
```

这个命令会在后台启动一个监听12345端口的netcat服务器，当有连接时，它会发送当前级别的密码。

### 6. 执行suconnect程序 🔄

现在我们可以执行`suconnect`程序，连接到我们设置的服务器：

```bash
./suconnect 12345
```

如果一切顺利，`suconnect`程序会连接到我们的服务器，接收到正确的密码，然后返回下一关的密码：

```
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
```

同时，我们的netcat服务器会收到并显示下一关的密码：

```
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
```

### 7. 使用另一种方法（可选）🔄

如果上述方法不起作用，我们可以尝试另一种方法：使用两个终端窗口。

在第一个终端窗口中，启动netcat服务器：

```bash
nc -l 12345
```

然后在第二个终端窗口中，执行`suconnect`程序：

```bash
./suconnect 12345
```

当`suconnect`程序连接到我们的服务器时，在第一个终端窗口中输入当前级别的密码：

```
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```

然后按Enter键发送密码。如果一切顺利，`suconnect`程序会返回下一关的密码，并显示在第一个终端窗口中。

## 命令解释 🔍

- `nc -l 12345 <<< "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" &`：
  - `nc`：netcat命令
  - `-l 12345`：监听12345端口
  - `<<< "GbKksEFF4yrVs6il55v6gwY5aVje5f0j"`：将字符串作为输入发送给netcat
  - `&`：在后台运行命令
- `./suconnect 12345`：
  - `./suconnect`：执行当前目录中的`suconnect`程序
  - `12345`：要连接的端口号

## 学习要点总结 📌

1. **网络套接字基础** - 学习了网络套接字和端口的概念
2. **服务器和客户端** - 了解了服务器监听和客户端连接的基本模型
3. **后台进程** - 学习了如何在后台运行命令
4. **netcat工具** - 了解了如何使用netcat工具设置简单的网络服务器

## 提示 💡

- 在实际工作中，netcat是一个非常有用的网络调试工具，被称为"网络的瑞士军刀"
- 端口号范围从1到65535，其中1-1023是保留给系统服务的特权端口
- 在Linux中，使用`jobs`命令可以查看当前终端的后台任务
- 使用`fg`命令可以将后台任务带回前台

## 下一步 ⏭️

使用获得的密码登录到Level 21：

```bash
ssh bandit21@bandit.labs.overthewire.org -p 2220
```

密码：`gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr`

## 相关资源 🔗

- [Netcat (nc) 命令详解](https://www.geeksforgeeks.org/nc-command-in-linux-with-examples/)
- [Linux后台任务管理](https://www.digitalocean.com/community/tutorials/how-to-use-bash-s-job-control-to-manage-foreground-and-background-processes)
- [网络套接字编程基础](https://www.tutorialspoint.com/unix_sockets/what_is_socket.htm)
