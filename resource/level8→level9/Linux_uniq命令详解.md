# Linux uniq命令详解 🔍

## 什么是uniq命令？ 🤔

`uniq`（unique的缩写）是Linux/Unix系统中的一个命令行工具，用于报告或忽略文件中的重复行。它的主要功能是从输入中筛选出唯一的行，或者识别重复的行，这在处理日志文件、数据分析和文本处理中非常有用。

`uniq`命令的一个重要特性是它**只能检测相邻的重复行**。这意味着，如果要找出文件中所有的重复行，通常需要先使用`sort`命令对文件进行排序，然后再使用`uniq`命令。

## 基本语法 📝

```bash
uniq [选项]... [输入文件 [输出文件]]
```

如果不指定输入文件，或者输入文件指定为`-`，`uniq`将从标准输入读取内容。如果不指定输出文件，结果将输出到标准输出。

## 基本用法示例 💡

### 1. 删除重复行

```bash
# 从文件中删除相邻的重复行
uniq file.txt

# 从标准输入读取并删除重复行
cat file.txt | uniq

# 先排序再删除重复行（常见用法）
sort file.txt | uniq
```

### 2. 将结果保存到文件

```bash
# 将去重结果保存到新文件
uniq file.txt > unique_lines.txt

# 先排序再去重，并保存结果
sort file.txt | uniq > unique_lines.txt
```

## 常用选项详解 🛠️

| 选项 | 描述 | 示例 |
|------|------|------|
| `-c` | 在每行前面显示该行重复出现的次数 | `uniq -c file.txt` |
| `-d` | 只显示重复的行（每组重复行仅输出一行） | `uniq -d file.txt` |
| `-D` | 只显示重复的行（显示所有重复行） | `uniq -D file.txt` |
| `-u` | 只显示不重复的行 | `uniq -u file.txt` |
| `-i` | 忽略大小写 | `uniq -i file.txt` |
| `-f N` | 忽略前N个字段 | `uniq -f 1 file.txt` |
| `-s N` | 忽略前N个字符 | `uniq -s 3 file.txt` |
| `-w N` | 只比较前N个字符 | `uniq -w 5 file.txt` |

## 实用示例 🌟

### 1. 统计每行出现的次数

```bash
# 文件内容：
# apple
# apple
# banana
# cherry
# cherry
# cherry

# 统计每行出现的次数
uniq -c fruits.txt
# 输出：
#       2 apple
#       1 banana
#       3 cherry

# 先排序再统计（更常用）
sort fruits.txt | uniq -c
# 输出：
#       2 apple
#       1 banana
#       3 cherry
```

### 2. 只显示重复的行

```bash
# 只显示有重复的行（每组重复行只显示一次）
uniq -d fruits.txt
# 输出：
# apple
# cherry

# 显示所有重复的行
uniq -D fruits.txt
# 输出：
# apple
# apple
# cherry
# cherry
# cherry
```

### 3. 只显示唯一的行

```bash
# 只显示没有重复的行
uniq -u fruits.txt
# 输出：
# banana
```

### 4. 忽略大小写

```bash
# 文件内容：
# Apple
# apple
# Banana
# Cherry

# 忽略大小写统计
uniq -i -c fruits_mixed.txt
# 输出：
#       2 Apple
#       1 Banana
#       1 Cherry
```

### 5. 忽略字段

```bash
# 文件内容：
# 1 apple
# 2 apple
# 3 banana
# 4 banana

# 忽略第一个字段（空格分隔）
uniq -f 1 data.txt
# 输出：
# 1 apple
# 3 banana
```

### 6. 忽略字符

```bash
# 文件内容：
# A-apple
# B-apple
# C-banana
# D-cherry

# 忽略前2个字符
uniq -s 2 prefixed.txt
# 输出：
# A-apple
# C-banana
# D-cherry
```

### 7. 只比较前N个字符

```bash
# 文件内容：
# apple pie
# apple juice
# banana split
# cherry tart

# 只比较前5个字符
uniq -w 5 desserts.txt
# 输出：
# apple pie
# banana split
# cherry tart
```

## 与其他命令组合使用 🔄

### 1. 与sort命令组合

这是最常见的组合，因为`uniq`只能检测相邻的重复行：

```bash
# 排序并去除重复行
sort file.txt | uniq

# 排序并统计每行出现的次数
sort file.txt | uniq -c

# 排序并只显示重复的行
sort file.txt | uniq -d
```

### 2. 与sort和head/tail命令组合

```bash
# 找出文件中出现次数最多的5行
sort file.txt | uniq -c | sort -nr | head -5

# 找出文件中出现次数最少的5行
sort file.txt | uniq -c | sort -n | head -5
```

