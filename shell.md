# Lecture 2: Shell Tools and Scripting

## Cursor
Control + a: begin of the line
Control + e: end of the line
ALT + f: move one word forward
ALT + b: move one word back

## Tools
TLDR
broot
nnn
fzf.zsh

## hashbang/shebang
The first line in this file is the "shebang" line.  When you execute a file from the shell, the shell tries to run the file using the command specified 
on the shebang line. The shebang line specifies exactly how to run a script.  In other words, this shebang line says that, when I type in `./basics.py`, the shell will actuall run 
    `/usr/bin/env python basics.py`

## Methods of Enabling Shell Script Debugging Mode
**-v (short for verbose)** – tells the shell to show all lines in a script while they are read, it activates verbose mode.
**-n (short for noexec or no ecxecution)** – instructs the shell read all the commands, however doesn’t execute them. This options activates syntax checking mode.
**-x (short for xtrace or execution trace)** – tells the shell to display all commands and their arguments on the terminal while they are executed. This option enables shell tracing mode.

1. Modifying the First Line of a Shell Script
```
#!/bin/sh option(s)
```
2. Invoking Shell With Debugging Options
```
$ /bin/bash option(s) script_name argument1 ... argumentN
```
4. Using set Shell Built-in Command
```
set -option
set +option
```

4. *bash only
```
export PS4='+(${BASH_SOURCE}:${LINENO}): ${FUNCNAME[0]:+${FUNCNAME[0]}(): }'
```


## Reference
[Lecture 2: Shell Tools and Scripting (2020)](https://www.youtube.com/watch?v=kgII-YWo3Zw)