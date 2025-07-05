
### Managing Packages 

Shows all packages that are currently installed
```sh
rpm -qa
```

Shows from which package _filename_ was installed
```sh
rpm -qf filename
```

Lists files installed from a package
```sh
rpm -ql filename
```

Shows scripts executed while installing the package 
```sh
rpm -q --scripts
```

Shows the change log for a package
```sh
rpm -q -changelog
```

Querying the package files is by adding `-p` to any above commands

### Extracting RPM Packages

Convert an RPM package to cpio format and shows the content
```sh
rpm2cpio mypackage-1.0.rpm | cpio -tv
```

Extract the content
```sh
rpm2cpio mypackage-1.0.rpm | cpio -idmv
```

---

### Manually Configuring Repository Access

To access repositories offered by subscription manager to add repository access
```sh
dnf config-manager --enable package_name
```

Third party repositories can be added using a repo file in `/etc/yum.repos.d` or using `dnf config-manager`
```sh
dnf config-manager --add-repo="file:///repo/AppStream"
```

Or by this
```sh
cat >> /etc/yum.repos.d/AppStream.repo <<EOF
> [AppStream]
> name=AppStream
> baseurl=file:///repo/AppStream
> gpgcheck=0
> EOF
```

_There is a step where you need to manually mount a file of a repo before using the adding the repo._

##### GPG Keys

Before installing a package, GPG is used to check the signature of the package to ensure it is not tampered. You can bypass that to a trusted packages by adding `gpgcheck=0` option the repository configuration file.

---

### Using dnf command

List all available packages
```sh
dnf list
```

Search for the name and summary _add `all` to search for description also_
```sh
dnf search 
```

Search for the package file list
```sh
dnf provides
```

Show information of a package
```sh
dnf info
```

Install a package
```sh
dnf install
```

Remove a package _and it's dependencies (Dangerous!)_
```sh
dnf remove
```

Update a package
```sh
dnf update
```

Update the kernel
```sh
dnf update kernel
```

---

### dnf Groups

Show a list of groups _add `hidden` to show all_ 
```sh
dnf group list
```

Show all packages within a group
```sh
dnf group info group_name
```

To install optional packages 
```sh
dnf group install --with-optional
```

---

### dnf Modularity and History

- dnf uses modularity to keep different versions of the same package to be maintained in the same repository.
- RHEL 9 offers 2 main repositories:
	- BaseOS: contains OS content for RHEL. Packages in BaseOS share the OS lifecycle.
	- AppStream: don't have the same lifecycle as RHEL.

- All transaction that dnf perform are inside `/var/log/dnf.rpm.log`
- `dnf history` to view the transactions
- `dnf history undo n` to undo specific transaction

