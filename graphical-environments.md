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