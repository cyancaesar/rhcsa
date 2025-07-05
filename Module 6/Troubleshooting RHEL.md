#### Changing a Lost Root User Password

- Enter Grub menu while booting
- Find the line that loads the Linux kernel and add `init=/bin/bash` to the end of the line
- `mount -o remount,rw /`
- `passwd root`
- `touch /.autorelabel`
- `exec /usr/lib/systemd/systemd`

#### Boot Debug Shell

- If something goers wrong early in the boot procedure, pop up debug shell
- `debug-shell.service`
- If the service is active, it starts root shell on TTY9 without entering the root password
- Useful for troubleshooting and analyzing, but needs to immediately be removed!
- Not active by default

Using it:
- `systemctl enable --now debug-shell.service`
- Whit booting access a virtual terminal on TTY9
	- `CTRL+Alt+F9`
- `systemctl disable --now debug-shell.service`

####  Filesystem Issues

- Real corruption does occur, but not often, and is automatically fixed
- Typos in /etc/fstab, and to fix remount the filesystem in read/write state and edit the file
- Fragmentation can be an issue, different tools exist to fix
	- `xfs_fsr` is XFS File System Reorganizer
	- `e4defrag` used to defragment Ext4
- After making a modification
	- `mount -a` to mount all filesystems without reboot
	- `findmnt --verify` to verify syntax
	- `reboot` very important. You made change early, reboot

#### Network Issues

- Wrong subnet mask
- Wrong router
- DNS not working: verify `/etc/resolv.conf` contents

#### Performance Issues

- Troubleshooting performance is an art on its own
- Four key areas of performance
	- memory
	- CPU load
	- disk load
	- network
- `top` gives you a generic image, but a specialized tools exists

#### Software Issues

- Dependency problems in RPM's
- Library problems
	- Run `ldconfig` to update the library cache
- Use containers to avoid these issues

#### Memory Shortage

- Memory issues appear if available memory as shown by `free` is low
- First help to fix memory issues, swap space can be used
	- It's good to use swap space, but if it is used a lot, that's not a good sign
- `vmstat 2 25` to make sure that adding swap space doesn't lead to too much I/O traffic

