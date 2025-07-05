
The advanced storage solutions in RHEL besides partitions:
- LVM Logical Volumes
	- Used in default installation of RHEL
	- Adds flexibility to storage 
- Stratis
	- Next generation Volume Managing Filesystem
	- Implemented in user space which makes API access possible (cloud solutions)
- Virtual Data Optimizer
	- Focusing on storing files in the most efficient way
	- Manages deduplicated and compressed storage pools
	- Integrated in LVM

#### Creating Procedure of LVM

- Create a partition and set type to **lvm**
- Use `pvcreate /dev/sdb1` to create the physical volume
- Use `vgcreate vgdata /dev/sdb1` to create the volume group
- Use `lvcreate -n lvdata -L 1G vgdata` to create the logical volume
- Use `mkfs /dev/vgdata/lvdata` to create a filesystem
- Put in fstab to mount it persistently

Extents are the elementary blocks of LVM allocation and the extent size can be defined while defining volume group.
- Use `vgcreate -s 8M vgdata /dev/sdb1` to create vg with an extent size of 8MB
- Use `vgdisplay` to show properties of volume groups

#### Device Mapper Names

- Device mapper is the system that the kernel uses to interface storage devices.
- Device mapper generates non-persistent names, like `/dev/dm-0` and `/dev/dm-1`
- Persistent names are provided as symbolic links through `/dev/mapper`
	- `/dev/mapper/vgdata-lvdata`
- Alternatively, use the LVM generated symbolic links
	- `/dev/vgdata/lvdata`

#### Resizing Logical Volumes

- Use `vgs` to verify the volume group has unused disk space
- Use `vgextend` to add one or more PVs to the VG
- Use `lvextend -r -L +1G` to grow the LVM logical volume, including the filesystem its's hosting
	- `resize2fs` is a resize utility for Ext filesystem
	- `xfs_growfs` to grow XFS filesystem
- Shrinking is possible on Ext4 filesystems, not on XFS

#### Reducing Volume Groups

- PV can be removed from the VG IF the PVs have sufficient free space to allocate its extents
- If the remaining PVs are fully used, it cannot work
- `pvmove` move used extents from the removed volume to the remaining volume(s)
- `vgreduce` to complete the removal

---

#### Lab

- Add a new 10GiB disk
- Create an LVM logical volume with the name lvdb and a size of 1 GiB. Also create the VG and PV that are required for this LV
- Format this LV with the XFS filesystem and mount it persistently on /mount/lvdb

