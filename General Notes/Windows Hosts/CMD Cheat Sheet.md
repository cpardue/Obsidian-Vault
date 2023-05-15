---
layout:     post
title:      CMD Cheat Sheet
date:       2023-01-01
summary:    Windows CMD Cheat Sheet 
categories: windows
thumbnail: cogs
tags:
 - windows
 - cli
 - cmd
---

# Windows CMD Cheat Sheet

CMD is the default command-line interface on Windows systems.

## Basic Commands

Here are some basic CMD commands:

### Navigation

- `cd <directory>`: Change the current working directory to `directory`
- `cd ..`: Move up one directory level
- `cd\`: Move to the root directory
- `dir`: List the files and directories in the current directory
- `dir <directory>`: List the files and directories in `directory`
- `md <directory>`: Create a new directory `directory`
- `rd <directory>`: Remove `directory` (if it is empty)

### File and directory management

- `copy <source> <destination>`: Copy `source` to `destination`
- `xcopy <source> <destination>`: Copy `source` and subdirectories to `destination`
- `move <source> <destination>`: Move `source` to `destination`
- `del <file>`: Delete `file`

### Text manipulation

- `type <file>`: Print the contents of `file` to the terminal
- `find "<string>" <file>`: Search for `string` in `file`
- `findstr "<string>" <file>`: Search for `string` in `file` (case-insensitive)
- `sort <file>`: Sort the lines of `file`

### Process management

- `taskkill /pid <pid>`: Terminate the process with the process ID `pid`
- `taskkill /im <process>`: Terminate all processes with the name `process`

### System information

- `ver`: Print the current system version
- `systeminfo`: Print system information
- `ipconfig`: Print the network configuration

### Networking

- `ping <host>`: Send a ping request to `host`
- `tracert <host>`: Print the route to `host`
- `nslookup <hostname>`: Look up the DNS information for `hostname`

### Users and groups

- `whoami`: Print the current user
- `net users`: List the users on the system
- `net user <user> <password>`: Create a new user `user` with the password `password`
- `net group <group>`: Display the members of `group`
- `net localgroup <group>`: Display the local groups on the system

## Advanced Commands

Here are some advanced CMD commands:

### Batch scripts

- `@echo off`: Turn off command echoing (commands will not be printed to the terminal)
- `%0`: The name of the script
- `%1`-`%9`: The first 9 arguments passed to the script
- `%*`: All arguments passed to the script
- `%~dp0`: The current directory of the script
- `goto <label>`: Jump to the specified `label` in the script
- `:<label>`: Define a `label` in the script
- `if <condition> <command>`: Execute `command` if `condition` is true
- `if <condition> (command) else (other command)`: Execute `command` if `condition` is true, otherwise execute `other command`
- `for <variable> in (<list>) do <command>`: Iterate through `list`, setting `variable` to the current item and executing `command` each time
- `for /f "tokens=<n>" %<variable> in (<file>) do <command>`: Read `file` line by line, setting `variable` to the `n`th token on each line and executing `command` each time

### Environment variables

- `set <variable>=<value>`: Set an environment variable `variable` to `value`
- `echo %<variable>%`: Print the value of the environment variable `variable`

### Redirection

- `<command> > <file>`: Redirect the output of `command` to `file` (overwrite)
- `<command> >> <file>`: Redirect the output of `command` to `file` (append)
- `<command> 2> <file>`: Redirect the error output of `command` to `file` (overwrite)
- `<command> 2>> <file>`: Redirect the error output of `command` to `file` (append)

### Pipes

- `<command1> | <command2>`: Pipe the output of `command1` as the input of `command2`

### Aliases

- `doskey <name>=<command>`: Set an alias `name` for the command `command`
- `doskey /macros`: List all aliases

### History

- `doskey /history`: List the command history
- `doskey <name> /history`: Display the command history for the alias `name`
- `doskey /reinstall`: Clear the command history

