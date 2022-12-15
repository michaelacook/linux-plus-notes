# Graphical environments
- GTK+ based desktops
  - written in C
  - Gnome, Xfce
- Qt based desktops
  - C++
  - KDE
- virtual desktops
  - in Gnome, Activities
  - basically similar to workspaces

## Assistive technologies on the Linux desktop
- tools and utilities for accessibility
- most commonly for Gnome, but others as well
- located under Universal Access in Gnome
  - high contrast
  - large text - increase font size for easy reading
  - cursor size to make it easier to find the cursor
  - zoom - magnifier
  - screen reader - uses orca for backend

```bash
orca --text-setup

# follow the prompts for voice package

# choose language

# choose echo option (whether screen reader speaks every word typed)

# choose key echo

# choose desired keyboard layout (desktop or laptop)

# enable braille if equipment is available

# press Ctl+c to quit

# enable screen reader from Universal Access 
```

- sound keys for number lock and caps lock are enabled/disabled
- visual alerts to go along with audio alerts
- on screen keyboard
- repeat keys - determine how fast keys repeat when held down
- cursor blinking speed inside text box
- typist assist - keyboard shortcuts to enable and disable accessibility settings
- sticky keys - treats a sequence of modifier keys as a key combination
- slow keys - puts a delay between when a key is pressed and when it is accepted
- bounce keys - ignores fast duplicate keypresses
- pointing and clicking - simulate mouse with keyboard
- click assist - use primary mouse button as a secondary mouse button
  - simulated secondary click - trigger secondary click by holding down primary button
  - hover click: mouse hover long enough gets treated as a click
- double-click delay - how much time between mouse clicks gets treated as a double click
- nightlight

## X11
- how graphics are displayed
- X.Org - provides graphical rendering for Unix-like operating systems
- Display server - X Server
  - core display server system that provides the protocol service for the X Window System
  - extra functionality added with extensions
    - RandR: provides dynamic resizing of the root window, refresh rates, mirroring displays, etc
    - GLX: provides rendering of 3D OpenGL content within windows
    - Xinerama: provides the ability to split the desktop display across multiple windows
- each graphical rendering on the screen is a client of the X.org server, which draws it on the screen
- architecture stack
  - application
    - terminal emulator, web browser, etc
  - Display manager (GNOME, KDE, Xfce, etc)
    - send intructions from display manager to Xlib/xcb
  - Xlib/XCB - libraries sending drawing requests to X server
    - XCB - KDE
  - X Server - configs in `/etc/X11/xorg.conf` and `/etc/X11/xorg.conf.d` dir
  - libDRM - Direct Rendering Manager
    - sits between kernel and X server
  - kernel and graphics driver
  - graphics card
- flows both ways

### Wayland
- replacement for X, but not there yet
- simpler, fewer steps
- XWayland - library that enables X Window clients to render with Wayland (for backwards compatibility)

## Remote graphical connections
- access remote system's display
- basic functionality of the X server is that it is able to be networked
- `xhost` - older and insecure method of allowing client systems the ability to display remote X11 windows
  - `xhost + | xhost -` to enable or disable
  - `xhost + [IP]` to allow a single host to access via the network
  - insecure, usually disabled, shouldn't use
- `xauth` - allows a user to edit and view security information that grants a user the ability to control remote X11 client windows
- `VNC` - Virtual Network Computing enables a remote computer to control the graphical display of a remote system. Insecure by default
- `SPICE` - TLS encrypted remote desktop protocol that can be used on Linux, Windows and Android systems
  - Simple Protocol for Independent Computing Environments