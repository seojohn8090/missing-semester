# Lecture 2_Command-line Environment
---
### Arguments and Globs
### 1. You might see commands like `cmd --flag -- --notaflag`. The `--` is a special argument that tells the program to stop parsing flags. Everything after `--` is treated as a positional argument. Why might this be useful? Try running `touch -- -myfile` and then removing it without `--`.
- It is useful because `--` explicitly tells the program that option parsing stops there, ensuring that any following arguments—even those starting with a dash `—` are treated as positional arguments rather than flags, which prevents ambiguity and unintended behavior under standard CLI conventions.
<div align="center">
<img src="image/02_01.png" style="width:75%;">
</div>

### 2. Read man ls and write an ls command that lists files in the following manner: Includes all files, including hidden files; Sizes are listed in human readable format (e.g. 454M instead of 454279954); Files are ordered by recency; Output is colorized.
- The answer should be `ls -laht --color=auto`
  
### 3. Process substitution `<(command)` lets you use a command’s output as if it were a file. Use `diff` with process substitution to compare the output of `printenv` and `export`. Why are they different? (Hint: try `diff <(printenv | sort) <(export | sort)`).
