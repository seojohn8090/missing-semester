# Lecture 2_Command-line Environment
---
### Arguments and Globs
### 1. You might see commands like `cmd --flag -- --notaflag`. The `--` is a special argument that tells the program to stop parsing flags. Everything after `--` is treated as a positional argument. Why might this be useful? Try running `touch -- -myfile` and then removing it without `--`.
- It is useful because `--` explicitly tells the program that option parsing stops there, ensuring that any following arguments—even those starting with a dash `—` are treated as positional arguments rather than flags, which prevents ambiguity and unintended behavior under standard CLI conventions.
```bash
% touch -- -myfile
% rm -myfile
rm: illegal option -- m
usage: rm [-f | -i] [-dIPRrvWx] file ...
       unlink [--] file
% rm -- -myfile
```

### 2. Read man ls and write an ls command that lists files in the following manner: Includes all files, including hidden files; Sizes are listed in human readable format (e.g. 454M instead of 454279954); Files are ordered by recency; Output is colorized.
- The answer should be `ls -laht --color=auto`
  
### 3. Process substitution `<(command)` lets you use a command’s output as if it were a file. Use `diff` with process substitution to compare the output of `printenv` and `export`. Why are they different? (Hint: try `diff <(printenv | sort) <(export | sort)`).
- `printenv` shows the environment variables that are visible to child processes. In other words, it displays the variables that would be inherited by programs started from the current shell. `export` shows all variables that the current shell has marked for export, formatted in a way suitable for shell reuse (often with quotes or `declare -x)`.
```bash
% diff <(printenv | sort) <(export | sort)
5c5
< CONDA_PROMPT_MODIFIER=(base) 
---
> CONDA_PROMPT_MODIFIER='(base) '
9c9
< GSETTINGS_SCHEMA_DIR_CONDA_BACKUP=
---
> GSETTINGS_SCHEMA_DIR_CONDA_BACKUP=''
33,35c33,34
< _=/usr/bin/printenv
< _CE_CONDA=
< _CE_M=
---
> _CE_CONDA=''
> _CE_M=''
```