# 0x16. C - Simple Shell
## 1. Simple shell 0.1

**Mandatory**


# ALX Simple Shell Team Project
<p align="center">
<img src="https://s3.amazonaws.com/intranet-projects-files/holbertonschool-low_level_programming/235/shell.jpeg" width="700" height="400" />
</p>

> This is an ALX collaboration project on Shell.
> We were tasked to create a simple shell that mimics the Bash shell.
> 
> Our shell shall be called `hsh`.
## Introduction
This repository is a ALX Holberton School Project. The school project consisted in writing a shell like `sh` (Bourne Shell) by Stephen Bourne , in `C`, using a limited number of standard library functions, so instead we used our own function that we rewrote over the past three month [Here.](https://github.com/Samfrodo9/alx-low_level_programming)

The goal in this project is to make us understand how a shell works.
- To single out some core topics which includes;
  - What is the environment?
  - The difference between functions and system calls.
  - How to create processes using `execve...`

## General Requirements


- `README file`, at the root of the folder of the project is mandatory.
- Allowed editors: `vi`, `vim`, `emacs`
- Compiler;
  - Ubuntu 20.04 LTS using gcc.
  - Using the options `-Wall -Werror -Wextra -pedantic -std=gnu89`
- Coding style;
  - Betty Style.
- Shell should not have any memory leaks.
- No more than 5 functions per file.
- All header files should be include guarded.

## How hsh works
- Prints a prompt and waits for a command from the user.
- Creates a child process in which the command is checked.
- Checks for built-ins, aliases in the PATH, and local executable programs.
- The child process is replaced by the command, which accepts arguments.
- When the command is done, the program returns to the parent process and prints the prompt.
- The program is ready to receive a new command.
- To exit: press `Ctrl-D` or enter "`exit`" (with or without a status).
- Works also in non interactive mode.

## Usage
In order to run this program,

Clone This Repo;

`git clone https://github.com/Ismail-easytech/simple_shell.git`

Compile it with;

`gcc 4.8.4 -Wall -Werror -Wextra -pedantic *.c -o hsh.`

You can then run it by invoking `./hsh` in that same directory.

How to use it
In order to use this shell, in a terminal, first run the program:
`prompt$ ./hsh`
It wil then display a simple prompt and wait for commands.
`$`
A command will be of the type `$` command

To invoke **hsh**, compile all .c files in the repository and run the resulting executable.

hsh can be invoked both interactively and non-interactively. If hsh is invoked with standard input not connected to a terminal, it reads and executes received commands in order.

Example:
```
$ echo "echo 'hello'" | ./hsh
'hello'
$
```
If hsh is invoked with standard input connected to a terminal (determined by isatty(3)), an interactive shell is opened. When executing interactively, hsh displays the prompt $ when it is ready to read a command.

Example:
```
$./hsh
$
```

Alternatively, if command line arguments are supplied upon invocation, hsh treats the first argument as a file from which to read commands. The supplied file should contain one command per line. hsh runs each of the commands contained in the file in order before exiting.

Example:
```
$ cat test
echo 'hello'
$ ./hsh test
'hello'
$
```

## Environment
Upon invocation, `hsh` receives and copies the environment of the parent process in which it was executed. This environment is an array of name-value strings describing variables in the format `NAME=VALUE`. A few key environmental variables are:

### HOME
The home directory of the current user and the default directory argument for the cd builtin command.
```
$ echo "echo $HOME" | ./hsh
/home/projects
```

### PWD
The current working directory as set by the cd command.
```
$ echo "echo $PWD" | ./hsh
/home/projects/alx/simple_shell
```

### OLDPWD
The previous working directory as set by the cd command.
```
$ echo "echo $OLDPWD" | ./hsh
/home/projects/alx/printf
```

### PATH
A colon-separated list of directories in which the shell looks for commands. A null directory name in the path (represented by any of two adjacent colons, an initial colon, or a trailing colon) indicates the current directory.
```
$ echo "echo $PATH" | ./hsh
/home/projects/.cargo/bin:/home/projects/.local/bin:/home/projects/.rbenv/plugins/ruby-build/bin:/home/projects/.rbenv/shims:/home/projects/.rbenv/bin:/home/projects/.nvm/versions/node/v10.15.3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/projects/.cargo/bin:/home/projects/workflow:/home/projects/.local/bin
```

## hsh Builtin Commands
### cd
- Usage: `cd [DIRECTORY]`
- Changes the current directory of the process to `DIRECTORY`.
- If no argument is given, the command is interpreted as `cd $HOME`.
- If the argument `-` is given, the command is interpreted as `cd $OLDPWD` and the pathname of the new working directory is printed to standad output.
- If the argument, `--` is given, the command is interpreted as `cd $OLDPWD` but the pathname of the new working directory is not printed.
- The environment variables `PWD` and `OLDPWD` are updated after a change of directory.

Example:
```
$ ./hsh
$ pwd
/home/projects/alx/simple_shell
$ cd ../
$ pwd
/home/projects/alx
$ cd -
$ pwd
/home/projects/alx/simple_shell
```

### alias
- Usage: `alias [NAME[='VALUE'] ...]`
- Handles aliases.
- `alias`: Prints a list of all aliases, one per line, in the form `NAME='VALUE'`.
- `alias NAME [NAME2 ...]`: Prints the aliases NAME, NAME2, etc. one per line, in the form `NAME='VALUE'`.
- `alias NAME='VALUE' [...]`: Defines an alias for each NAME whose `VALUE` is given. If name is already an alias, its value is replaced with `VALUE`.

Example:
```
$ ./hsh
$ alias show=ls
$ show
AUTHORS            builtins_help_2.c  errors.c         linkedlist.c        shell.h       test
README.md          env_builtins.c     getline.c        locate.c            hsh
alias_builtins.c   environ.c          helper.c         main.c              split.c
builtin.c          err_msgs1.c        helpers_2.c      man_1_simple_shell  str_funcs1.c
builtins_help_1.c  err_msgs2.c        input_helpers.c  proc_file_comm.c    str_funcs2.c
```

### exit
- Usage: exit [STATUS]
- Exits the shell.
- The STATUS argument is the integer used to exit the shell.
- If no argument is given, the command is interpreted as exit 0.

Example:
```
$ ./hsh
$ exit
```

### env
- Usage: env
- Prints the current environment.

Example:
```
$ ./hsh
$ env
NVM_DIR=/home/projects/.nvm
...
```

### setenv
- Usage: setenv [VARIABLE] [VALUE]
- Initializes a new environment variable, or modifies an existing one.
- Upon failure, prints a message to stderr.

Example:
```
$ ./hsh
$ setenv NAME Poppy
$ echo $NAME
Poppy
```

### unsetenv
- Usage: `unsetenv [VARIABLE]`
- Removes an environmental variable.
- Upon failure, prints a message to `stderr`.

Example:
```
$ ./hsh
$ setenv NAME Poppy
$ unsetenv NAME
$ echo $NAME
$

```
Write a UNIX command line interpreter.
Usage: ``` simple_shell ```
Your Shell should:
Display a prompt and wait for the user to type a command. A command line always ends with a new line.
The prompt is displayed again each time a command has been executed.
The command lines are simple, no semicolons, no pipes, no redirections or any other advanced features.
The command lines are made only of one word. No arguments will be passed to programs.
If an executable cannot be found, print an error message and display the prompt again.
Handle errors.
You have to handle the “end of file” condition (Ctrl+D)
You don’t have to:

use the PATH
implement built-ins
handle special characters : ", ', `, \, *, &, #
be able to move the cursor
handle commands with arguments
execve will be the core part of your Shell, don’t forget to pass the environ to it…

2. Simple shell 0.2
mandatory
Simple shell 0.1 +
Handle command lines with arguments

3. Simple shell 0.3
mandatory
Simple shell 0.2 +
Handle the PATH
fork must not be called if the command doesn’t exist

4. Simple shell 0.4
mandatory
Simple shell 0.3 +
Implement the exit built-in, that exits the shell
Usage: exit
You don’t have to handle any argument to the built-in exit

5. Simple shell 1.0
mandatory
Simple shell 0.4 +
Implement the env built-in, that prints the current environment

6. Simple shell 0.1.1
#advanced
Simple shell 0.1 +
Write your own getline function
Use a buffer to read many chars at once and call the least possible the read system call
You will need to use static variables
You are not allowed to use getline
You don’t have to:
be able to move the cursor

7. Simple shell 0.2.1
#advanced
Simple shell 0.2 +
You are not allowed to use strtok

8. Simple shell 0.4.1
#advanced
Simple shell 0.4 +
handle arguments for the built-in exit
Usage: exit status, where status is an integer used to exit the shell

9. setenv, unsetenv
#advanced
Simple shell 1.0 +
Implement the setenv and unsetenv builtin commands
setenv
Initialize a new environment variable, or modify an existing one
Command syntax: setenv VARIABLE VALUE
Should print something on stderr on failure
unsetenv
Remove an environment variable
Command syntax: unsetenv VARIABLE
Should print something on stderr on failure

10. cd
#advanced
Simple shell 1.0 +
Implement the builtin command cd:
Changes the current directory of the process.
Command syntax: cd [DIRECTORY]
If no argument is given to cd the command must be interpreted like cd $HOME
You have to handle the command cd -
You have to update the environment variable PWD when you change directory
man chdir, man getcwd

11. ;
#advanced
Simple shell 1.0 +
Handle the commands separator ;

12. && and ||
#advanced
Simple shell 1.0 +
Handle the && and || shell logical operators

13. alias
#advanced
Simple shell 1.0 +
Implement the alias builtin command
Usage: alias [name[='value'] ...]
alias: Prints a list of all aliases, one per line, in the form name='value'
alias name [name2 ...]: Prints the aliases name, name2, etc 1 per line, in the form name='value'
alias name='value' [...]: Defines an alias for each name whose value is given. If name is already an alias, replaces its value with value

14. Variables
#advanced
Simple shell 1.0 +
Handle variables replacement
Handle the $? variable
Handle the $$ variable

15. Comments
#advanced
Simple shell 1.0 +
Handle comments (#)

16. File as input
#advanced
Simple shell 1.0 +
Usage: simple_shell [filename]
Your shell can take a file as a command line argument
The file contains all the commands that your shell should run before exiting
The file should contain one command per line
In this mode, the shell should not print a prompt and should not read from stdin

**Thank you for going through our shell documentation.**

## AUTHORS

### 🤵🏼 Samuel Adeyemi

###### [Twitter](https://twitter.com/adeyemifrodo?t=6JGpNMULaRPkxUWk-9pGug&s=09) ![image](https://user-images.githubusercontent.com/106933402/185825114-48aecc27-08d6-42fe-a77a-811439356247.png)

###### [Instagram](https://www.instagram.com/adeyemifrodo/) ![image](https://user-images.githubusercontent.com/106933402/185825340-cc014407-6242-45e4-8bd7-85f7cba492d4.png)

###### [Facebook](https://www.facebook.com/SAMFRODO100) ![image](https://user-images.githubusercontent.com/106933402/185828761-a9a56af0-2e4d-404a-849a-13e1d9db2dd1.png)

###### [LinkedIn](https://www.linkedin.com/in/samuel-adeyemi-a50055160) ![image](https://user-images.githubusercontent.com/106933402/185828055-400303e2-1888-4624-af76-943d18a37288.png)


