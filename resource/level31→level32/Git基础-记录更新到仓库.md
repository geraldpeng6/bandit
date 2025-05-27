# Git基础 - 记录更新到仓库 📝

## 文件的生命周期 🔄

在Git中，文件主要有四种状态：

1. **未跟踪（Untracked）** - 文件存在于工作目录中，但Git尚未开始跟踪它
2. **已跟踪，未修改（Tracked, Unmodified）** - 文件已被Git跟踪，且与上次提交时的版本相同
3. **已跟踪，已修改（Tracked, Modified）** - 文件已被Git跟踪，但内容与上次提交时不同
4. **已跟踪，已暂存（Tracked, Staged）** - 文件已被Git跟踪，且已标记为下次提交时要保存的内容

了解这些状态及其之间的转换是掌握Git的基础。

## 检查文件状态 👀

要查看文件的状态，使用`git status`命令：

```bash
git status
```

输出示例：
```
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   index.js

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new_file.txt
```

这个输出告诉我们：
- README.md已修改并已暂存（将被包含在下次提交中）
- index.js已修改但未暂存（不会被包含在下次提交中，除非先暂存它）
- new_file.txt是一个未跟踪的新文件

### 简洁状态输出

如果你想要更简洁的输出，可以使用`-s`或`--short`选项：

```bash
git status -s
```

输出示例：
```
M  README.md
 M index.js
?? new_file.txt
```

其中：
- 左列表示暂存区的状态，右列表示工作区的状态
- `M`表示已修改
- `??`表示未跟踪
- 空格表示未修改

## 跟踪新文件 ✨

要开始跟踪一个新文件，使用`git add`命令：

```bash
git add new_file.txt
```

这个命令将文件添加到暂存区，告诉Git你想要在下次提交时包含这个文件。

### 跟踪多个文件

你可以一次添加多个文件：

```bash
git add file1.txt file2.txt file3.txt
```

或者使用通配符：

```bash
git add *.txt  # 添加所有.txt文件
```

### 添加目录

你也可以添加整个目录：

```bash
git add src/  # 添加src目录及其所有内容
```

## 暂存修改的文件 📋

对于已跟踪但已修改的文件，也使用`git add`命令将其暂存：

```bash
git add modified_file.txt
```

`git add`命令是多功能的：
- 开始跟踪新文件
- 暂存已修改的文件
- 标记合并冲突已解决

### 部分暂存

如果你只想暂存文件的部分更改，可以使用交互式添加：

```bash
git add -p  # 或 --patch
```

这会逐个显示每个更改，并询问你是否要暂存它。

## 忽略文件 🙈

有些文件你不希望Git跟踪，如编译生成的文件、临时文件、密钥等。你可以创建一个名为`.gitignore`的文件，列出要忽略的文件模式。

### .gitignore文件示例

```
# 忽略所有.log文件
*.log

# 忽略node_modules目录
node_modules/

# 忽略所有.tmp文件，但不忽略important.tmp
*.tmp
!important.tmp

# 忽略根目录下的config.json，但不忽略子目录中的config.json
/config.json

# 忽略所有目录中的build目录
**/build/
```

### .gitignore规则

- 空行或以`#`开头的行会被忽略
- 标准的glob模式匹配
- 以`/`开头的模式只匹配根目录
- 以`/`结尾的模式只匹配目录
- 以`!`开头的模式表示取反（不忽略）

### 全局忽略文件

你可以创建全局忽略文件，适用于所有Git仓库：

```bash
git config --global core.excludesfile ~/.gitignore_global
```

然后在`~/.gitignore_global`中添加规则。

### 已跟踪文件的忽略

`.gitignore`只能忽略尚未被跟踪的文件。如果文件已经被跟踪，添加到`.gitignore`不会使Git停止跟踪它。

要停止跟踪已跟踪的文件，使用：

```bash
git rm --cached filename
```

这会从暂存区删除文件，但保留在工作目录中。

## 查看暂存和未暂存的更改 🔍

### 查看未暂存的更改

要查看工作目录中的更改（尚未暂存的更改），使用：

```bash
git diff
```

这会显示你修改了什么，但尚未暂存。

### 查看已暂存的更改

要查看已暂存的更改（将要进入下次提交的内容），使用：

```bash
git diff --staged  # 或 --cached
```

### 使用外部工具查看差异

如果你更喜欢图形化的差异查看器，可以使用：

```bash
git difftool
```

这会启动你配置的差异查看工具。

## 提交更改 📤

当你准备好提交更改时，使用`git commit`命令：

```bash
git commit
```

这会打开一个文本编辑器，让你输入提交消息。

### 内联提交消息

你可以使用`-m`选项直接提供提交消息：

```bash
git commit -m "添加用户认证功能"
```

### 跳过暂存区

如果你想跳过暂存区，直接提交所有已跟踪且已修改的文件，可以使用`-a`选项：

```bash
git commit -a -m "更新所有已跟踪文件"
```

这相当于先执行`git add`添加所有已跟踪的文件，然后再提交。

## 删除文件 🗑️

要从Git中删除文件，使用`git rm`命令：

```bash
git rm file_to_delete.txt
```

这会从暂存区和工作目录中删除文件。

### 保留工作目录中的文件

如果你想保留工作目录中的文件，但从Git中删除它，使用`--cached`选项：

