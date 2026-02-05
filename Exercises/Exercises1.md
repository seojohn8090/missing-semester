# Lecture 1_Introduction to the Shell
---
1. To make sure you’re running an appropriate shell, you can try the command `echo $SHELL`. 
2. What does the `-l` flag to `ls` do? Run `ls -l /` and examine the output. What do the first 10 characters of each line mean? (Hint: `man ls`)
    - `ls`: list directory contents. 列出当前目录内容
    - `ls -l`: the long format 显示详细列表
      - file mode, number of links, owner name, group name, number of bytes in the file, abbreviated month, day-of-month file was last modified, hour file last modified, minute file last modified, and the pathname.
      - file mode: entry type + permissions.
        - entry type:
            `-`: Regular file.
            `b`: Block special file.
            `c`: Character special file.
            `d`: Directory.
            `l`: Symbolic link.
            `p`: FIFO.
            `s`: Socket.
            `w`: Whiteout.
        - permissions
            1. `r` or `-`: the file is readable or not.
            2. `w` or `-`: the file is writable or not.
            3. third character
                - owner and group permissions
                    - `S`: If in the owner permissions, the file is not executable and set-user-ID mode is set.  If in the group permissions, the file is not executable and set-group-ID mode is set.
                    - `s`: If in the owner permissions, the file is executable and set-user-ID mode is set.  If in the group permissions, the file is executable and setgroup-ID mode is set.
                    - `x`: The file is executable or the directory is searchable.
                    - `-`: The file is neither readable, writable, executable, nor set-user-ID nor set-group-ID mode, nor sticky.
                - other permissions
                    - `T`: The sticky bit is set (mode 1000), but not execute or search permission.
                    - `t`: The sticky bit is set (mode 1000), and is searchable or executable.
<div align="center">
<img src="Image/01.png" style="width:90%;">
</div>

