<table style="border-style: none;">
    <tr >
        <td>
            <img src="../resources/command-prompt-logo.png" style="height: 60px">
        </td>
        <td>
            <span style="font-weight: 500; font-size: 2em;">CMD - Command prompt for Windows</span>
        </td>
    </tr>
</table>


- [Usual commands](#usual-commands)
- [Linux / Windows shell correspondence](#linux--windows-shell-correspondence)
- [Solutions to common use cases](#solutions-to-common-use-cases)
  - [Get a command's manual](#get-a-commands-manual)
  - [Remove a non-empty directory](#remove-a-non-empty-directory)
  - [Chaining commands](#chaining-commands)
  - [Piping](#piping)
  - [Computing a cryptographic hash](#computing-a-cryptographic-hash)
- [Stream redirection](#stream-redirection)
  - [Standard streams](#standard-streams)
  - [Redirection](#redirection)
  - [Redirection to device `NUL`](#redirection-to-device-nul)

# Usual commands

| Command| Description |
|:------:|:------------|
| `cd | chdir` | Change the directory or print the current directory if no argument is specified (same as Linux `pwd`). |
| `cls` | Clear the screen. |
| `copy` | Copies one or more files from one location to another. |
| `del` | Deletes one or more files. |
| `dir` | Displays a list of a directory's files and subdirectories. |
| `echo` | Displays messages or turns on or off the command echoing feature (`echo [on | off]`). |
| `erase` | Same as `del`.  |
| `exit` | Exit the command interpreter |
| `fc` | Compare the content of files. Similar to Linux `diff` command. |
| `find` | Searches for a string of text in a file or files, and displays lines of text that contain the specified string. Similar to Linux `grep` command. |
| `help` | Provides online information about system commands (that is, non-network commands). If used without parameters, help lists and briefly describes every system command. |
| `md | mkdir` | Creates a directory or subdirectory recursively.  |
| `more` | Displays one screen of output at a time. Similar to Linux `less` command. |
| `move` | Moves one or more files from one directory to another directory. |
| `rd | rmdir` | Deletes a directory. |
| `ren | rename` | Renames files or directories. |
| `type` | Use the type command to view a text file without modifying it. |
| `where` | Displays the location of files that match the given search pattern. |
| `xcopy` | Copy files and directories, including subdirectories. |

Official [Miscrosoft reference](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/commands-by-server-role).

# Linux / Windows shell correspondence

| Linux shell | Windows cmd |
|:-----------:|:-----------:|
| `clear` | `cls` |
| `cp` | `copy` |
| `diff` | `fc` |
| `grep` | `find` |
| `man` | *command* `/?` |
| `mv` | `move` (change location), `ren` (rename)|
| `pwd` | `cd`, `chdir`|
| `rm` | `del` (files) |
| `rm -r` | `rd` *path* `/s` |

# Solutions to common use cases

## Get a command's manual
Get help for a command `<command>` with `/?`.

*E.g.*
```cmd
find /?
```

## Remove a non-empty directory
```cmd
rmdir path\to\the\directory /s
```

## Chaining commands

|||
|--------------------------|------------------------------------------------|
| commandA `&`  commandB   | Run commandA and then run commandB             |
| commandA `&&` commandB   | Run commandA, if it succeeds then run commandB |
| commandA `||` commandB   | Run commandA, if it fails then run commandB    |

Example:
```cmd
commandA && commandB || commandC 
```
Means: If *commandA* succeeds run *commandB*, if it fails *commandC*

## Piping
Pipe the output from *commandA* into *commandB*:
```cmd
commandA  |  commandB     
```

## Computing a cryptographic hash

Windows 10 uses the native command `certutil` (use `fciv` for prior versions). 

*E.g.* computing a MD5 sum
```
> certutil -hashfile "path\to\the\file.ext" MD5
MD5 hash of path\to\the\file.ext:
9f64fa1162e198583ed33f160299f861
CertUtil: -hashfile command completed successfully.
```

Other algorithms: SHA1, SHA512

# Stream redirection

## Standard streams

The standard streams, are referenced using the numbers 0, 1, and 2. **Stdin** is file 0, **stdout** is file 1, and **stderr** is file 2.

## Redirection

A very common task in batch files is sending the output of a program to a log file. The `>` operator sends, or redirects, *stdout* or *stderr* to another file. *E.g.* write a listing of the current directory to a text file:
```cmd
dir > temp.txt
```
The `>` operator overwrites the contents of *temp.txt* with *stdout* from the `dir` command. The `>>` operator is a slight variant that appends the output to a target file, rather than overwriting the target file.

By default, the `>` and `>>` operators redirect stdout. You can redirect *stderr* by using the file number 2 in front of the operator:
```cmd
dir SomeFile.txt  2>> error.txt
```
You can even combine the *stdout* and *stderr* streams using the file number and the `&` prefix:
```cmd
dir SomeFile.txt 2>&1
```
This is useful for writing both stdout and stderr to a single log file.
```cmd
dir SomeFile.txt > output.txt 2>&1
```
To use the contents of a file as the input to a program, instead of typing the input from the keyboard, use the `<` operator. *E.g.*:
```cmd
command < SomeFile.txt
```

## Redirection to device `NUL`

Similarly to Linux `/dev/null`, the pseudo-command `NUL` can be used as an output black hole:
```cmd
command > NUL
```
