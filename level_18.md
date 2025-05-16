# Bandit Level 17 → Level 18 攻略 📊

## 关卡目标 🎯

本关卡的目标是找出文件`passwords.old`和`passwords.new`之间的区别，其中只有一行发生了变化，这一行包含了下一关的密码。

## 所需知识 📚

### 文件比较 📋

在Linux中，可以使用多种工具比较文件之间的差异：
- `diff`：显示两个文件之间的差异
- `cmp`：逐字节比较两个文件
- `comm`：比较两个已排序文件的行

### diff命令 🔄

`diff`命令是最常用的文件比较工具，它可以显示两个文件之间的差异，包括哪些行被添加、删除或修改。

## 详细解题步骤 📝

### 1. 确认当前登录状态 🔐

如果你已经按照上一关的步骤使用SSH私钥登录到bandit17，可以跳过这一步。否则，使用上一关获得的私钥登录：

```bash
ssh -i /tmp/bandit16/sshkey17.private bandit17@bandit.labs.overthewire.org -p 2220
```

### 2. 查看当前目录中的文件 👀

登录后，使用`ls`命令查看当前目录中的文件：

```bash
ls
```

输出结果应该显示有两个文件：

```
passwords.new  passwords.old
```

### 3. 使用diff命令比较两个文件 🔍

使用`diff`命令比较两个文件之间的差异：

```bash
diff passwords.old passwords.new
```

输出结果应该类似于：

```
42c42
< 810zq8IK64u5A9Lb2ibdTGBtlcSZsoe8
---
> kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
```

这表明第42行发生了变化，`<`表示`passwords.old`中的行，`>`表示`passwords.new`中的行。因此，下一关的密码是`kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`。

### 4. 使用其他方法（可选）🔄

除了使用`diff`命令外，我们还可以使用其他方法查看文件内容并手动比较：

查看`passwords.old`的内容：

```bash
cat passwords.old
```

查看`passwords.new`的内容：

```bash
cat passwords.new
```

但由于文件可能包含大量行，手动比较可能不太实用。

另一种方法是使用`diff`命令的`--side-by-side`选项，以并排方式显示差异：

```bash
diff --side-by-side passwords.old passwords.new
```

或者使用`vimdiff`命令，以可视化方式显示差异：

```bash
vimdiff passwords.old passwords.new
```

### 5. 获取bandit17的密码（可选）📝

虽然我们已经登录到bandit17，但如果需要密码（例如，如果我们想直接使用密码登录而不是使用私钥），可以查看密码文件：

```bash
cat /etc/bandit_pass/bandit17
```

输出结果应该显示bandit17的密码：

```
xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn
```

## 命令解释 🔍

- `diff passwords.old passwords.new`：
  - `diff`：比较文件的命令
  - `passwords.old`：第一个要比较的文件
  - `passwords.new`：第二个要比较的文件
- `diff --side-by-side passwords.old passwords.new`：
  - `--side-by-side`：以并排方式显示差异
- `vimdiff passwords.old passwords.new`：
  - `vimdiff`：使用vim编辑器以可视化方式显示文件差异

## 学习要点总结 📌

1. **文件比较基础** - 学习了如何使用`diff`命令比较文件差异
2. **差异输出解读** - 了解了如何解读`diff`命令的输出
3. **多种比较方法** - 学习了多种文件比较方法，包括命令行和可视化方法
4. **文件内容查看** - 复习了使用`cat`命令查看文件内容

## 提示 💡

- `diff`命令有多种输出格式选项，可以根据需要选择最适合的格式
- 在实际工作中，文件比较是一项常见任务，特别是在处理配置文件、代码文件和日志文件时
- 对于大型文件或复杂差异，可以考虑使用图形化差异查看工具，如`meld`或`kdiff3`
- `git diff`是另一个常用的差异查看工具，特别是在处理版本控制系统时

## 下一步 ⏭️

使用获得的密码登录到Level 18：

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

密码：`kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`

## 相关资源 🔗

- [Linux diff命令详解](https://www.geeksforgeeks.org/diff-command-linux-examples/)
- [Linux文件比较工具](https://www.tecmint.com/best-linux-file-diff-tools-comparison/)
- [vimdiff使用指南](https://www.tutorialspoint.com/vim/vim_diff.htm)
