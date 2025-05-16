# Bandit Level 16 → Level 17 攻略 🔍

## 关卡目标 🎯

本关卡的目标是首先找出哪些服务器端口在本地主机（localhost）的31000-32000范围内监听，然后确定哪些端口使用SSL加密，最后向其中一个端口提交当前级别的密码以获取SSH私钥，该私钥用于访问下一关。

## 所需知识 📚

### 端口扫描 🔎

端口扫描是一种网络技术，用于确定主机上哪些端口是开放的（有服务在监听）。在Linux中，可以使用`nmap`工具进行端口扫描。

### SSL/TLS服务识别 🔐

除了确定端口是否开放外，还需要确定哪些端口提供SSL/TLS加密服务。这可以通过尝试与每个开放端口建立SSL连接来实现。

### SSH私钥使用 🗝️

与Level 13类似，本关卡需要使用SSH私钥进行认证。私钥通常是一个文本文件，需要正确设置权限才能使用。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit16@bandit.labs.overthewire.org -p 2220
```

密码：`cluFn7wTiGryunymYOu4RcffSxQluehd`

### 2. 扫描端口范围 🔍

使用`nmap`命令扫描31000-32000范围内的端口：

```bash
nmap -p 31000-32000 localhost
```

输出结果应该类似于：

```
Starting Nmap 7.40 ( https://nmap.org ) at 2023-05-16 12:00 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00023s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```

这表明在指定范围内有5个开放的端口。

### 3. 确定哪些端口使用SSL 🔐

我们可以使用`openssl s_client`命令尝试连接到每个开放的端口，看哪些端口支持SSL：

```bash
openssl s_client -connect localhost:31046
```

如果端口不支持SSL，会显示错误信息。如果支持SSL，会显示证书信息。

重复这个过程，尝试连接到每个开放的端口，直到找到支持SSL的端口。例如：

```bash
openssl s_client -connect localhost:31790
```

如果这个端口支持SSL，我们会看到证书信息，然后可以输入当前级别的密码：

```
cluFn7wTiGryunymYOu4RcffSxQluehd
```

如果这是正确的端口，服务器会返回一个SSH私钥：

```
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```

### 4. 保存私钥到文件 💾

将返回的私钥保存到一个文件中，例如`/tmp/sshkey17.private`：

```bash
mkdir -p /tmp/bandit16
echo "-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----" > /tmp/bandit16/sshkey17.private
```

### 5. 设置私钥文件权限 🔒

SSH私钥文件需要设置正确的权限（只有所有者可读写），否则SSH客户端会拒绝使用它：

```bash
chmod 600 /tmp/bandit16/sshkey17.private
```

### 6. 使用私钥登录到bandit17 🔓

使用保存的私钥文件登录到bandit17：

```bash
ssh -i /tmp/bandit16/sshkey17.private bandit17@localhost
```

成功登录后，我们就可以访问Level 17了。

## 命令解释 🔍

- `nmap -p 31000-32000 localhost`：
  - `nmap`：网络扫描工具
  - `-p 31000-32000`：指定要扫描的端口范围
  - `localhost`：目标主机（本地主机）
- `openssl s_client -connect localhost:31790`：
  - `openssl`：OpenSSL命令行工具
  - `s_client`：用于创建SSL/TLS客户端连接的子命令
  - `-connect localhost:31790`：指定要连接的主机和端口
- `chmod 600 /tmp/bandit16/sshkey17.private`：
  - `chmod`：更改文件权限
  - `600`：设置权限为只有所有者可读写（rw-------）
  - `/tmp/bandit16/sshkey17.private`：目标文件

## 学习要点总结 📌

1. **端口扫描** - 学习了如何使用`nmap`工具扫描端口
2. **SSL服务识别** - 了解了如何识别提供SSL/TLS服务的端口
3. **SSH私钥处理** - 学习了如何保存和使用SSH私钥
4. **文件权限设置** - 了解了SSH私钥文件的正确权限设置

## 提示 💡

- `nmap`是一个功能强大的网络扫描工具，在实际工作中常用于网络安全评估和故障排除
- 在处理SSH私钥时，正确设置文件权限非常重要，否则SSH客户端会拒绝使用它
- 在实际工作中，私钥通常存储在用户的`.ssh`目录中，并且应该妥善保管
- 使用`-v`选项（如`ssh -v -i ...`）可以启用详细输出，有助于调试SSH连接问题

## 下一步 ⏭️

现在我们已经成功登录到bandit17，可以继续进行下一关的挑战。

## 相关资源 🔗

- [Nmap端口扫描指南](https://nmap.org/book/man-port-scanning-basics.html)
- [OpenSSL s_client命令详解](https://www.openssl.org/docs/man1.1.1/man1/openssl-s_client.html)
- [SSH密钥认证详解](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
