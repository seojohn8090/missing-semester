# Lecture 2_Command-line Environment
---
## The Command Line Interface (CLI) 命令行界面
### Arguments 参数
- Arguments are plain strings in shell.
<div align="center">
<img src="image/02_02.png" style="width:30%;">
<img src="image/02_01.png" style="width:70%;">
</div>

- Most common globs
    - wildcards (通配符) `*` (zero or more of anything), `?` (exactly one of anything) and curly braces. Curly braces `{}` expand a comma-separated list of patterns into multiple arguments.
<div align="center">
<img src="image/02_03.png" style="width:70%;">
<img src="image/02_04.png" style="width:60%;">
</div>

### Streams
- When using the pipe operator `|`, the shell operates on streams of data that flow from one program to the next in the chain. We can demonstrate this concurrency, all commands in a pipeline start immediately:
<div align="center">
<img src="image/02_05.png" style="width:75%;">
</div>

- redirection
<div align="center">
<img src="image/02_06.png" style="width:75%;">
</div>

- `fzf` (fuzzy finder)
<div align="center">
<img src="image/02_07.png" style="width:50%;">
</div>

### Environment variables

### Return codes
### Signals
---