```bash
git rm --cached file_to_keep.txt
```

这会从暂存区删除文件，但保留在工作目录中。

### 删除多个文件

你可以使用通配符删除多个文件：

```bash
git rm log/*.log  # 删除log目录中的所有.log文件
```

## 移动文件 🚚

要重命名文件或移动文件，使用`git mv`命令：

```bash
git mv old_name.txt new_name.txt
```

这相当于执行以下三个命令：
```bash
mv old_name.txt new_name.txt
git rm old_name.txt
git add new_name.txt
```

## 查看提交历史 📜

要查看提交历史，使用`git log`命令：

```bash
git log
```

这会显示所有提交的详细信息，包括提交哈希、作者、日期和提交消息。

### 简化输出

如果你想要更简洁的输出，可以使用`--oneline`选项：

```bash
git log --oneline
```

### 显示差异

要显示每次提交的差异，使用`-p`选项：

```bash
git log -p
```

### 限制输出数量

要限制显示的提交数量，使用`-n`选项：

```bash
git log -n 5  # 只显示最近5次提交
```

### 图形化显示

要以图形方式显示分支和合并历史，使用`--graph`选项：

```bash
git log --graph --oneline --all
```

## 修改最后一次提交 🔄

如果你刚刚提交，但发现有些文件忘记添加，或者提交消息有错误，可以使用`--amend`选项修改最后一次提交：

```bash
# 修改提交消息
git commit --amend -m "新的提交消息"

# 添加遗漏的文件
git add forgotten_file.txt
git commit --amend
```

⚠️ **注意**：不要修改已经推送到远程仓库的提交，这会导致历史分叉。

## 取消暂存的文件 ↩️

如果你错误地暂存了一个文件，可以使用`git restore --staged`命令取消暂存：

```bash
git restore --staged wrongly_added.txt
```

在旧版Git中，使用`git reset HEAD`命令：

```bash
git reset HEAD wrongly_added.txt
```

## 撤销对文件的修改 ↩️

如果你想丢弃工作目录中的更改，恢复到上次提交的状态，使用`git restore`命令：

```bash
git restore modified_file.txt
```

在旧版Git中，使用`git checkout -- file`命令：

```bash
git checkout -- modified_file.txt
```

⚠️ **警告**：这个操作会丢失你的更改，且不可恢复。

## 实际工作流示例 🔄

以下是一个典型的工作流程：

1. 修改文件
   ```bash
   # 编辑文件...
   ```

2. 查看更改
   ```bash
   git status
   git diff
   ```

3. 暂存更改
   ```bash
   git add modified_file1.txt modified_file2.txt
   ```

4. 再次查看状态，确认暂存的内容
   ```bash
   git status
   git diff --staged
   ```

5. 提交更改
   ```bash
   git commit -m "实现新功能：用户认证"
   ```

6. 查看提交历史
   ```bash
   git log
   ```

## 高级技巧 🚀

### 交互式暂存

交互式暂存允许你选择性地暂存文件的部分内容：

```bash
git add -i  # 或 --interactive
```

这会显示一个交互式菜单，让你选择要执行的操作。

### 储藏更改

如果你需要切换分支，但不想提交当前的更改，可以使用`git stash`命令暂时储藏它们：

```bash
git stash
```

稍后，你可以恢复这些更改：

```bash
git stash apply  # 应用最近的储藏
```

### 清理工作目录

要删除所有未跟踪的文件和目录，使用`git clean`命令：

```bash
git clean -fd  # -f表示强制，-d表示包括目录
```

⚠️ **警告**：这个操作会永久删除文件，请谨慎使用。

## 常见问题解答 ❓

### 问题：如何撤销最后一次提交？

**解决方案**：
```bash
# 保留更改，但撤销提交
git reset --soft HEAD~1

# 完全丢弃最后一次提交的更改
git reset --hard HEAD~1
```

### 问题：如何强制添加被忽略的文件？

**解决方案**：
```bash
git add -f ignored_file.txt
```

### 问题：如何查看特定文件的提交历史？

**解决方案**：
```bash
git log --follow -- path/to/file
```

## 练习题 🎯

1. 创建一个新的Git仓库，添加几个文件，并提交它们。
2. 修改一个文件，查看未暂存的更改，然后暂存并提交它。
3. 创建一个`.gitignore`文件，忽略所有`.log`文件和`node_modules`目录。
4. 尝试使用`git add -p`命令部分暂存一个文件的更改。
5. 修改最后一次提交的消息，然后查看提交历史。

## 小贴士 💡

- 养成经常使用`git status`和`git diff`的习惯，确保你了解当前的工作状态
- 提交消息应该简洁明了，描述你做了什么，而不是为什么做
- 小而频繁的提交比大而少的提交更好，这样更容易追踪变更
- 使用有意义的提交消息，这将帮助你和你的团队理解历史变更
- 在提交前，使用`git diff --staged`检查将要提交的内容
- 学习使用`.gitignore`文件，避免提交不必要的文件
- 定期推送你的更改到远程仓库，以防本地数据丢失

通过掌握这些基本的Git操作，你可以有效地管理代码变更，与团队协作，并保持项目历史的清晰和可追踪。这些技能是任何开发者工具箱中的重要组成部分。
