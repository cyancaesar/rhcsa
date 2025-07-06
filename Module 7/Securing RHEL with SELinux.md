#### SELinux Modes

SELinux is either disabled or enabled and to switch between them you need to reboot.

Enabled state offers two modes:
- Enforcing: normal operational mode
- Permissive: only logging

- `setenforce` to toggle between modes
- `getenforce` to show the current state
- `/etc/sysconfig/selinux` to manage the default state
- SELinux state can also be set during boot using the kernel parameter
	- `enforcing=1` set to enforcing mode
	- `enforcing=0` set to permissive mode
	- `selinux=1` enable SELinux
	- `selinux=0` disable SELinux

#### SELinux Components

- SELinux works with *context labels*
- Context labels define specific permissions and are applied to source objects and target objects
- Source objects are users and processes
- Target objects are files, directories and ports
- Context management is just applying contexts to target objects
- Booleans allow parts of the SELinux policy to be rewritten to allow or disallow specific functionality

#### Context Labels

- Every object is labeled with a context label
	- user
	- role
	- type
- Many command support `-Z` option to show current context information
- Context types are used in the rules in the policy to define which source object has access to which target object


Context inheritance:

- When a file are created in a directory, is inherits the context from its parent
- When a file is copied, it inherits the context from its parent directory
- When file is moved. it keeps its original context

Managing file context labels:

- `semanage fcontext` set the file context label
	- `semanage fcontext -a` set new context label
	- `semanage fcontext -m` modify context label
- `restorecon` to enforce the policy setting on the file system
- Alternatively, `touch /.autorelabel` to relabel all files to the context that is specified in the policy
- `man semanage-fcontext`
- Do NOT use `chcon`. It writes to the inode and not the policy
- `semanage fcontext -l -C` shows only settings that have changed in the current policy

Finding the right context:
- Check the default configuration context setting if you've applied non-default configuration
- Man pages from `selinux-policy-doc`
- Use `sealert`

```sh
# Install selinux-policy-doc
dnf install selinux-policy-doc

# Query the man page of SELinux policy docs
man -k _selinux

# Get httpd docs
man -k _selinux | grep httpd
```

#### Managing Ports

- SELinux policy is configured to allow default port access
- `semanage port` to apply non-default port access
- `man semanage-port` for examples

When changing default configuration and the changed service failed to start, do these tips:
1. View systemd error logs
2. Set SELinux mode to permissive and retest
3. If it passes then SELinux is the problem
4. `grep AVC /var/log/audit/audit.log` to grep all SELinux errors

Boolean enables you to selectively enable or disable specific parts of the SELinux policy.

- All booleans  can be viewed with `seamanage boolean -l` or `getsebool -a`
- Set booleans `setsebool -P boolean [on|off]`
- `semanage boolean -l -C` all boolean that have non-default setting

#### sealert

- SELinux uses *auditd* to write log messages to the audit log
- sealert interprets messages from the audit log and writes meaningful messages to `/var/log/messages`
- `journalctl | grep sealert` to grep sealert with uuid and get a suggestion on how to fix

Common issue in early RHEL 9, `/.autorelabel` might cause the system to do infinite reboot, and it happens because it cannot remove `/.autorelabel` because of a Read-only filesystem. Procedure to fix:

1. Boot into emergency target `systemd.unit=emergency.target`
2. Remount as read-write `mount -o remount,rw /`
3. Reconstruct the labels manually `restorecon -Rv /`
4. Reboot

#### Lab

- Configure the Apache web server to bind to port 82
- Use `mv /etc/hosts /var/www/html/` and ensure that the file gets an SELinux context that makes it readable by the Apache web server

