---
layout:     post
title:      Bash Cheat Sheet
date:       2023-01-01
summary:    Cheat Sheet for Bash 
categories: linux
thumbnail: cogs
tags:
 - linux
 - cli
 - bash
---

# Linux Bash Commands Cheat Sheet

Bash is the default shell on most Linux systems.

## Basic Commands

Here are some basic bash commands:

### Navigation

- `cd <directory>`: Change the current working directory to `directory`
- `cd ..`: Move up one directory level
- `pwd`: Print the current working directory

### File and directory management

- `ls`: List the files and directories in the current directory
- `ls <directory>`: List the files and directories in `directory`
- `ls -l`: List the files and directories in the current directory in long format
- `ls -a`: List all files and directories, including hidden ones
- `mkdir <directory>`: Create a new directory `directory`
- `touch <file>`: Create a new file `file`
- `cp <source> <destination>`: Copy `source` to `destination`
- `mv <source> <destination>`: Move `source` to `destination`
- `rm <file>`: Delete `file`
- `rmdir <directory>`: Delete `directory` (if it is empty)

### Text manipulation

- `cat <file>`: Print the contents of `file` to the terminal
- `head <file>`: Print the first 10 lines of `file` to the terminal
- `tail <file>`: Print the last 10 lines of `file` to the terminal
- `sort <file>`: Sort the lines of `file`
- `grep <pattern> <file>`: Print the lines in `file` that contain `pattern`

### Process management

- `ps`: List the running processes
- `top`: Display the top running processes
- `kill <pid>`: Terminate the process with the process ID `pid`
- `killall <process>`: Terminate all processes with the name `process`

### System information

- `uname`: Print the current system information
- `uname -a`: Print all system information
- `uptime`: Print the current uptime
- `free`: Print the memory usage
- `df`: Print the disk usage
- `du`: Print the directory space usage

### Networking

- `ping <host>`: Send a ping request to `host`
- `traceroute <host>`: Print the route to `host`
- `host <hostname>`: Look up the IP address of `hostname`
- `nslookup <hostname>`: Look up the DNS information for `hostname`
- `ifconfig`: Print the network interface information

### Users and groups

- `whoami`: Print the current user
- `who`: Print the users currently logged in
- `adduser <user>`: Add a new user `user`
- `addgroup <group>`: Add a new group `group`
- `usermod -a -G <group> <user>`: Add `user` to `group`
- `passwd <user>`: Change the password for `user`

## Advanced Commands

Here are some advanced bash commands:

### Shell scripts

- `#! /bin/bash`: Shebang indicating the path to the bash interpreter
- `$0`: The name of the script
- `$1`-`$9`: The first 9 arguments passed to the script
- `$@`: All arguments passed to the script
- `$#`: The number of arguments passed to the script
- `$?`: The exit status of the last command
- `$!`: The process ID of the last background command

### Loops

- `for i in <list>; do <commands>; done`: Iterate through `list`, executing `commands` each time
- `while <condition>; do <commands>; done`: Execute `commands` while `condition` is true
- `until <condition>; do <commands>; done`: Execute `commands` until `condition` is true
### Conditionals

- `if <condition>; then <commands>; fi`: Execute `commands` if `condition` is true
- `if <condition>; then <commands>; else <other commands>; fi`: Execute `commands` if `condition` is true, otherwise execute `other commands`

### Functions

- `function <name> { <commands>; }`: Define a function `name` with the commands `commands`
- `<name> <arguments>`: Call the function `name` with the arguments `arguments`

### Variables

- `<variable>=<value>`: Set the value of `variable` to `value`
- `$<variable>`: Access the value of `variable`
- `<variable>=$(<command>)`: Set the value of `variable` to the output of `command`

### Redirection

- `<command> > <file>`: Redirect the output of `command` to `file` (overwrite)
- `<command> >> <file>`: Redirect the output of `command` to `file` (append)
- `<command> < <file>`: Redirect the input of `command` from `file`
- `<command> 2> <file>`: Redirect the error output of `command` to `file`
- `<command> 2>> <file>`: Redirect the error output of `command` to `file` (append)

### Pipes

- `<command1> | <command2>`: Pipe the output of `command1` as the input of `command2`

### Background processes

- `<command> &`: Run `command` in the background

### Aliases

- `alias <name>='<command>'`: Set an alias `name` for the command `command`
- `alias`: List all aliases

### Environment variables

- `export <variable>=<value>`: Set an environment variable `variable` to `value`
- `echo $<variable>`: Print the value of the environment variable `variable`

### Shell options

- `set -o <option>`: Enable the shell option `option`
- `set +o <option>`: Disable the shell option `option`

### History

- `history`: List the command history
- `!<number>`: Execute the command with the history number `number`
- `!!`: Execute the last command
- `!<string>`: Execute the most recent command starting with `string`
