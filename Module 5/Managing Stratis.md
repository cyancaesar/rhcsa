- Stratis volumes uses XFS
- Stratis volumes are thin provisioned
- Stratis pool allocates the volume storage
- Stratis volume needs to be at least 4GiB
- `stratisd` and `stratis-cli` needs to be installed
- `startis` to create pools and filesystems
- `stratis pool list` to list pool space
- To mount stratis volumes you must use UUID
	- Include `x-systemd.requires=stratisd.service` as a mount option

```sh
dnf install stratisd stratis-cli
systemctl enable --now stratisd
stratis pool create mypool /dev/sdb
stratis pool list
stratis pool add-data mypool /dev/sdc
stratis blockdev list
stratis fs create mypool myfs
mkdir /myfs
lsblk --outout=UUID /dev/stratis/mypool/myfs >> /etc/fstab
# Edit /etc/fstab to include:
# UUID=... /myfs xfs systemd.requires=stratisd.service 0 0
```

#### Stratis Snapshots

- A metadata copy that allows to access the state of the snapshot at any time
- Not a backup, but can be helpful in accessing deleted files
- Snapshot is mounted by device name not by UUID

#### Lab

- Create a stratis pool with as size of 10 GiB, with the name `stratispool`, containing 2 filesystems: myfiles and myprograms
- Mount these volumes persistently on /myfiles and /myprograms
- Copy all files from /etc/ that have a name starting with an a, c, or f to /myfiles
- Create a snapshot of the myfiles filesystem
- Delete all files from /myfiles that have a name starting with an a
- Verify that you can still access these files from the snapshot

