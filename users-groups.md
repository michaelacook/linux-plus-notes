# Users and groups
Standard user and group commands can be found [here](https://github.com/michaelacook/linux-essentials-cheat-sheet/blob/master/cheatsheets/users-groups.md)

## Securing local logins
- locking passwords and accounts is a good way to secure accounts that will not be used for a certain period of time, such as an employee vacation
  - `usermod -L [username]` to lock password 
  - `usermod -e 1 [username]` to lock account
    - to unlock the account and password run `sudo usermod -U -e "" -s /bin/bash [username]`
- can also temporarily remove the login shell for added security
  - `sudo usermod -s /sbin/nologin [username]`
- `/etc/nologin` - file can be used to display a message on the console when someone attempts to login with an account that is using the `/sbin/nologin` shell
- interestingly, `/etc/shadow` password database has no permissions on it, but we can still use it because root can run the `/bin/passwd` command
  - permissions on user these files should never be changed

## Bash shell environment
- default is the Bourne Again Shell
- also 
  - csh, C programming style syntax
  - ksh, KornShell, based on Bourne shell, with some features of the C shell added
  - zsh, Z Shell includes elements of the Bash and Korn Shells
- Environment variables - settings that dictate common functionaity and locations for various purposes
  - view with `env` command
  - `set` - view all env variables in the collation sequence of the current locale when given no args
    - can turn on debugging for scripts with `set -x`
    - turn off debugging with `set +x`
    - remove env variables and user-created functions
      - `unset -f [function name]` to remove functions
      - `unset [var name]` to remove env variable
        - don't use the `$` sign
  - `shopt` command to see all options for bash environment
    - `shopt -s [option]` to enable an option
  - `export` - used to export a variable to the current shell and any new shells started from the current shell
    - `export [VARNAME=value]`
- `pwd` to get current working dir, gets its value from `$PWD` env var
- `which [application]` to find the location to an application binary
- `type [application]` to determine if something is a function, file, alias, built-in, or keyword

### Quoting
- can be tricky
- weak quotes - double quotes
  - expand variables, but not characters used for path substitution or pattern matching
- strong quotes - single quotes
  - nothing is interpreted or expanded

## Setting up the shell environment
- two types of shell environments
  - interactive login shell
    - created when logging into a console (real virtual terminal on the machine)
    - created when logging in remotely over the network
    - how it's created under the hood
      - `/etc/profile` script is run, uses configs from `/etc/profile.d/*`
      - `/etc/profile` then runs `~/.bash_profile` (used by RHEL distros)
        - this might also be `.profile`
      - `~/.bash_profile` then runs `~/.bashrc` which pulls configs from `/etc/bashrc`
  - interactive non-login shell
    - created when a terminal application such as GNOME terminal is started
    - terminal emulator programs
    - how it's created under the hood
      - first runs `~/.bashrc` which gets configs from `/etc/bashrc`
- determine whether you are running in a login or non-login shell with the command `echo $0`
  - `-bash` for login shell
  - `bash` for non-login shell

### Bash configuration files 
- `/etc/profile`
  - first file read on a login session. sets up system-wide environment variables, umask values, bash history controls, etc
  - does many things, including setting system-wide path, history size, umask, etc
  - shouldn't change this file. instead add custom scripts in `/etc/profile.d`
- `/etc/profile.d`
  - directory containing extra script config files for bash
  - the `/etc/profile` file will read in the contents of this directory
- `/etc/bashrc`
  - configure system-wide functions and aliases here
- `/etc/skel`
  - directory containing the default `.bash_profile`, `.bashrc` and other files that are added to a user's home directory when an account is created on the system
- `~/.bash_profile`
  - contains a user's modified PATH env var and will source the `~/.bashrc` file
  - can also be named `~/.profile` in some distributions
  - runs when user logs into system from the console
- `~/.bashrc`
  - local user command aliases and functions defined here
  - this file sources the `/etc/bashrc` file
  - runs when user opens a terminal emulation session in a graphical environment
- `~/.bash_logout`
  - gets called on a user logout and can be used to shut down applications, display a message, or perform other environment cleanup tasks
- `~/.bash_login`
  - gets called on a user login
  - usually `~/.bash_profile` and `~/.bashrc` are used instead of this file

## Customizing the shell environment
- turn off file globbing `set -f`
- turn on file globbing `set +f`
- alias a command `alias [command]=[new command]`
  - persist by putting in `~/.bashrc`
- create functions with the `function` keyword then write a JavaScript-style function
  - no return value
  - persist by putting it in the `~/.bashrc`
- `PATH`
  - all locations for binaries
  - append new directories to the `PATH` with `export PATH=$PATH:/path/to/binary`
    - persist this by adding to `~/.bash_profile` or `~/.profile`

## Bash history and man pages
- `history` - display recent bash history
  - lists history of commands in numerical order
  - can run a command by number with `![number]`
    - useful if the command was too long to type out again but you do know its history number
- `~/.bash_history` - file located in the user's home dir that contains the previously run commands
  - number of records this file keeps before deleting them is dictated by the `HISTFILESIZE` env var
  - default is 500 lines
- manual pages - built-in documentation for commands, configs and system admin tasks
  - broken into sections
    - Section 1: executable programs or shell commands 
    - Section 2: system calls - functions provided by the kernel
    - Section 3: library calls - functions within program libraries
    - Section 4: special files - typically those found in `/dev`
    - Section 5: file formats and conventions - e.g., `/etc/passwd` and other configs
    - Section 6: Games
    - Section 7: Misc items and conventions - e.g., `man(7)`, `regex(7)`
    - Section 8: system admin commands - usually only for root
    - Section 9: kernel routines (non standard)
- `apropos [keyword]` - links to the `man -k` command 
- `man -k [keyword]` - used to search the man pages for a specific keyword
- `info [command/file]` for less terse, more readable information