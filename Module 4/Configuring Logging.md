- **systemd-journald** is receiving log messages from different locations
	- The kernel
	- Early boot procedure
	- Syslog events
	- Standard output and error from daemons
- The systemd journal is non-persistent by default
- **rsyslog** service reads syslog messages and writes them to different locations
	- Files in `/var/log`
	- According  to output modules
- Services may write to `/var/log`

### Viewing systemd-journald messages

- `systemctl status name.unit` view last messages
- `journalctl` prints the entire journal
- `journalctl -p err` shows only messages with a priority error and higher
- `journalctl -f shows last 10 lines and append on message
- `journalctl -u sshd.service` shows messages for sshd.service only

### Viewing Boot Logs

- `journalctl -b` shows the current boot log
- `journalctl -xb` adds explanation texts to the boot log messages
- `journalctl --list-boots` shows all boots that have been logged
- `journalctl -b 3` shows messages from the third boot log only

### Persistent journald


### Rsyslog

- Rsyslog needs rsyslogd service to be running
- Main config: `/etc/rsyslog.conf`
- Snap-in files can be placed in: `/etc/rsyslog.d/`
- Logger line contains three items
	- facility
	- severity
	- destination
- Log files are in `/var/log`
- `logger` command to write messages to rsyslog manually

### logrotate

- `logrotate` command is started by systemd timer to prevent log files to grow big
- After rotation, log file are appended with the rotation date as an extension
- Old rotated files are discarded
- Configuring is found on `/etc/logrotate.conf` and `/etc/logrotate.d/`

