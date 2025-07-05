
Boot procedure steps:
1. Firmware
2. Boot device
3. grub (bootloader)
4. kernel with a helper called initramfs (initrd)
5. Systemd
6. Early state
7. Services
8. Shell

Why all of that? for troubleshooting

#### Modifying Grub2 Runtime Parameters

From Grub2 boot menu, press e to edit runtime boot options to the end of the line that starts with linux
- systemd.unit=emergency.target
- systemd.unit=rescue.target

Press c for Grub2 command mode

#### Grub2 Persistent Parameters

- Edit persistent Grub2 parameters from `/etc/default/grub`
- Compile the changes and output it to `/boot/grub2/grub.cfg`
- For NBR systems
	- `grub2-mkconfig -o /boot/grub2/grub.cfg`
- For EFI systems
	- `grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg`

To determine if you are in an EFI system
- `lsblk`
- Check the boot disk if it `/boot/efi` then it is EFI otherwise its an NBR

#### Systemd Targets

- A group of unit files
- Some targets are isolatable, means they define the final state a system is starting in
	- emergency.target
	- rescue.target
	- multi-user.target (default with no GUI)
	- graphical.target
- When enabling a unit, it is added to a specific target

Setting the default systemd target:

- `systemctl get-default` current default target
- `systemctl set-default` set a new default target

#### Booting into a Specific Target

- On Grub2 boot prompt, use `systemd.unit=xxx.target` to boot into a specific target
- To change between targets on a running system, use `systemctl isolate xxx.target` for going from top to bottom
- Or use `systemctl start xxx.target` to go from bottom to top

#### Lab Note

On emergency target, you are very limited, you cannot write. Do a remount of the root directory with a write access, and do `mount -a` to mount boot directory also

