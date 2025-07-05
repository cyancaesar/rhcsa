
To view all types of units
```sh
systemctl -t help
```

- Service units to start processes
- Socket units monitor activity on a port and start the corresponding Service unit when needed
- Timer units to start services periodically
- Path units can start Service units when activity is detected in the file system
- Mount units for mounting file systems
- Other unit types

Show the current status of any unit
```sh
systemctl status sshd
```

Start/stop a unit
```sh
systemctl start/stop
```

Flag the unit for automatic starting upon system start
```sh
systemctl enable [--now]
```

Flag the unit to be no longer auto started
```sh
systemctl disable [--now]
```

Reload unit configuration without restarting the unit
```sh
systemctl reload
```

Restarts the unit after which the process it manages gets a new PID
```sh
systemctl restart
```

### Modifying Systemd Unit Configs

- Default system-provided systemd unit files are in `/usr/lib/systemd/system`, don't override it
- Custom unit files in `/etc/systemd/system`
- Run-time auto-generated unit files `/run/systemd`
- Better use `systemctl edit UNIT`
- List of unit tunables `systemctl show UNIT`
- `systemctl cat UNIT` to view the original and override config files
- `systemctl list-dependencies UNIT` to show unit dependencies

### systemctl mask

- Some unit cannot work simultaneously on the same system
- To prevent administrators from accidentally starting these units, use `systemctl mask`
- It links a unit to `/dev/null` device
- `systemctl unmask` to remove the mask





