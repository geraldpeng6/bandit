# Bandit Level 31 → Level 32 攻略 📤

## 关卡目标 🎯

本关卡的目标是克隆一个Git仓库，该仓库位于`ssh://bandit31-git@localhost/home/bandit31-git/repo`，然后按照仓库中的说明创建一个文件并将其推送到远程仓库，以获取下一关的密码。

## 所需知识 📚

### Git文件操作 📝

我们需要了解如何在Git仓库中创建、修改和添加文件。

### Git提交和推送 📤

我们需要了解如何将更改提交到本地仓库，然后推送到远程仓库。

### Git忽略文件 🙈

我们需要了解`.gitignore`文件的作用，以及如何强制添加被忽略的文件。

## 详细解题步骤 📝

### 1. 登录游戏服务器 🔐

使用上一关获得的密码登录服务器：

```bash
ssh bandit31@bandit.labs.overthewire.org -p 2220
```

密码：`47e603bb428404d265f59c42920d81e5`

### 2. 创建临时目录 📂

首先，我们需要创建一个临时目录来克隆Git仓库：

```bash
mkdir -p /tmp/bandit31_git
cd /tmp/bandit31_git
```

### 3. 克隆Git仓库 📦

现在我们可以使用`git clone`命令克隆仓库：

```bash
git clone ssh://bandit31-git@localhost/home/bandit31-git/repo
```

当提示输入密码时，输入bandit31的密码：`47e603bb428404d265f59c42920d81e5`

### 4. 查看克隆的仓库 👀

克隆完成后，我们可以进入仓库目录并查看其内容：

```bash
cd repo
ls -la
```

输出结果应该类似于：

```
total 20
drwxr-sr-x 3 bandit31 root 4096 May 16 13:40 .
drwxr-sr-x 3 bandit31 root 4096 May 16 13:40 ..
drwxr-sr-x 8 bandit31 root 4096 May 16 13:40 .git
-rw-r--r-- 1 bandit31 root    6 May 16 13:40 .gitignore
-rw-r--r-- 1 bandit31 root  147 May 16 13:40 README.md
```

我们可以看到仓库中有一个README.md文件和一个.gitignore文件。让我们查看它们的内容：

```bash
cat README.md
```

输出结果可能类似于：

```
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

这表明我们需要创建一个名为`key.txt`的文件，内容为`May I come in?`，并将其推送到master分支。

让我们也查看`.gitignore`文件的内容：

```bash
cat .gitignore
```

输出结果可能类似于：

```
*.txt
```

这表明所有`.txt`文件都被忽略，不会被Git跟踪。这可能会阻止我们推送`key.txt`文件。

### 5. 创建key.txt文件 📝

根据README.md的说明，我们需要创建一个名为`key.txt`的文件，内容为`May I come in?`：

```bash
echo 'May I come in?' > key.txt
```

### 6. 添加文件到Git 📥

由于`.gitignore`文件指定忽略所有`.txt`文件，我们需要使用`-f`（强制）选项来添加`key.txt`文件：

```bash
git add -f key.txt
```

### 7. 提交更改 📤

现在我们可以提交更改：

```bash
git commit -m "Add key.txt"
```

输出结果应该类似于：

```
[master 5503b5e] Add key.txt
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
```

### 8. 推送到远程仓库 🚀

最后，我们需要将更改推送到远程仓库：

```bash
git push origin master
```

当提示输入密码时，再次输入bandit31的密码：`47e603bb428404d265f59c42920d81e5`

输出结果应该包含下一关的密码：

```
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 321 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: ### Attempting to validate files... ####
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
remote: Well done! Here is the password for the next level:
remote: 56a9bf19c63d650ce78e6ec0354ee45e
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
To ssh://localhost/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://bandit31-git@localhost/home/bandit31-git/repo'
```

尽管推送被拒绝（这是正常的，因为我们没有权限修改远程仓库），但我们仍然收到了下一关的密码：`56a9bf19c63d650ce78e6ec0354ee45e`。

## 命令解释 🔍

- `echo 'May I come in?' > key.txt`：
  - `echo`：输出字符串
  - `'May I come in?'`：要输出的字符串
  - `> key.txt`：将输出重定向到`key.txt`文件
- `git add -f key.txt`：
  - `git add`：将文件添加到暂存区
  - `-f`：强制添加，即使文件被`.gitignore`忽略
  - `key.txt`：要添加的文件
- `git commit -m "Add key.txt"`：
  - `git commit`：提交更改
  - `-m "Add key.txt"`：提交消息
- `git push origin master`：
  - `git push`：推送更改到远程仓库
  - `origin`：远程仓库的名称
  - `master`：要推送的分支

## 学习要点总结 📌

1. **Git文件操作** - 学习了如何在Git仓库中创建和添加文件
2. **Git忽略文件** - 了解了`.gitignore`文件的作用，以及如何强制添加被忽略的文件
3. **Git提交和推送** - 学习了如何将更改提交到本地仓库，然后推送到远程仓库
4. **Git钩子** - 了解了Git钩子（如pre-receive钩子）的概念

## 提示 💡

- `.gitignore`文件用于指定Git应该忽略的文件或目录
- 使用`git add -f`可以强制添加被`.gitignore`忽略的文件
- Git钩子是在Git执行特定操作（如提交、推送）时自动运行的脚本
- 在实际开发中，推送被拒绝通常表示远程仓库有保护措施，如分支保护或钩子脚本

## 下一步 ⏭️

使用获得的密码登录到Level 32：

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```

密码：`56a9bf19c63d650ce78e6ec0354ee45e`

## 相关资源 🔗

- [Git基础 - 记录更新到仓库](./resource/level31→level32/Git基础-记录更新到仓库.md)
- [Git工具 - 忽略文件](./resource/level31→level32/Git工具-忽略文件.md)
- [Git钩子](./resource/level31→level32/Git钩子.md)
