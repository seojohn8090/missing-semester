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
### Environment Variables
### 1. Write bash functions `marco` and `polo` that do the following: whenever you execute `marco` the current working directory should be saved in some manner, then when you execute `polo`, no matter what directory you are in, `polo` should `cd` you back to the directory where you executed `marco`. For ease of debugging you can write the code in a file `marco.sh` and (re)load the definitions to your shell by executing `source marco.sh`.
```bash
% touch marco.sh
% nano marco.sh
```
```bash
#!/bin/bash
marco() {
    MARCO_DIR="$PWD"
}
polo() {
    cd "$MARCO_DIR"
}
```
```bash
% source marco.sh
```
### Return Codes
### 1. Say you have a command that fails rarely. In order to debug it you need to capture its output but it can be time consuming to get a failure run. Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took for the script to fail.
- The script simulates a command that fails rarely by generating a random number and exiting with an error only when the number equals 42 (about 1% probability). Most runs succeed, but occasionally it prints an error message and exits with status 1. This models a nondeterministic bug that requires repeated execution to capture a failure for debugging.
```bash
% touch random.sh
% nano random.sh
% chmod +x random.sh
% touch run_until_fail.sh
% nano run_until_fail.sh
```
```bash
#!/usr/bin/env bash
count=0
while ./random.sh > out.txt 2> err.txt
do
    count=$((count+1))
done
echo "The script failed after $((count+1)) runs."
echo "----- STDOUT -----"
cat out.txt
echo "----- STDERR -----"
cat err.txt
```
```bash
% chmod +x run_until_fail.sh
% ./run_until_fail.sh
The script failed after 26 runs.
----- STDOUT -----
Something went wrong
----- STDERR -----
The error was using magic numbers
```
### Signals and Job Control
### 1. Start a `sleep 10000` job in a terminal, background it with `Ctrl-Z` and continue its execution with `bg`. Now use `pgrep` to find its pid and `pkill` to kill it without ever typing the pid itself. (Hint: use the `-af` flags).
```bash
% sleep 10000
^Z
zsh: suspended  sleep 10000
% bg
[1]  + continued  sleep 10000
% pgrep -afl sleep
15497
% pkill -f sleep
[1]  + terminated  sleep 10000
```
- `-a`, List the full command line as well as the process ID.
- `-f`, The pattern is normally only matched against the process name. When `-f` is set, the full command line is used.
- `-l`, List the process name as well as the process ID.  

### 2. Say you don’t want to start a process until another completes. How would you go about it? In this exercise, our limiting process will always be `sleep 60 &`. One way to achieve this is to use the  [`wait`](https://www.man7.org/linux/man-pages/man1/wait.1p.html) command. Try launching the sleep command and having an `ls` wait until the background process finishes. 
### However, this strategy will fail if we start in a different bash session, since `wait` only works for child processes. One feature we did not discuss in the notes is that the `kill` command’s exit status will be zero on success and nonzero otherwise. `kill -0` does not send a signal but will give a nonzero exit status if the process does not exist. Write a bash function called `pidwait` that takes a pid and waits until the given process completes. You should use `sleep` to avoid wasting CPU unnecessarily.
```bash
% sleep 60 &
[2] 15947
% pid=$!    
% wait $pid 
ls
[2]  - done       sleep 60                 
% ls
```
```bash
% pidwait() {
  pid=$1
  while kill -0 "$pid" 2>/dev/null; do
    sleep 1
  done
}
% sleep 60 &
[2] 16180
% pidwait 16180
[2]  - done       sleep 60
```

