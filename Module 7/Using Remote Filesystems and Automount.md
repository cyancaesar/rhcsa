#### Base NFS Server

```sh
dnf install nfs-utils
mkdir -p /nfsdata /home/ldap/ldapuser{1..9}
echo "/nfsdata *(rw,no_root_squash)" >> /etc/exports
systemctl enable --now nfs-server
for i in nfs mountd rpc-bind; do firewall-cmd --add-service $i --permanent; done
firewalld --reload
```

#### Mounting NFS Shares

- `showmount -e nfsserver` show shared exports
- `mount nfsserver:/share /mnt` to mount

AutoFS:
- Install `autofs`
- `/etc/auto.master` place directories to let automount service to manage it
	- `/data /etc/nfs.data`
- `/etc/auto.nfsdata` identify the subdirectories on which to mount, and what to mount
	- `files -rw nfsserver:/nfsdata`
- `systemctl enable --now autofs`
- `/etc/auto.misc` useful to syntax example

#### Lab

- Use a second server with the name `nfsserver`
- On the server, create home directories `/home/ldap/ldapuser{1..9}`
- Configure the server to export these home directories
- Automount the home directories on /homes/