### 3. 与grep命令组合

```bash
# 找出包含"error"的行，并统计每种错误出现的次数
grep "error" log.txt | sort | uniq -c

# 找出至少出现3次的错误
grep "error" log.txt | sort | uniq -c | awk '$1 >= 3 {print $0}'
```

### 4. 与cut命令组合

```bash
# 提取第2列，并统计每个值出现的次数
cut -d, -f2 data.csv | sort | uniq -c
```

## 实际应用场景 🛠️

### 1. 日志分析

```bash
# 统计日志中每个IP地址的访问次数
awk '{print $1}' access.log | sort | uniq -c | sort -nr

# 找出访问次数最多的10个IP地址
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10

# 找出所有产生404错误的URL
grep " 404 " access.log | awk '{print $7}' | sort | uniq
```

### 2. 数据去重

```bash
# 去除CSV文件中的重复行
sort data.csv | uniq > unique_data.csv

# 只保留CSV文件的唯一标识符列
cut -d, -f1 data.csv | sort | uniq
```

### 3. 文本分析

```bash
# 统计文本中每个单词出现的次数
cat text.txt | tr -s '[:space:]' '\n' | sort | uniq -c | sort -nr

# 找出文本中只出现一次的单词
cat text.txt | tr -s '[:space:]' '\n' | sort | uniq -c | grep "^\s*1 "
```

### 4. 比较文件

```bash
# 找出两个文件中的共同行
sort file1.txt file2.txt | uniq -d

# 找出只在file1.txt中出现的行
sort file1.txt file2.txt file2.txt | uniq -u
```

## 性能考虑 ⚡

### 1. 内存使用

`uniq`命令通常不会占用大量内存，因为它一次只需要比较相邻的两行。

### 2. 处理大文件

对于非常大的文件，主要的性能瓶颈可能是排序操作，而不是`uniq`本身：

```bash
# 对大文件进行排序时限制内存使用
sort -S 1G large_file.txt | uniq
```

### 3. 替代方法

对于特定的任务，其他工具可能更高效：

```bash
# 使用awk统计唯一行
awk '!seen[$0]++' file.txt

# 使用awk统计每行出现的次数
awk '{count[$0]++} END {for (line in count) print count[line], line}' file.txt
```

## 常见问题与解决方案 ❓

### 问题1：uniq没有去除所有重复行

**问题**：使用`uniq`后，文件中仍然有重复的行。

**原因**：`uniq`只能检测相邻的重复行。

**解决方案**：先使用`sort`命令对文件进行排序，再使用`uniq`：

```bash
sort file.txt | uniq
```

### 问题2：字段比较问题

**问题**：需要基于特定字段去重，而不是整行。

**解决方案**：使用`-f`选项忽略前N个字段，或结合`awk`使用：

```bash
# 使用uniq的-f选项
sort -k 2 file.txt | uniq -f 1

# 使用awk（更灵活）
awk '!seen[$2]++' file.txt
```

### 问题3：大小写敏感性

**问题**：需要忽略大小写进行去重。

**解决方案**：使用`-i`选项：

```bash
sort file.txt | uniq -i
```

## 替代工具 🔄

虽然`uniq`命令在大多数情况下都很有用，但有时其他工具可能更适合：

1. **awk**：更灵活，可以基于复杂条件进行去重
   ```bash
   awk '!seen[$0]++' file.txt  # 去除所有重复行，不需要预先排序
   ```

2. **sort -u**：直接排序并去重
   ```bash
   sort -u file.txt  # 相当于sort file.txt | uniq
   ```

3. **perl**：支持更复杂的模式匹配和处理
   ```bash
   perl -ne 'print unless $seen{$_}++' file.txt
   ```

4. **Python**：对于复杂的去重逻辑
   ```bash
   python -c "import sys; lines = set(sys.stdin.readlines()); print(''.join(lines))" < file.txt
   ```

## 练习题 🧩

1. 如何统计一个文件中每行出现的次数，并按出现次数降序排列？

2. 编写一个命令，找出日志文件中出现次数超过100次的错误消息。

3. 如何找出两个文件中的共同行和唯一行？

4. 编写一个命令，统计CSV文件中第3列的唯一值及其出现次数。

5. 如何找出文本文件中只出现一次的单词？

---

通过掌握`uniq`命令的各种用法，你可以更高效地处理文本数据中的重复内容。虽然`uniq`命令本身很简单，但与其他命令（如`sort`、`grep`、`awk`等）结合使用时，它可以成为强大的文本处理工具。记住，对于大多数`uniq`操作，预先排序是必要的！
