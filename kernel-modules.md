# Install, Configure, and Monitor Kernel Modules
- kernel is core framework of the operating system
- provides a way for the rest of the system to oeprate with hardware, memory, networking and itself
- monolithic kernel - kernel handles all memory management and hardware device interations by itself 
- extra functionality can be loaded and unloaded through kernel modules without rebooting
- kernel modules are usually third-party drivers
- commands 
  - `uname` - disply information about the current running kernel
    - `-m` - get architecture 
    - `-rm` - get architecture and release version
    - `-a` - get all info about kernel
  - `lsmod` - displays a list of all currently loaded kernel modules
    - output columns: module name, size of memory used, modules using the module 
  - `modinfo [module]` to get information about a specific module
  - `modprobe [module]` used to dynamically load and unload kernel modules at runtime
    - `modprobe -r [module]` to remove a module
    - `modprobe [module]` to load a module and its dependencies

## More kernel module commands 
- legacy commands 
- `ls /lib/modules/$(uname -r)/` - list out the kernel modules directory for the current running kernel
- `rmmod` - removes a kernel module from a running kernel
  - `rmmod [module]` to remove
  - trying to remove a module with dependencies will fail unless `-f` force option supplied 
  - better to use `modprobe -r [module]` instead
- `insmod` - insert a module from a directory into the running kernel. Does NOT take into account module dependencies
  - `insmod [/path/to/module]` 
    - to remove a module you only need the module name, but to insert a module you need the full path to the location of the module on disk
    - modules for a kernel are located in a dir with the associated kernel name under `/lib/modules/$(uname -r)/kernel`
    - all kernel modules end in a `.ko` file extension, but on RHEL-based distros compress modules to save space with xz compression, so they will have a `.ko.xz` extension
- `depmod` - generates a list of kernel dependencies and map files
  - used when you install a new module so that its dependencies can be calculated for the kernel
- `/etc/modprobe.d/` - directory location where kernel modules can be listed in blacklist files, preventing the kernel from loading them
  - contains files that apply specific settings to specific modules 
  - can create a blacklist file to prevent a module from loading
  - create `/etc/modprobe.d/(module-blacklist.conf)` and use the keyword `blacklist` followed by the name(s) of the module(s) to blacklist
    - sometimes the case that dependencies must also be blacklisted


## Kernel panic
- a safety measure to prevent further data damage to the system
- state where kernel will not prevent any new messages
- causes: failing hardware, bugs in device drivers, bugs in operating system software
- debugging
  - kdump
  - can be analyzed with the GNU Debugger (gdb) utility, or the `crash` program
- `/proc/sys/kernel/panic` - contains the number of seconds a system will wait before rebooting to recover from a kernel panic. default of '0' indicates that the system will not reboot
  - the number is the number of seconds the system waits to reboot on kernel panic (but still, 0 means never)
  - set a different number to ensure a reboot, otherwise you have to manually reboot on kernel panic and this is not good
  - but this does not persist the change across reboots so it's kind of useless
  - must set permanent changes in `/etc/sysctl.conf`
- `/etc/sysctl.conf` - config file that contains various kernel parameters that can be altered
  - `kernel.panic = 15` - this line in the `/etc/sysctl.conf` file will reboot the system automatically after 15 seconds of a kernel panic's detection 
`sysctl` - this command will parse the `/etc/sysctl.conf` file, and can also reload parameters that are set within it
  - after making changes to the `sysctl.conf` file, run `sudo sysctl -p` to reparse the file and apply changes, otherwise changes won't happen until next boot 
