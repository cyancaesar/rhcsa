```bash
# Copy the disk image to rhel9.iso file
lsblk # Check /dev/sr0|sr1 is connected
dd if=/dev/sr1 of=/rhel9.iso bs=1M

# Mount rhel9.iso to /repo
mkdir /repo
vi /etc/fstab
# /rhel9.iso /repo iso9660 defaults 0 0

# Configure BaseOS and AppStream repositories

cd /etc/yum.repos.d
vi base.repo
# [Base]
# name=BaseOS
# baseurl=file:///repo/BaseOS
# gpgcheck=0

vi appstream.repo
# [AppStream]
# name=AppStream
# baseurl=file:///repo/AppStream
# gpgcheck=0

# Check the repositories
dnf repolist

# You can now install a package!
dnf install -y vim
```