```sh
# Creating partitions
fdisk /dev/sda
n
<Enter>
<Enter>
+1G
n
<Enter>
<Enter>
+1G
t
swap
w

# Formatting
mkfs.vfat /dev/sda4
mkswap /dev/sda5

# Insert into fstab
vim /etc/fstab
/dev/sda4 /winfile vfat defaults 0 0
/dev/sda5 none swap defaults 0 0

# Activate all partitions marked as swap 
swapon -a

# Reboot if swap is not activated
df -h

# Ensure everything is alright, otherwise you will be in trouble later
reboot
```