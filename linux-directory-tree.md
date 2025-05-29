# Major Parts of Linux Directory Tree

1. Root `/`
	- Starting point of the filesystem.
	- Contains essential files for system boot and recovery.
	- Not to be confused with `/root` (root user’s home).

2. `/bin` — User Binaries
	- Essential executable programs for all users.
	- Example: `ls`, `cp`, `grep`.

3. `/sbin` — System Binaries
	- System maintenance executables.
	- Typically used by administrators.
	- Example: `fdisk`, `reboot`.

4. `/etc` — Configuration Files
	- System-wide configuration files.
	- Startup/shutdown scripts.
	- Example: `/etc/resolv.conf`.

5. `/dev` — Device Files
	- Interface to system devices.
	- Example: `/dev/tty1`.

6. `/proc` — Process Information
	- Virtual filesystem showing runtime system information.
	- Example: `/proc/cpuinfo`.

7. `/var` — Variable Files
	- Data expected to change over time.
	- Example: `/var/log`, `/var/mail`.

8. `/tmp` — Temporary Files
	- For temporary data; cleared on reboot.

9. `/usr` — User Programs
	- Non-essential user programs, libraries, documentation.
	- Subdirectories:
	- `/usr/bin`: User binaries.
	- `/usr/sbin`: Admin binaries.
	- `/usr/lib`: Libraries.
	- `/usr/local`: Locally compiled apps.

10. `/home` — User Home Directories
	- Personal user data.
	- Example: `/home/alice`.

11. `/boot` — Boot Loader Files
	- Contains kernel and bootloader.
	- Example: `vmlinuz`, `initrd`.

12. `/lib` — System Libraries
	- Essential libraries for binaries in `/bin` and `/sbin`.
	- Example: `libc.so`.

13. `/opt` — Optional Add-on Applications
	- For third-party or vendor software.

14. `/mnt` — Mount Directory
	- Temporary mounting of filesystems by sysadmins.

15. `/media` — Removable Media
	- Mount points for devices like CD-ROMs or USBs.
	- Example: `/media/usb`.

16. `/srv` — Service Data
	- Data for services hosted on the system.
	- Example: `/srv/ftp`.