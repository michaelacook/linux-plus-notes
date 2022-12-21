# Package managers

## Advanced Package Tool (APT)
- installs packages and their dependencies
- uninstalls, updates and upgrades
- reads the `/etc/apt/sources.list` file for list of repositories
  - after resolving dependencies and downloading the dependency packages it sends install and uninstall tasks to the Debian-native `dpkg` tool
  - basically `apt` doesn't install programs, it just handles dependencies
- update with `apt-get update`
  - uses a local cache of available packages - apt cache
- upgrade any upgradable packages with `apt-get upgrade`
- install with `apt-get install [package]`
- uninstall with `apt-get remove`
  - this only removes only the application itself, not its dependencies or configuration files
  - to then remove all dependencies run `apt-get autoremove`
- to uninstall a program and remove all configuration for it run `apt-get purge [package]`
- upgrade distribution to the latest level by installing new packages with `apt-get dist-upgrade`
  - brings everything up to date including the installation of new packages
- download a package without installing it with `apt-get download [htop]`
  - don't elevate permissions if you want the package to have the permissions of the user who downloaded it
- search for a package with `apt-cache search [package]`
- get more information about a package with `apt-cache show [package]`
  - get more detailed information with `apt-cache showpkg [package]`

### `/etc/apt/sources.list`
- lines start with `deb` or `deb-src` for debian package or source code
- then the URL for the repo
- then the distribution name, such as `xenial` or `kinetic`
- then the repository type, such as `multiverse` or `main`

## `dpkg`
- install `.deb` packages
- contains
  - application
  - default configs
  - how and where to install the files that come with the package
  - listing of dependencies the package requires
- must already have dependencies installed
- commands 
  - `dpkg --info [package]` - get info on a package
  - `dpkg --status [package]` - similar to info but less info
    - usually used with packages that are already installed
    - can use without args to list a list of all installed packages
  - `dpkg -l [string]` list packages that match the string provided
  - `dkpg -i [package]` install specified package(s)
  - `dpkg -L [package]` list out files installed with a package 
  - `dpkg -r [package]` to remove a package
    - keeps config files
  - `dpkg -P [package]` to remove a package and its config files
  - `dpkg -S [string]` to see if there is anything in the package database matching the provided string
  - `dpkg-reconfigure [package]` allows for the modification of a package by re-running the applications default configuration tool

## RPM-based distributions
- for a lot of RPM-based package management I already have notes [here](https://github.com/michaelacook/RHEL-package-management)

### `yum`
- avoid dependency hell
- global yum config file in `/etc/yum.conf`
- reads from `/etc/yum.repos.d`
  - each repo gets its own config file in this dir
- caches latest repository information in `/var/cache/yum`
- `Zypper` is used on SUSE and OpenSUSE
  - `zypper repos` - list individual repos
  - `zypper install [package]`
- `DNF` - Dandified yum
  - used on Fedora
    - future replacement for yum in RHEL
    - (I think at this point it basically has replaced yum)
  - command syntax same as yum

### DNF & Zypper
- Dnf has basically exact same commands as yum
- uses `/etc/yum.repos.d` same as yum does
- `dnf install [package]`
- `dnf remove [package]`
- `dnf update`
- `dnf whatprovides [command]`
  - will help you search for a particular command or application by asking Dnf what RPM package contains it
- `zypper in [package]`
- `zypper rm [package]`
- `zypper refresh` - refresh all repositories to see what new packages are available
- `zypper se [package]` - search the repositories for a particular package

## Compiling from source
- compiling an application allows you to include modules or directives that are not included in the application by default
- makefile - contains the directives for the make utility
- make - utility that is commonly used for compiling applications
- process
  - usually first run a `configure` script first, and this will create a makefile
  - then run the makefile with `make` command
  - this will output a compiled application binary
  - then run `make install` to install the binary using the top level makefile
  - make a systemd unit file to run the application as a daemon if that is what you want
    - `sudo vim /etc/systemd/system/[binary name].service
      - add the contents of the unit file
      - then start the service with systemd
      - note: it's necessary to create your own systemd unit file to run the process as a service when you compile and manually install, unlike when you install through more standard means
      - also note: when you install from source it isn't being handled by a package manager, so when an update comes out you need to go through the process again to update the application. in fact you don't really update the application, you install the new version

## Managing shared libraries
- shared libs are functionaity that other applications can use
- ends with a `.so` extension - stands for shared object
- found under 
  - `/lib`
  - `/usr/lib` for 32 bit systems
  - `/usr/lib64` for 64 bit systems
  - `/usr/local/lib`
  - `/usr/share`
- two types 
  - dynamic: `.so`
  - statically linked: `.a`
    - for applications that need the exact same version of a function each time the application makes a call
    - sometimes applications come with their own individual statically-linked libraries that work with just that application
      - games, graphics
    - most use shared libraries though
- commands 
  - `ldd [/path/to/binary]` - determine which libraries are being used by an application
  - `ldconfig` - configures dynamic linker run-time bindings, creates a cache based on library directories and can show you what is currently cached
    - create cached listing of the most recently used libraries on your system, given a directory given
  - `/etc/ld.so.conf` - config file that points to directories and other configuration files that hold references to library directory locations
  - `LD_LIBRARY_PATH` - legacy env var that points to a path where library files can be read from
    - empty by default