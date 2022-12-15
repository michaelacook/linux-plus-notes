# Privilege Escalation
- elevate privileges by being member of the `wheel` group or whatever group has permission to elevate to root user privileges 
- `su` - substitute user command
  - `su example` to change to example user
  - can also use with a hyphen 
    - using without a hyphen gives you the user's identity but does not change login session
    - using a hyphen gives a new login shell with that user's session
    - in short: no hyphen - no new session; with hyphen - get a new session
    - this has security implications - don't want normal users to be able to login as another user
    - can give a user ability to edit files without giving them sudo abilities by giving them access to `sudoedit`
      -  `sudoedit [file]`

## `sudo` configuration
- `/etc/sudoers` - grants permissions to users and groups to elevate via `sudo`
  - never edit via normal text edit, only with `visudo` command 
    - to edit sudoers file run `sudo visudo`
    - does syntax check, make sure everything is done correctly
    - otherwise could lock out ability to elevate
- give user ability to use `sudoedit` rather than `sudo` to edit with root privilege but do nothing else with root privilege 
  - (note: `%wheel` refers to the group wheel rather than a user)
  - (note: sudoedit doesn't work on symlinks)
  - create a group that has access to sudoedit and grant it the paths it can edit, then assign users to that group who you want access to sudoedit
  - at the bottom of sudoers file: 
    - `[%group] ALL = sudoedit [path|ALL]` 
- `visudo` arguments
  - `-c` check for errors
  - `-f` file name (other than default)
  - `-s` strict mode
  - `-x` output the sudoers file in json
- add user to the sudoers file
  - at bottom of file: `username ALL=(ALL:ALL) ALL`
  - must use `visudo`


## user account types
- disable root user by default for security purposes
- Windows also does this
- standard users have a home dir, login shell, run in their own context
  - login shell means you can get a GUI shell, terminal shell, login over SSH
  - standard users also have a profile - profile files to control their user account
    - i.e., `.bash_profile`
- service users are different - have no login shell, so can't login as that user, no SSH, no GUI, etc
  - `grep nologin /etc/passwd`
  
