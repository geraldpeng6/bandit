# Git钩子详解 🪝

## 什么是Git钩子？ 🤔

Git钩子（Git Hooks）是在Git仓库中特定事件发生时自动执行的脚本。这些事件包括提交代码、推送到远程仓库、接收推送等。钩子可以用来自动化或自定义你的Git工作流，执行代码质量检查，触发部署，发送通知等。

Git钩子本质上是位于`.git/hooks`目录中的可执行脚本，可以用任何脚本语言编写，如Bash、Python、Ruby等，只要系统能够执行它们。

## Git钩子的类型 📋

Git钩子分为两大类：客户端钩子和服务器端钩子。

### 客户端钩子

客户端钩子在开发者的本地仓库中运行，用于在本地Git操作期间执行操作。

主要的客户端钩子包括：

#### 提交工作流钩子

1. **pre-commit** - 在提交消息编辑前运行，用于检查即将提交的快照
2. **prepare-commit-msg** - 在默认提交消息生成后，编辑器启动前运行
3. **commit-msg** - 用于验证提交消息
4. **post-commit** - 在整个提交过程完成后运行

#### 电子邮件工作流钩子

1. **applypatch-msg** - 用于验证补丁的提交消息
2. **pre-applypatch** - 在补丁应用后但提交前运行
3. **post-applypatch** - 在补丁应用且提交后运行

#### 其他客户端钩子

1. **pre-rebase** - 在变基操作前运行
2. **post-rewrite** - 在`git commit --amend`或`git rebase`等重写提交的命令后运行
3. **post-checkout** - 在`git checkout`成功运行后执行
4. **post-merge** - 在`git merge`成功运行后执行
5. **pre-push** - 在`git push`运行期间，更新了远程引用但尚未传输对象前运行
6. **pre-auto-gc** - 在垃圾收集（garbage collection）自动运行前调用

### 服务器端钩子

服务器端钩子在远程Git仓库上运行，用于在接收推送时执行操作。

主要的服务器端钩子包括：

1. **pre-receive** - 在服务器接收推送但尚未更新引用前运行
2. **update** - 类似于pre-receive，但针对每个被推送的分支运行一次
3. **post-receive** - 在整个推送过程完成后运行

## 钩子的工作原理 ⚙️

Git钩子的工作原理非常简单：

1. 当特定的Git事件发生时，Git会查找对应的钩子脚本
2. 如果找到了可执行的钩子脚本，Git会执行它
3. 对于某些钩子（如pre-commit、pre-push等），如果脚本以非零状态退出，Git会中止操作

所有的钩子脚本都存储在`.git/hooks`目录中。Git在初始化仓库时会创建一些示例钩子，它们都带有`.sample`后缀，需要重命名并设置为可执行才能生效。

## 创建和启用钩子 ✨

### 基本步骤

1. 进入Git仓库的hooks目录：
   ```bash
   cd .git/hooks
   ```

2. 创建一个新的钩子脚本或编辑现有的示例：
   ```bash
   # 创建新钩子
   touch pre-commit
   
   # 或重命名示例
   cp pre-commit.sample pre-commit
   ```

3. 编辑钩子脚本，添加你想要执行的命令

4. 使钩子脚本可执行：
   ```bash
   chmod +x pre-commit
   ```

### 示例：简单的pre-commit钩子

以下是一个简单的pre-commit钩子，用于检查代码中是否有调试语句：

```bash
#!/bin/bash

# 检查是否有调试语句
if git diff --cached | grep -E 'console\.log|debugger|alert\(' > /dev/null; then
    echo "错误：提交包含调试语句（console.log、debugger或alert）"
    echo "请在提交前删除这些语句"
    exit 1
fi

exit 0
```

这个脚本会检查暂存区中的代码是否包含`console.log`、`debugger`或`alert(`，如果找到，会阻止提交并显示错误消息。

## 常用钩子示例 📚

### pre-commit钩子

#### 代码风格检查

