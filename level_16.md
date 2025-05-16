# Bandit Level 15 → Level 16 攻略 🔒

## 关卡目标 🎯

本关卡的目标是通过SSL加密连接将当前级别的密码提交到本地主机（localhost）的30001端口来获取下一关的密码。

## 所需知识 📚

### SSL/TLS加密通信 🔐

SSL（Secure Sockets Layer）和其继任者TLS（Transport Layer Security）是用于在网络上提供加密通信的协议。它们广泛用于保护网站（HTTPS）、电子邮件和其他应用程序的通信安全。

### OpenSSL工具 🛠️

OpenSSL是一个强大的开源工具包，用于实现SSL/TLS协议和各种加密功能。它包含了一个命令行工具`openssl`，可以用于各种加密操作，包括与SSL/TLS服务器通信。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
```

密码：`BfMYroe26WYalil77FoDi9qh59eK5xNr`

### 2. 获取当前级别的密码 🔑

我们已经知道当前级别（bandit15）的密码，但为了确保我们使用正确的密码，可以从密码文件中读取：

```bash
cat /etc/bandit_pass/bandit15
```

输出结果应该显示bandit15的密码：

```
BfMYroe26WYalil77FoDi9qh59eK5xNr
```

### 3. 使用OpenSSL连接到本地主机的30001端口 🔌

我们可以使用`openssl`命令的`s_client`子命令连接到SSL/TLS服务器：

```bash
openssl s_client -connect localhost:30001
```

这个命令会建立一个SSL/TLS连接，并显示大量关于连接的信息，包括证书详情。连接建立后，我们可以输入当前级别的密码：

```
BfMYroe26WYalil77FoDi9qh59eK5xNr
```

输入密码后，服务器应该返回下一关的密码：

```
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd
```

### 4. 使用更简洁的命令（可选）🔄

如果我们想要跳过显示证书信息，可以使用`-quiet`选项：

```bash
openssl s_client -connect localhost:30001 -quiet
```

然后输入密码。

或者，我们可以使用`echo`和管道直接发送密码：

```bash
echo "BfMYroe26WYalil77FoDi9qh59eK5xNr" | openssl s_client -connect localhost:30001 -quiet
```

这会直接将密码发送到服务器，并显示返回的结果。但由于连接信息和返回结果混在一起，可能需要仔细查看输出以找到密码。

## 命令解释 🔍

- `openssl s_client -connect localhost:30001`：
  - `openssl`：OpenSSL命令行工具
  - `s_client`：用于创建SSL/TLS客户端连接的子命令
  - `-connect localhost:30001`：指定要连接的主机和端口
- `echo "BfMYroe26WYalil77FoDi9qh59eK5xNr" | openssl s_client -connect localhost:30001 -quiet`：
  - `echo "BfMYroe26WYalil77FoDi9qh59eK5xNr"`：输出密码
  - `|`：管道操作符，将`echo`命令的输出作为`openssl`命令的输入
  - `-quiet`：减少输出信息，只显示必要的内容

## 学习要点总结 📌

1. **SSL/TLS基础** - 学习了SSL/TLS加密通信的基本概念
2. **OpenSSL工具** - 了解了如何使用OpenSSL工具与SSL/TLS服务器通信
3. **加密连接** - 学习了如何建立加密连接并通过该连接发送数据
4. **证书信息** - 了解了SSL/TLS连接中的证书信息

## 提示 💡

- SSL/TLS是现代互联网安全的基础，几乎所有需要安全通信的网站和应用程序都使用它
- OpenSSL是一个功能强大的工具包，除了与SSL/TLS服务器通信外，还可以用于生成密钥、证书和执行各种加密操作
- 在实际工作中，了解如何使用OpenSSL工具对于调试SSL/TLS问题和验证服务器配置非常有用

## 下一步 ⏭️

使用获得的密码登录到Level 16：

```bash
ssh bandit16@bandit.labs.overthewire.org -p 2220
```

密码：`cluFn7wTiGryunymYOu4RcffSxQluehd`

## 相关资源 🔗

- [OpenSSL命令行工具详解](./resource/level_16/OpenSSL命令行工具详解.md)
- [SSL/TLS协议介绍](./resource/level_16/SSL_TLS协议介绍.md)
- [使用OpenSSL调试SSL/TLS连接](./resource/level_16/OpenSSL调试SSL_TLS连接.md)
