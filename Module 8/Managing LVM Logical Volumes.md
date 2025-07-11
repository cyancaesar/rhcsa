
Create the partition:

```sh
fdisk /dev/sda
n
<Enter>
<Enter>
<Enter>
t
lvm
w
```

Install lvm2

```sh
dnf install -y lvm2
```

Use `vgcreate` to create the volume group (no need for `pvcreate` as `vgcreate` takes care of it)

```sh
vgcreate -s 8M vglab /dev/sda6
lvcreate -n myfiles -l 75 /dev/vglab
mkfs.ext4 /dev/vglab/myfiles
mkdir /myfiles
# Mount on fstab
/dev/vglab/myfiles /myfiles ext4 defaults 0 0
```

Increase the size to 5G

```sh
lvextend -r -L +5G /dev/vglab/myfiles
```

Reboot.