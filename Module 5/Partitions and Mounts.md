Two partitioning scheme:
- MBR (obsolete)
- GPT

List device blocks
```sh
lsblk
```

`lsblk` takes information from `/proc/partitions`.

```sh
fdisk -l <disk_device>
```

#### Linux Storage Options

- Partitions
	- Allocate a dedicated storage to specific types of data
- LVM Logical Volumes
	- Default installation for RHEL
	- Flexibility to storage in terms of resize, snapshots
- Stratis
	- Next generation volume managing filesystem that uses thin provisioning by default

#### Linux Block Devices

- `/dev/sdx` for SCSI and SATA disks
- `/dev/vdx` for KVM virtual machines
- `/dev/nvmexny` for NVME devices

#### Partitioning Tools

- fdisk
	- `g` to create GPT partition table
- gdisk
- parted

Partition type is a low-level identifier of the OS, filesystem, and boot manager. Set partition type in `fdisk` with `t`.

Aliases
- linux: standard linux
- swap: linux swap
- uefi: uefi boot
- lvm: logical volume manager


#### Create and Mount Filesystems

After partitioning a disk is creating a mounting a filesystem to the partitions.

- XFS is the default filesystem
	- Fast and scalable
	- Uses CoW to guarantee data integrity
	- Size can be increased, not decreased
- Ext4 was default in RHEL 6
	- Backward compatible to Ext2
	- Uses Journal to guarantee data integrity
	- Size can be increased and decreased
- vfat offers multi-OS support
	- Used for shared devices
- Btrfs is a new and advanced filesystem
	- Not installed by default  on RHEL

#### Creating Filesystems

- `mkfs.xfs` creates a XFS filesystem
- `mkfs.ext4` creates an Ext4 filesystem
- `mkfs.vfat` create a vfat filesystem

#### Mount Filesystems

- `mount` command is used to mount in runtime
	- `mount /dev/vdb1 /mnt`
- Use `umount` before disconnecting a device
	- `lsof /mnt` if you get an error about open files
- `findmnt` shows which filesystem is mounted more easier than `mount`

So a recap of all of that:
1. You partition a disk whether with MBR or GPT (preferable)
2. Create a filesystem on the partition (for mounting it)
3. Mount the partition on a directory `/mnt`
4. To unmount, check if the mount point is busy `lsof /mnt`
5. `umount /mnt`
6. Be aware that extended partitions cannot be mount, only logical

---

#### /etc/fstab

- `/etc/fstab` is the main configuration file to persistently mount partitions
- `/etc/fstab` content is used generate systemd mounts by the `systemd-fstab-generator` utility
- To update systemd mounts, it is recommended to use `systemctl daemon-reload` after editing `/etc/fstab`
- When error is found on fstab, you will be dropped at a troubleshooting shell
- After adding lines in fstab, `mount -a` to mount all that hasn't been mount
- `findmnt --verify` to verify syntax

### Labels and UUIDs

In datacenter environments, block device names may change. Different solutions exist for persistent naming
- UUID
- Labels
- Unique device names are created in `/dev/disk/*`

Managing persistent naming attributes:
- `blkid` shows all devices with their naming attributes
- `tune2id -L` to set label on an Ext system
- `xfs_admin -L` used to set a label on an XFS system
- `mkfs.* -L` used to set a label while creating a filesystem


When cloning an XFS filesystem, the UUID is cloned as well, so you lost the uniqueness.
- Use `xfs_admin -U generate /dev/sdb1` to generate a new UUID

> Watch Lesson 18.7 for a demo that shows common use cases for UUID and trouble when removing partitions.

Extracting the UUID of nvme0n1p3 partition
```sh
blkid | grep "/dev/nvme0n1p3" | cut -d " " -f 2
```


#### Systemd Mounts

- Lines in /etc/fstab are converted to systemd mounts
	- Check `/run/systemd/generator` for the automatically generated files
- Mounts can be created using systemd .mount files
- `systemctl cat tmp.mount`

#### Managing swap

- Swap is RAM that is emulated on disk
- All Linux systems should have at least some swap
	- The amount of swap depends on the use of the server
- Swap can be created on any block device, including swap files
- Set partition type to linux-swap
- After creating the swap partition, `mkswap` to create the swap FS
- `swapon` to activate
- Include it on fstab



