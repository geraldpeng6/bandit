# Bandit Level 13 → Level 14 攻略 🔑

## 关卡目标 🎯

本关卡的目标是使用私钥通过SSH登录到下一关（bandit14）。密码存储在文件`/etc/bandit_pass/bandit14`中，只有用户bandit14才能读取。

## 所需知识 📚

### SSH密钥认证 🔐

SSH支持两种主要的认证方式：
1. 密码认证：使用用户名和密码登录
2. 密钥认证：使用密钥对（公钥和私钥）登录

密钥认证的优点：
- 更安全，不容易受到暴力破解攻击
- 可以实现自动登录，无需每次输入密码
- 可以对密钥设置密码短语（passphrase）增加安全性

### SSH私钥 🗝️

私钥是一个文本文件，通常以PEM格式存储，以`-----BEGIN RSA PRIVATE KEY-----`开头，以`-----END RSA PRIVATE KEY-----`结尾。私钥必须保密，而对应的公钥可以分发给需要的服务器。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

密码：`8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL`

### 2. 查看当前目录中的文件 👀

登录后，使用`ls`命令查看当前目录中的文件：

```bash
ls
```

输出结果应该显示有一个名为`sshkey.private`的文件：

```
sshkey.private
```

### 3. 查看私钥文件内容 🔑

使用`cat`命令查看私钥文件内容：

```bash
cat sshkey.private
```

输出结果应该显示一个SSH私钥，以`-----BEGIN RSA PRIVATE KEY-----`开头。

### 4. 使用私钥登录到bandit14 🔓

使用`ssh`命令的`-i`选项指定私钥文件，登录到本地主机（localhost）的bandit14账户：

```bash
ssh -i sshkey.private bandit14@localhost
```

系统可能会询问是否信任主机，输入`yes`继续。

### 5. 查看bandit14的密码 📝

成功登录到bandit14后，我们可以查看存储在`/etc/bandit_pass/bandit14`中的密码：

```bash
cat /etc/bandit_pass/bandit14
```

输出结果应该显示bandit14的密码：

```
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

## 命令解释 🔍

- `ssh -i sshkey.private bandit14@localhost`：
  - `ssh`：SSH客户端命令
  - `-i sshkey.private`：指定要使用的私钥文件
  - `bandit14@localhost`：以bandit14用户身份登录到本地主机

## 学习要点总结 📌

1. **SSH密钥认证** - 学习了如何使用SSH私钥进行认证
2. **私钥文件** - 了解了SSH私钥文件的格式和用途
3. **本地SSH登录** - 学习了如何在同一台服务器上使用SSH登录到不同的用户账户
4. **文件权限** - 理解了Linux文件权限系统，特别是关于敏感文件（如密码文件）的权限控制

## 提示 💡

- 在实际使用中，私钥文件应该设置为只有所有者可读（权限600）
- 可以使用`ssh-keygen`命令生成自己的SSH密钥对
- 使用SSH密钥认证可以提高服务器的安全性，减少密码被盗的风险
- 在多台服务器之间使用SSH密钥认证可以简化登录过程

## 下一步 ⏭️

使用获得的密码登录到Level 14：

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

密码：`4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e`

## 相关资源 🔗

- [SSH密钥认证详解](./resource/level13→level14/SSH密钥认证详解.md)
- [使用SSH私钥登录Linux和macOS](./resource/level13→level14/使用SSH私钥登录Linux和macOS.md)
- [SSH密钥认证工作原理](./resource/level13→level14/SSH密钥认证工作原理.md)
