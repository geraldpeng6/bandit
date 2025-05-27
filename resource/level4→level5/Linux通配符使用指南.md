# Linux通配符使用指南 🌟

## 什么是通配符？ 🤔

通配符（Wildcards）是一种特殊的字符，用于表示一个或多个其他字符。在Linux系统中，通配符主要用于匹配文件名和字符串模式。通配符也被称为"glob模式"（glob patterns），"glob"一词来源于"global"，意为全局匹配。

通配符的强大之处在于它们可以帮助你简洁地表达复杂的匹配模式，从而大大提高命令行操作的效率。无论是查找文件、批量处理还是编写脚本，通配符都是Linux用户必备的技能。

## 基本通配符 📌

Linux shell（如Bash）支持以下基本通配符：

| 通配符 | 描述 | 示例 |
|-------|------|------|
| `*` | 匹配零个或多个任意字符 | `*.txt`匹配所有.txt文件 |
| `?` | 匹配任意单个字符 | `file?.txt`匹配file1.txt、fileA.txt等 |
| `[...]` | 匹配方括号中的任意一个字符 | `[abc]*.txt`匹配以a、b或c开头的.txt文件 |
| `[!...]` | 匹配不在方括号中的任意字符 | `[!a-z]*.txt`匹配不以小写字母开头的.txt文件 |
| `[a-z]` | 匹配指定范围内的任意字符 | `[a-z]*.txt`匹配以小写字母开头的.txt文件 |
| `{...}` | 匹配花括号中的任意一个模式 | `file.{jpg,png,gif}`匹配file.jpg、file.png和file.gif |

## 基本通配符使用示例 🔍

### 星号（*）通配符

星号是最常用的通配符，它可以匹配任意数量的字符（包括零个字符）。

```bash
# 列出所有.txt文件
ls *.txt

# 列出所有以"report"开头的文件
ls report*

# 列出所有包含"data"的文件
ls *data*

# 列出所有文件（包括隐藏文件，需要额外的-a选项）
ls -a *
```

### 问号（?）通配符

问号用于匹配任意单个字符。

```bash
# 列出所有单字符扩展名的文件（如file.a、file.b等）
ls *.?

# 列出所有名称为三个字符的文件
ls ???

# 列出所有以"file"开头后跟一个字符的文件
ls file?.*
```

### 方括号（[...]）通配符

方括号用于匹配方括号中的任意一个字符。

```bash
# 列出所有以a、b或c开头的文件
ls [abc]*

# 列出所有以数字开头的文件
ls [0-9]*

# 列出所有不以小写字母开头的文件
ls [!a-z]*

# 列出所有以大写或小写字母开头的文件
ls [A-Za-z]*
```

### 花括号（{...}）扩展

花括号扩展不是真正的通配符，但它提供了类似的功能，可以生成多个模式。

```bash
# 创建多个目录
mkdir {docs,images,videos}

# 列出多种类型的文件
ls *.{jpg,png,gif}

# 创建带有序号的文件
touch file{1..5}.txt

# 创建带有字母序列的文件
touch file{a..e}.txt
```

## 扩展通配符（Extended Globbing）🚀

Bash提供了扩展通配符功能，可以实现更复杂的模式匹配。要启用扩展通配符，需要使用以下命令：

```bash
shopt -s extglob
```

扩展通配符包括：

| 模式 | 描述 |
|------|------|
| `?(patterns)` | 匹配零次或一次指定的模式 |
| `*(patterns)` | 匹配零次或多次指定的模式 |
| `+(patterns)` | 匹配一次或多次指定的模式 |
| `@(patterns)` | 精确匹配一次指定的模式 |
| `!(patterns)` | 匹配除指定模式外的任何内容 |

其中，`patterns`是由`|`（管道符）分隔的一个或多个模式。

## 扩展通配符使用示例 🔍

### ?(patterns) - 匹配零次或一次

```bash
# 匹配所有.txt文件和没有扩展名的文件
ls *.?(txt)

# 匹配file、file.txt和file.log
ls file?(|.txt|.log)
```

### *(patterns) - 匹配零次或多次

```bash
# 匹配所有以e开头的文件，包括没有e的文件
ls *(e)*

# 匹配所有.txt、.log文件和没有扩展名的文件
ls *.*(txt|log)
```

### +(patterns) - 匹配一次或多次

```bash
# 匹配所有以一个或多个e开头的文件
ls +(e)*

# 匹配所有.txt或.log文件（必须有扩展名）
ls *.+(txt|log)
```

### @(patterns) - 精确匹配一次

```bash
# 只匹配file.txt和file.log，不匹配file或其他文件
ls file.@(txt|log)

# 只匹配以一个e开头的文件
ls @(e)*
```

### !(patterns) - 排除匹配

```bash
# 匹配所有不是.txt或.log的文件
ls *.!(txt|log)

# 匹配所有不以a、b或c开头的文件
ls !([abc]*)
```

## 通配符在不同命令中的应用 🛠️

通配符不仅可以用于`ls`命令，还可以用于许多其他命令：

### 1. 复制文件

```bash
# 复制所有.txt文件到docs目录
cp *.txt docs/

# 复制所有图像文件到images目录
cp *.{jpg,png,gif} images/
```

