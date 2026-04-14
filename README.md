# pipex

[![42](https://img.shields.io/badge/42-Project-blue)](https://42.fr)
[![C](https://img.shields.io/badge/Language-C-green)](https://en.wikipedia.org/wiki/C_(programming_language))

> Recreate the behaviour of the shell pipe operator `|`.

## Overview

`pipex` is a 42 School Rank 2 project. The goal is to replicate how the shell handles piped commands, using `fork`, `pipe`, `dup2`, and `execve`.

This implementation supports multiple commands (not just two), mirroring the behaviour of:

```bash
< file1 cmd1 | cmd2 | ... | cmdN > file2
```

## Usage

```bash
make
./pipex file1 cmd1 cmd2 ... cmdN file2
```

### Examples

```bash
./pipex infile.txt "ls -l" "wc -l" outfile.txt
./pipex infile.txt "grep hello" "wc -w" outfile.txt
./pipex infile.txt "cat" "grep foo" "wc -l" outfile.txt
```

These are equivalent to:
```bash
< infile.txt ls -l | wc -l > outfile.txt
< infile.txt grep hello | wc -w > outfile.txt
< infile.txt cat | grep foo | wc -l > outfile.txt
```

## Project structure

```
pipex/
├── srcs/
│   ├── main.c         # Entry point, pipex loop
│   ├── utils.c        # Command path resolution, execution, fd cleanup
│   ├── my_prints.c    # Error handling and exit logic
│   └── pipex.h        # Header
├── libft/             # Custom C library
└── Makefile
```

## How it works

1. Opens `file1` (read) and `file2` (write/create)
2. For each command except the last: creates a pipe, forks a child that reads from the previous fd and writes to the pipe
3. The last command reads from the final pipe and writes to `file2`
4. Parent waits for all children to finish

## Make targets

| Target | Description |
|--------|-------------|
| `make` | Build the binary |
| `make clean` | Remove object files |
| `make fclean` | Remove object files and binary |
| `make re` | Rebuild from scratch |
| `make norm` | Run norminette check |
| `make val` | Run with valgrind |
| `make run` | Quick test run |

## Author

**Laher Maciel**
- GitHub: [@LaherMaciel](https://github.com/LaherMaciel)
- 42 Login: lawences
