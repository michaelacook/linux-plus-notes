# Boot process 

1. POST
2. Boot sector on first drive
3. Load kernel
4. Load initial ramdisk, contains device drivers needed
5. kernel starts init system, start services/daemons, mount filesystems, initrmfs removed, services loaded so computer can be used

## boot logs
- volatile, restart at boot
- generated from kernel ring buffer
- `dmesg` - legacy command to output contents of kernel ring buffer
  - details on hardware kernel can see, how activated, low level memory management messages
  - determine if hardware is recognized by kernel
- `journalctl -k` - systemd utility to view the kernel ring buffer within the systemd journal
  - this is the newer and preferred way to see boot info and kernel ring buffer logs

## legacy GRUB
- Grant Unified Bootloader
- MBR (first 512 bytes of disk space)
- stages:
  - boot.img (stage 1) 
  - core.img (1.5)
    - locate boot partition
    - exists in empty space at beginning the boot disk
  - `/boot/grub` should contain `grub.conf` or `menu.lst` file (stage 2)
    - `grub.conf` on most RHEL-based distros 
    - `menu.lst` on most Debian-based distros
    - `device.map` 
      - indicates which drives contains the actual kernel
      - once kernel is located, it is loaded into memory
- installing GRUB		
  - `grub-install [device]` where device to install grub files to
  - find location where grub can be installed: 'findmnt /boot` will output device where the boot partition is found
  - can then use this location as the device to install grub on
  - `grub-install '(hd0)'` as an alternative
  - can also run `grub` command to list out bios drive info for your grub location
- GRUB shell
  - `grub` - invokes grub shell environment
  - `help` - print the help listing for grub, or get more info on a command 
  - `find` - search for a file in all partitions and list the devices the file is on
  - `quit` - exit

## GRUB2
- for UEFI and GPT
- stages:
  - POST
  - MBR load boot.img (Stage 1)
    - GPT header 
    - Partition Entry Array - list of partitions, IDs and locations
  - core.img stored in empty sectors (Stage 1.5) stored after PEA
  - core.img needs to locate `/boot/efi` which is the EFI System Partition
    - ESP must be vfat or FAT32
    - boot image from `/boot/efi` is read and loaded
  - `/boot/grub2` (Stage 2) 
    - `grubenv`
    - `themes`
- GRUB2 config commands
  - `grub2-editenv list` - view the default boot entry for the grub config file
  - `grub2-mkconfig` - creates (or updates) a `/boot/grub2/grub.cfg` file based on entries from the `/etc/default/grub` file
  - (On Debian-based systems the '2' is ommitted from the command name. e.g., `grub-mkconfig`)
  - `update-grub` - used to update a GRUB2 config after changes to `/etc/default/grub` have been made, found on Debian-based systems
  

## `initrmfs`
- commands
  - `lsinitrd` - allows you to view the contents of an initramfs file
  - `dracut` - will create a new initramfs for kernels on the system. it can be used to add and remove modules and drivers from initramfs builds
  `/etc/dracut.conf` - the primary config file for dracut, it typically points to the `/etc/dracut.conf.d` directory