```bash
#!/bin/bash

# 运行ESLint检查JavaScript文件
files=$(git diff --cached --name-only --diff-filter=ACM | grep '\.js$')
if [ -n "$files" ]; then
    echo "检查JavaScript代码风格..."
    npx eslint $files
    if [ $? -ne 0 ]; then
        echo "ESLint检查失败，请修复上述问题后再提交"
        exit 1
    fi
fi

exit 0
```

#### 防止提交敏感信息

```bash
#!/bin/bash

# 检查是否有密码、API密钥等敏感信息
if git diff --cached | grep -E 'password|api_key|secret|token' > /dev/null; then
    echo "警告：提交可能包含敏感信息"
    echo "请检查并确认这不是密码或API密钥"
    exit 1
fi

exit 0
```

### commit-msg钩子

#### 强制提交消息格式

```bash
#!/bin/bash

# 检查提交消息是否符合约定式提交规范
commit_msg_file=$1
commit_msg=$(cat $commit_msg_file)

if ! echo "$commit_msg" | grep -E '^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .+' > /dev/null; then
    echo "错误：提交消息不符合约定式提交规范"
    echo "格式应为：<类型>[(范围)]: <描述>"
    echo "例如：feat(auth): 添加用户登录功能"
    exit 1
fi

exit 0
```

### pre-push钩子

#### 运行测试

```bash
#!/bin/bash

# 在推送前运行测试
echo "运行测试..."
npm test
if [ $? -ne 0 ]; then
    echo "测试失败，推送已中止"
    exit 1
fi

exit 0
```

### post-receive钩子（服务器端）

#### 自动部署

```bash
#!/bin/bash

# 部署到生产环境
target="/var/www/myapp"
GIT_WORK_TREE="$target" git checkout -f master
cd "$target"
npm install
npm run build
pm2 restart myapp

echo "应用已成功部署到生产环境"
```

## 共享钩子 🔄

Git钩子默认存储在`.git/hooks`目录中，这个目录不会被提交到仓库中。如果你想在团队中共享钩子，有几种方法：

### 1. 使用脚本安装钩子

创建一个脚本来安装钩子：

```bash
#!/bin/bash
# install-hooks.sh

# 复制钩子到.git/hooks目录
cp hooks/* .git/hooks/
# 使钩子可执行
chmod +x .git/hooks/*

echo "Git钩子已安装"
```

团队成员可以运行这个脚本来安装钩子。

### 2. 使用符号链接

将钩子存储在仓库中的一个目录（如`git-hooks`），然后创建符号链接：

```bash
# 创建符号链接
ln -sf ../../git-hooks/pre-commit .git/hooks/pre-commit
```

### 3. 使用Git配置

Git 2.9+支持配置钩子路径：

```bash
git config core.hooksPath ./git-hooks
```

这会告诉Git在`./git-hooks`目录中查找钩子，而不是默认的`.git/hooks`。

### 4. 使用钩子管理工具

有许多工具可以帮助管理和共享Git钩子，如：

- **Husky** - 用于JavaScript项目
- **pre-commit** - 用于Python项目
- **overcommit** - 用于Ruby项目

## 钩子的最佳实践 ✨

### 1. 保持钩子简单快速

钩子应该快速执行，避免长时间运行的操作，特别是pre-commit等频繁触发的钩子。

### 2. 提供跳过选项

为钩子提供跳过选项，以便在必要时绕过检查：

```bash
#!/bin/bash

# 允许使用SKIP_TESTS环境变量跳过测试
if [ "$SKIP_TESTS" = "true" ]; then
    echo "警告：跳过测试"
    exit 0
fi

# 运行测试...
```

使用时：
```bash
SKIP_TESTS=true git push
```

### 3. 提供有用的错误消息

当钩子失败时，提供清晰的错误消息和修复建议：

```bash
echo "错误：代码格式检查失败"
echo "请运行 'npm run format' 修复格式问题"
```

### 4. 使用适当的退出代码

- 退出代码0表示成功，Git会继续操作
- 非零退出代码表示失败，Git会中止操作（对于pre-*钩子）

### 5. 考虑性能影响

对于大型仓库，考虑只检查已更改的文件，而不是整个仓库：

