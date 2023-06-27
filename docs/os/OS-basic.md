---
title: os
type: repo
keywords: ubuntu, macos, linux
---

# OS basic

useful commands

## common linux

- 获取指定文件夹下的文件名和文件内容的 md5 值
  可以使用 `find` 命令和 `md5sum` 命令来获取指定文件夹下的文件名和文件内容的 md5 值。具体步骤如下：

1. 使用 `find` 命令查找指定文件夹下的所有文件，并将结果输出到一个文件中
   `find /path/to/folder -type f > files.txt`
   这个命令会将 `/path/to/folder` 文件夹下的所有文件的路径输出到 `files.txt` 文件中。

2. 使用 `xargs` 命令读取 `files.txt` 文件中的文件路径，并对每个文件执行 `md5sum` 命令：
   `cat files.txt | xargs md5sum > md5sum.txt`
   这个命令会将 `files.txt` 文件中的每个文件路径作为参数传递给 `md5sum` 命令，并将每个文件的 md5 值和文件名输出到 `md5sum.txt` 文件中。

3. 查看 `md5sum.txt` 文件，即可获取指定文件夹下的文件名和文件内容的 md5 值。
   `cat md5sum.txt`
   这个命令会将 `md5sum.txt` 文件中的内容输出到终端中，每行包含一个文件的 md5 值和文件名，用空格分隔。
   `find lvmh -type f -print0 | xargs -0 md5 | awk '{if($1 in a) print a[$2],$2; else a[$1]=$2;}' | sort | uniq -d > dup.txt`

- awk 命令如何使用？

  `awk` 是一种文本处理工具，可以用于从文本文件中提取和操作数据。它的基本语法如下：  
   `awk 'pattern {action}' file`

  - 其中，`pattern` 是一个正则表达式，用于匹配文件中的文本行；`action` 是一个命令序列，用于对匹配的行进行处理；`file` 是要处理的文件名。
  - 例如，要从一个名为 `data.txt` 的文件中提取第一列和第三列的数据，可以使用以下命令：  
     `awk '{print $1, $3}' data.txt`  
     在这个命令中，`{print $1, $3}` 是一个 `action`，它会打印每一行的第一列和第三列，用空格分隔。`$1` 和 `$3` 是 `awk` 中的变量，表示当前行的第一列和第三列。  
     除了 `print` 命令，`awk` 还支持很多其他的命令，例如 `if`、`for`、`while` 等，可以用于更复杂的数据处理任务。

  - 除了基本语法之外，`awk` 还有一些常用的选项和内置变量，可以帮助我们更好地处理数据。
  - 下面是一些常用的选项和内置变量：

    - `-F` 选项：指定字段分隔符。例如，`-F ','` 表示使用逗号作为字段分隔符。
    - `$0` 变量：表示当前行的全部内容。 - `$1`、`$2`、`$3` 等变量：表示当前行的第一列、第二列、第三列等。
    - `NF` 变量：表示当前行的字段数。
    - `NR` 变量：表示当前行的行号。
    - `BEGIN` 和 `END` 块：分别在处理文件之前和之后执行的命令块。

  - 例如，要从一个以逗号分隔的文件中提取第一列和第三列的数据，可以使用以下命令：  
     `awk -F ',' '{print $1, $3}' data.csv`  
     在这个命令中，
    `-F ','` 指定了逗号作为字段分隔符，
    `{print $1, $3}` 打印了每一行的第一列和第三列。

  另外，如果我们想要对数据进行一些统计分析，可以使用 `awk` 的内置函数，例如 `sum`、`avg`、`max`、`min` 等。
  例如，要计算一个文件中所有数字的平均值，可以使用以下命令：  
   `awk '{sum += $1} END {print sum/NR}' data.txt`  
   在这个命令中，`{sum += $1}` 表示累加每一行的第一列，
  `END {print sum/NR}` 表示在处理完文件之后打印平均值。
  `NR` 变量表示行数，因此 `sum/NR` 就是平均值。

## MacOS

[[Setup Mac]]

## ubuntu

## Backend service

### Nginx webserver
