# Lecture 2_Command-line Environment
---
## The Command Line Interface (CLI) 命令行界面
### Arguments
- Arguments are plain strings in shell.
```bash
% vim test.sh

echo "name of program: $0"
echo "first argument: $1"
echo "second argument: $2"
echo "number of arguments is $#"
echo "all arguments: $@"


% chmod +x test.sh
% ./test.sh apple orange banana 123
name of program: ./test.sh
first argument: apple
second argument: orange
number of arguments is 4
all arguments: apple orange banana 123
```

- Most common globs
    - wildcards (通配符) `*` (zero or more of anything), `?` (exactly one of anything) and curly braces. Curly braces `{}` expand a comma-separated list of patterns into multiple arguments.
```bash
% mkdir folder
% mv *{.py, .sh} folder
% ls folder
check.sh    flaky.sh    test.sh    test_x.sh
```

### Streams
- When using the pipe operator `|`, the shell operates on streams of data that flow from one program to the next in the chain. We can demonstrate this concurrency, all commands in a pipeline start immediately:
```bash
% cat > numbers.txt
1
2
3
123
12
13
23
% (sleep 15 && cat numbers.txt) | grep -E '^[0-9]$' | sort | uniq &
[1] 75889 75890 75892 75893
% ps
  PID TTY          TIME CMD
75683 ttys000   0:00.14 -zsh
75889 ttys000   0:00.00 -zsh
75890 ttys000   0:00.00 grep -E ^[0-9]$
75891 ttys000   0:00.00 sleep 15
75892 ttys000   0:00.00 sort
75893 ttys000   0:00.00 uniq
% 1
2
3

[1]  + done      (  sleep 15 && cat numbers.txt;  ) | grep -E '^[0-9]$' | sort | uniq
```

- redirection
```bash
% ls /nonexistent
ls: /nonexistent: No such file or directory
% ls /nonexistent 2>/dev/null
```

- `fzf` (fuzzy finder)

### Environment variables
```bash
% foo=123
% echo $foo
123
% echo '$foo'
$foo
% echo "$foo"
123
% echo "Today is $(date)"
Today is Wed Feb 25 13:56:32 CST 2026
```

- Command Substitution (命令替换)
```bash
% files=$(ls)            
% echo $files | grep "missing"
missing-semester
```
- Process Substitution (进程替换)
- TZ (time zone)
```bash
% date              
Wed Feb 25 14:20:07 CST 2026
% TZ=Asia/Seoul date
Wed Feb 25 15:20:08 KST 2026
```
### Return codes
- `echo $?` access the return code of the last command
- Boolean operators `&&` and `||`: conditionally run commands based on the success or failure of previous commands, where success is determined based on whether the return code is zero or not. (same as `if` and `while` statements)
   
### Signals
- killing a program: `^C` `Ctrl-C` `SIGINT` or `^\` `Ctrl-\` `SIGQUIT`
- `SIGSTOP` pauses a process. In the terminal, typing `Ctrl-Z` will prompt the shell to send a `SIGTSTP` signal, short for Terminal Stop.
- PID: process ID
- `pgrep`: process grep
- `pkill`: process kill
- 