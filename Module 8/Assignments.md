#### Setting up a Base Server

```txt
Install two servers using the minimal installation pattern. Use
the names server1.example.com and server2.example.com and
use DHCP to get an IP address from the local DNS server.
Ensure that the partition for / is 15 GiB. Also create a 1 GiB swap
partition. Do NOT register the servers with Red Hat.
```

#### Resetting the Root Password

```txt
Use the appropriate solution to reset the root password on
server2, assuming that you have lost the root password and you
have no administrator access to the server anymore
```

#### Configuring a Repository

```
On both servers, create an ISO file based on the installation
DVD. Mount this ISO file persistently and configure the servers
to use the local ISO file as the repositories. After successfully
completing this assignment, you should be able to install
software on both servers.
```

#### Managing Partitions

```txt
On server 1, use your virtualization software to increase the size
of your primary disk in such a way that at least 10 GiB of
unallocated disk space is available

In the free disk space, create a 1 GiB partition and format it with
the vfat filesystem. Make sure it is mounted persistently on the /winfile directory

Also create a 1 GiB swap parititon and ensure it is mounted persistently
```

#### Managing LVM Logical Volumes

```txt
Create a logical volume with the name myfiles. Ensure it uses 8
MiB extents. Configure the volume to use 75 extents. Format it
with the ext4 file system and ensure it mounts persistently on
/myfiles

Increase the size of the / logical volume by 5 GiB

If volume groups need to be created, create them as needed
```

#### Creating Users and Groups

```txt
Create a user lisa. Ensure she has the password set to
"password" and is using UID 1234. She must be a member of
the secondary group sales

Create a user myapp. Ensure this user cannot open an
interactive shell
```

#### Managing Permissions

```txt
Create a shared group directory /sales and ensure that lisa is
the owner of that directory. The owner and the group sales
should have permissions to access this directory and read and
write files in it. Other users should have no permissions at all.
Ensure that any new file that is created in this directory is
group-owned by the group sales automatically.
```

#### Scheduling Jobs

```txt
Schedule a job that writes "hello folks" to syslog every Monday
through Friday at 2 AM. Make sure this job is executed as the
user lisa.
```

#### Managing Containers as Services

```txt
Create a container with the name mydb that runs the mariadb
database as user lisa. The container should automatically be
started at system start, regardless of whether or not user lisa is
logging in. The container further should meet the following
requirements:
- The host directory /home/lisa/mydb is mounted on the container
directory /var/lib/mysql
- You do not have to create any databases in it.
```

#### Managing Automount

```txt
On server2, create the directories /homes/userl and /homes/user2.
Use NFS to share these directories and ensure the firewall does not
block access to these directories.

On server 1, create a solution that automatically, on-demand
mounts server2:/homes/user1 on /homes/userl, and also that
automatically, on-demand mounts server2:/homes/user2 on
/homes/user2 when these directories are accessed
```

#### Setting Time

```txt
Configure server 1 and server2 as an NTP client for pool.ntp.org
```

#### Managing SELinux

```txt
Ensure that the Apache web server is installed and configure it to
offer access on port 82

Copy the file /etc/hosts to /tmp/hosts. Next, move /tmp/hosts to the
directory /var/www/html/hosts and ensure this file can be accessed
by the Apache web server
```