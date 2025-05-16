# Bandit Level 14 → Level 15 攻略 🌐

## 关卡目标 🎯

本关卡的目标是通过将当前级别的密码提交到本地主机（localhost）的30000端口来获取下一关的密码。

## 所需知识 📚

### 网络通信基础 🌐

在Linux中，可以使用多种工具与网络服务进行通信：
- `telnet`：一种早期的网络协议，用于通过网络提供双向交互式文本通信
- `nc`（netcat）：一个功能强大的网络工具，被称为"网络的瑞士军刀"
- `curl`：用于传输数据的命令行工具

### 端口和服务 🔌

端口是网络通信的端点，每个端口都可以运行一个服务。常见的端口包括：
- 22：SSH
- 80：HTTP
- 443：HTTPS

在本关卡中，我们需要连接到端口30000，这是一个自定义端口，运行着一个特殊的服务。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

密码：`4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e`

### 2. 获取当前级别的密码 🔑

我们已经知道当前级别（bandit14）的密码，但为了确保我们使用正确的密码，可以从密码文件中读取：

```bash
cat /etc/bandit_pass/bandit14
```

输出结果应该显示bandit14的密码：

```
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

### 3. 使用netcat连接到本地主机的30000端口 🔌

我们可以使用`nc`（netcat）命令连接到本地主机的30000端口：

```bash
nc localhost 30000
```

连接成功后，我们需要输入当前级别的密码：

```
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

输入密码后，服务器应该返回下一关的密码：

```
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr
```

### 4. 使用其他方法（可选）🔄

除了使用`nc`命令外，我们还可以使用其他工具完成相同的任务：

使用`telnet`：

```bash
telnet localhost 30000
```

然后输入密码。

使用`echo`和管道：

```bash
echo "4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e" | nc localhost 30000
```

这会直接将密码发送到服务器，并显示返回的结果。

## 命令解释 🔍

- `nc localhost 30000`：
  - `nc`：netcat命令
  - `localhost`：目标主机（本地主机）
  - `30000`：目标端口
- `echo "4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e" | nc localhost 30000`：
  - `echo "4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e"`：输出密码
  - `|`：管道操作符，将`echo`命令的输出作为`nc`命令的输入
  - `nc localhost 30000`：连接到本地主机的30000端口

## 学习要点总结 📌

1. **网络通信基础** - 学习了如何使用命令行工具与网络服务进行通信
2. **端口和服务** - 了解了端口在网络通信中的作用
3. **netcat工具** - 学习了如何使用`nc`命令连接到网络服务
4. **数据传输** - 了解了如何通过网络发送和接收数据

## 提示 💡

- `nc`（netcat）是一个非常强大的网络工具，可以用于端口扫描、文件传输、聊天等多种用途
- 在实际工作中，了解基本的网络通信工具和概念对于系统管理和安全测试非常重要
- 使用`echo`和管道组合可以自动化网络通信过程，无需手动输入数据

## 下一步 ⏭️

使用获得的密码登录到Level 15：

```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
```

密码：`BfMYroe26WYalil77FoDi9qh59eK5xNr`

## 相关资源 🔗

- [Netcat (nc) 命令详解](https://www.geeksforgeeks.org/nc-command-in-linux-with-examples/)
- [Linux网络命令大全](https://www.tecmint.com/linux-networking-commands/)
- [端口和套接字基础](https://www.digitalocean.com/community/tutorials/understanding-sockets)
