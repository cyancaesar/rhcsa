Solutions exist for performing automated installations:
- Vagrant is used to automate the deployment of virtual machines
- Cloud-init and other templates used in cloud environment
- Kickstart native Red Hat solution. Can be used with PXE-boot server to provide instructions for automatic installation of RHEL

#### Kickstart File

After installation, `anaconda-ks.cfg` is dropped in the root user home directory. You can edit the file to make any changes. After editing, `ksvalidator /root/anaconda-ks.cfg` to verify syntax.

Common options in Kickstart file:
- `url --url="http://server/"` specifies which URL to access for the installation media
- `repo --name="AppStream" --baseurl=` how to access repositories
- `text` for force text mode installation rather than the graphical installation
- `vnc --password=SECRET` enables the VNC viewer for remote access
- `clearpart --all --drive=sda,sdb` removes all partitions
- `part /home -fstype=ext4 --label=home --size=2048 --maxsize=4096 --grow` creates and mount a partition
- `autopart` creates root, swap and boot partition
- `network --device=ens33 --bootproto=dhcp` configure initial network interface
- `firewall --enabled --service=ssh,http` for firewall configuration
- `timesource --ntp-server pool.ntp.org` set up NTP
- `rootpw --plaintext SECRET` not secure actually
	- `rootpw --iscrypted ENCRYPTED_SECRET`
- `selinux --enforcing` activates SELinux

Kickstart file is provided on the installation server.

Before installation, the client needs to get the Kickstart file from:
- `ks=http://server/ks.cfg`
- `ks=hd:LABEL=USB:/directory/file`
- installation interface provided by the virtual machine manager

#### RHEL-based Virtualization

- Linux kernel offers KVM (Kernel-based Virtual Machine)
- RHEL can be used as a virtualization host

Configuring RHEL to be a virtualization host:

- `grep -E "svm|vmx" /proc/cpuinfo`
- Install virtualization package group `dnf group install "Virtualization Host"`
- Validate the host `virt-host-validate`

Installing virtual machines from the command line:

- `dnf install virt-install`
- `virt-install --name testvm --memory 2048 --vcpus 2 --disk size 20 --os-type linux --cdrom /rhel9.iso`

Using the Web Console:

- `dnf install cockpit-machines`
- `systemctl enable --now cockpit.socket`
- Login in at `localhost:9090`