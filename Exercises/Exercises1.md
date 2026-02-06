# Lecture 1_Introduction to the Shell
---
### 1. To make sure you’re running an appropriate shell, you can try the command `echo $SHELL`. 
### 2. What does the `-l` flag to `ls` do? Run `ls -l /` and examine the output. What do the first 10 characters of each line mean? (Hint: `man ls`)
<div align="center">
<img src="image/01.png" style="width:70%;">
</div>

- `ls`: list directory contents. 列出当前目录内容
- `ls -l`: the long format 显示详细列表
    - file mode, number of links, owner name, group name, number of bytes in the file, abbreviated month, day-of-month file was last modified, hour file last modified, minute file last modified, and the pathname.
    - file mode: entry type + permissions.
    - entry type:  
        - `-`: Regular file.
        - `b`: Block special file.
        - `c`: Character special file.
        - `d`: Directory.
        - `l`: Symbolic link.
        - `p`: FIFO.
        - `s`: Socket.
        - `w`: Whiteout.
    - permissions
        1. `r` or `-`: the file is readable or not.
        2. `w` or `-`: the file is writable or not.
        3. third character
            - owner and group permissions
                - `S`: If in the owner permissions, the file is not executable and set-user-ID mode is set.  If in the group permissions, the file is not executable and set-group-ID mode is set.
                - `s`: If in the owner permissions, the file is executable and set-user-ID mode is set.  If in the group permissions, the file is executable and set-group-ID mode is set.
                - `x`: The file is executable or the directory is searchable.
                - `-`: The file is neither readable, writable, executable, nor set-user-ID nor set-group-ID mode, nor sticky.
            - other permissions
                - `T`: The sticky bit is set (mode 1000), but not execute or search permission.
                - `t`: The sticky bit is set (mode 1000), and is searchable or executable.
### 3. In the command `find ~/Downloads -type f -name "*.zip" -mtime +30`, the `*.zip` is a “glob”. What is a glob? Create a test directory with some files and experiment with patterns like `ls *.txt`, `ls file?.txt`, and `ls {a,b,c}.txt`. See [Pattern Matching](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html) in the Bash manual.
<div align="center">
<img src="image/02.png" style="width:70%;">
</div>

- The command means "Find all ZIP files in your Downloads directory that were last modified more than 30 days ago".
- Glob: simpler patterns (than regular expression) that the shell (or programs like find) uses to match filenames. 更加便捷的用来查找符合特定规则的目录和文件的方法
- `*` matches any string, including the null string.
    ```
    ls *.zip
    ls file*
    ls **/*
    ls **/
    ```
- `?` matches any single character.
- `[...]` (bracket expression) matches a single character.
    ```
    ls file[0-9].txt
    ls report_[A-Z].pdf
    ls file[^0-9].txt 
    ls file[!0-9].txt
    ls file[[:digit:]].log
    ls [[:upper:]_][[:lower:]]*.txt
    ```
### 4. What’s the difference between `'single quotes'`, `"double quotes"`, and `$'ANSI quotes'`? Write a command that echoes a string containing a literal `$`, a `!`, and a newline character. See [Quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html).
<div align="center">
<img src="image/06.png" style="width:70%;">
</div>   

- quoting: remove the special meaning.
- Escape Character(转义字符): A non-quoted backslash(反斜杠) ‘\’ is the Bash escape character. It preserves the literal value of the next character that follows, removing any special meaning it has, with the exception of newline(space, 换行符). If a \newline pair appears, and the backslash itself is not quoted, the \newline is treated as a line continuation(续行).
<div align="center">
<img src="image/03.png" style="width:70%;">
</div>   

- Single Quotes: Enclosing characters in single quotes (‘'’) preserves the literal value of each character within the quotes. A single quote may not occur between single quotes, even when preceded by a backslash.
- Double Quotes: Enclosing characters in double quotes (‘"’) preserves the literal value of all characters within the quotes, with the exception of ‘$’, ‘`’, ‘\’, and, when history expansion is enabled, ‘!’. 
<div align="center">
<img src="image/04.png" style="width:70%;">
</div>   

- ANSI-C Quoting: in the form of `$'string'`. The sequence expands to string, with backslash-escaped characters in string replaced as specified by the ANSI C standard.
<div align="center">
<img src="image/05.png" style="width:70%;">
</div>   

### 5. The shell has three standard streams: stdin (0), stdout (1), and stderr (2). Run `ls /nonexistent /tmp` and redirect stdout to one file and stderr to another. How would you redirect both to the same file? See [Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html).