### 2. 移动文件

```bash
# 移动所有2023年的报告到archive目录
mv report_2023*.* archive/

# 移动所有不是.tmp的文件到另一个目录
mv *.!(tmp) ../other_dir/
```

### 3. 删除文件

```bash
# 删除所有.tmp文件
rm *.tmp

# 删除所有以test开头的文件，但保留test.important
rm test!(important).*
```

> ⚠️ **警告**：使用通配符删除文件时要特别小心，最好先用`ls`命令测试模式是否匹配预期的文件。

### 4. 查找文件

```bash
# 查找当前目录中所有.log文件
find . -name "*.log"

# 查找所有不是.txt的文件
find . -not -name "*.txt"
```

### 5. 压缩文件

```bash
# 压缩所有.txt文件
tar -czvf texts.tar.gz *.txt

# 压缩所有图像文件
zip images.zip *.{jpg,png,gif}
```

## 通配符在脚本中的高级应用 📜

通配符不仅可以用于匹配文件名，还可以用于其他模式匹配场景：

### 1. 在条件测试中使用

```bash
#!/bin/bash
shopt -s extglob

filename="document.pdf"

# 检查文件是否为图像文件
if [[ $filename == *.@(jpg|png|gif) ]]; then
    echo "这是一个图像文件"
elif [[ $filename == *.@(doc|docx|pdf) ]]; then
    echo "这是一个文档文件"
else
    echo "这是其他类型的文件"
fi
```

### 2. 在case语句中使用

```bash
#!/bin/bash
shopt -s extglob

for file in *; do
    case $file in
        *.@(jpg|png|gif))
            echo "$file 是图像文件"
            ;;
        *.@(mp3|wav|ogg))
            echo "$file 是音频文件"
            ;;
        *.@(mp4|avi|mkv))
            echo "$file 是视频文件"
            ;;
        *)
            echo "$file 是其他类型的文件"
            ;;
    esac
done
```

### 3. 在参数扩展中使用

```bash
#!/bin/bash
shopt -s extglob

# 移除所有图像文件的扩展名
for file in *.@(jpg|png|gif); do
    name=${file%%.*}
    echo "文件 $file 的名称是 $name"
done
```

## 通配符的限制和注意事项 ⚠️

1. **隐藏文件**：默认情况下，通配符不匹配以点（.）开头的隐藏文件。要匹配隐藏文件，需要明确指定，如`.*.txt`或`.*`。

2. **空目录**：当通配符没有匹配到任何文件时，它会保持原样而不是扩展为空字符串。例如，如果没有.txt文件，`echo *.txt`会输出`*.txt`而不是空行。

3. **路径分隔符**：通配符不会跨目录匹配。例如，`*/*.txt`只会匹配当前目录的子目录中的.txt文件，不会匹配更深层次的目录。

4. **命令注入**：在脚本中使用通配符时，要注意可能的命令注入风险，特别是当通配符来自用户输入时。

5. **性能考虑**：在包含大量文件的目录中使用复杂的通配符模式可能会导致性能问题。

## 通配符使用技巧 💡

1. **测试匹配**：在执行可能有风险的操作（如删除）之前，先使用`echo`或`ls`命令测试通配符是否匹配预期的文件。

   ```bash
   # 先测试
   echo rm *.tmp
   # 确认无误后执行
   rm *.tmp
   ```

2. **组合使用**：可以组合多种通配符创建复杂的匹配模式。

   ```bash
   # 匹配以a-z开头，后跟一个数字，然后是.txt的文件
   ls [a-z][0-9].txt
   ```

3. **使用否定模式**：使用`!`或`[!...]`排除不需要的文件。

   ```bash
   # 复制除了.tmp和.bak之外的所有文件
   cp !(*.tmp|*.bak) /backup/
   ```

4. **记住启用扩展通配符**：在使用扩展通配符之前，不要忘记启用它们。

   ```bash
   shopt -s extglob
   ```

5. **在脚本中保持一致性**：在脚本中使用通配符时，考虑在脚本开头启用扩展通配符，并在结束时恢复原始设置。

   ```bash
   #!/bin/bash
   # 保存原始设置
   extglob_setting=$(shopt -p extglob)
   # 启用扩展通配符
   shopt -s extglob
   
   # 脚本主体...
   
   # 恢复原始设置
   $extglob_setting
   ```

## 练习题 🧩

1. 如何列出当前目录中所有以数字开头，但不以.tmp结尾的文件？

2. 编写一个命令，将所有.jpg、.png和.gif文件移动到images目录，同时保持它们的原始名称。

3. 如何删除所有空文件（大小为0的文件）？

4. 编写一个Bash脚本，将所有文本文件（.txt、.md、.log）的扩展名更改为.bak，但保留原始文件名。

5. 如何使用通配符找出当前目录中所有不是图像文件（.jpg、.png、.gif）、不是文档文件（.doc、.docx、.pdf）且不是隐藏文件的文件？

---

通过掌握Linux通配符，你可以大大提高命令行操作的效率，简化文件管理任务，并编写更强大的shell脚本。记住，实践是掌握通配符的最佳方式，所以不要犹豫，开始在你的日常Linux使用中尝试这些模式吧！