```bash
# 只检查已更改的JavaScript文件
files=$(git diff --cached --name-only --diff-filter=ACM | grep '\.js$')
if [ -n "$files" ]; then
    eslint $files
fi
```

## 高级钩子技术 🚀

### 1. 多语言钩子

虽然大多数示例使用Bash脚本，但你可以使用任何脚本语言。例如，Python钩子：

```python
#!/usr/bin/env python3
# pre-commit hook in Python

import sys
import subprocess

# 获取暂存的文件
result = subprocess.run(['git', 'diff', '--cached', '--name-only'], 
                        capture_output=True, text=True)
files = result.stdout.splitlines()

# 检查Python文件
python_files = [f for f in files if f.endswith('.py')]
if python_files:
    print("检查Python文件...")
    result = subprocess.run(['flake8'] + python_files)
    if result.returncode != 0:
        print("Flake8检查失败，请修复上述问题")
        sys.exit(1)

sys.exit(0)
```

### 2. 使用Git环境变量

Git设置了许多环境变量，你可以在钩子中使用：

```bash
#!/bin/bash
# post-checkout钩子

# $GIT_DIR - .git目录的路径
# $GIT_WORK_TREE - 工作目录的路径
# $GIT_AUTHOR_NAME - 作者名称
# $GIT_AUTHOR_EMAIL - 作者邮箱

echo "切换到: $(git symbolic-ref HEAD)"
echo "工作目录: $GIT_WORK_TREE"
```

### 3. 交互式钩子

钩子可以是交互式的，询问用户输入：

```bash
#!/bin/bash
# pre-push钩子

branch=$(git symbolic-ref HEAD | sed -e 's,.*/\(.*\),\1,')
if [ "$branch" = "master" ]; then
    read -p "你确定要推送到master分支吗？ (y/n) " -n 1 -r
    echo
    if [[ ! $REPLY =~ ^[Yy]$ ]]; then
        echo "推送已取消"
        exit 1
    fi
fi

exit 0
```

## 常见问题解答 ❓

### 问题：我的钩子没有执行

**可能的原因**：
- 钩子文件没有执行权限
- 钩子文件名不正确（有`.sample`后缀或拼写错误）
- 钩子的shebang行（`#!/bin/bash`）不正确

**解决方案**：
```bash
# 检查钩子是否存在且可执行
ls -la .git/hooks/pre-commit

# 设置执行权限
chmod +x .git/hooks/pre-commit

# 检查shebang行
head -n 1 .git/hooks/pre-commit
```

### 问题：如何临时禁用钩子？

**解决方案**：
```bash
# 跳过pre-commit钩子
git commit --no-verify

# 跳过pre-push钩子
git push --no-verify
```

### 问题：如何在CI/CD环境中使用钩子？

**解决方案**：
大多数CI/CD系统不会运行Git钩子。相反，你应该在CI/CD配置中直接包含相同的检查：

```yaml
# .gitlab-ci.yml示例
stages:
  - test

lint:
  stage: test
  script:
    - eslint .
```

## 练习题 🎯

1. 创建一个pre-commit钩子，检查代码中是否有TODO注释，如果有，提示开发者但允许提交。
2. 编写一个commit-msg钩子，确保提交消息至少有10个字符。
3. 创建一个post-commit钩子，在每次提交后显示最近5次提交的简短日志。
4. 编写一个pre-push钩子，防止直接推送到master分支。
5. 创建一个安装脚本，将仓库中的钩子安装到`.git/hooks`目录。

## 小贴士 💡

- 使用`echo`或`printf`在钩子中输出有用的信息
- 在开发钩子时，使用`set -x`启用调试输出
- 记住，钩子是本地的，不会随仓库一起克隆
- 考虑使用钩子管理工具，如Husky，简化钩子的安装和管理
- 在团队中记录钩子的用途和行为
- 为长时间运行的钩子添加进度指示
- 定期审查和更新钩子，确保它们仍然有用且高效

通过掌握Git钩子，你可以自动化许多任务，提高代码质量，并确保团队遵循一致的工作流程。钩子是Git强大而灵活的特性，可以根据你的需求进行定制，使Git更好地适应你的工作方式。
