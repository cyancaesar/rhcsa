
### Shell Jobs

Start a job in the background

```sh
command &
```

Moving a job to a background using `ctrl+z` to stop it and then issue `bg` command

View all jobs
```sh
jobs
```

Move last job to the foreground
```sh
fg [n]
```

---

### Using ps

ps command has two different dialects: System V and BSD
- System V: options do have a leading dash `-`
- BSD: options do not have a leading dash

Overview of all processes
```sh
ps aux
```

Shows hierarchical relations between processes
```sh
ps -fax
```

Shows all processes owned by linda
```sh
ps -fU linda
```

Shows a process tree for a specific process
```sh
ps -f --forest -C sshd
```

Shows format specifiers
```sh
ps L
```

Some specifiers to show a list of processes
```sh
ps -eo pid,ppid,user,cmd
```

---

### Monitoring Memory Usage

Get details of current memory usage
```sh
free -m
```

More detailed memory information can be found on `/proc/meminfo`.

- When writing files, a write cache (buffer) is used.
- The write cache is periodically committed to disk by the `pdflush` kernel thread.
- As a result, after committing a file write, it's not committed immediately.
- Run `sync` command to ensure file is written to disk immediately.

---

### Observe CPU Load

- CPU load is checked with `uptime`
- It is expressed as an average number of runnable processes over the last 1, 5, and 15 minutes
- A rough guideline, this number should not exceed the number of CPU cores on a system
- Use `lscpu` to see how many cpu cores does the system have.

### Using top

- `top` a dashboard for monitoring system activities
- Press `f` to show and select fields
- Press `M` to filter on memory usage
- Press `W` to save new display settings
- `htop` a common alternative but not installed by default


---

### Managing Processes


- `kill` command is used to send signals to PID
- Use `k` from `top`
- `pkill` and `killall` variations

```sh
ps aux | zombie
kill <childid> # fails
kill -SIGCHLD <ppid> # ignored
kill <ppid> # zombie will be adopted by init, and reaped away
```

### Managing Priority

```sh
nice -n 10 dd if=/dev/zero of=/dev/null &
```

### Using tuned Profiles

```sh
sysctl -a
```

We can use `tuned` to optimize system using pre-defined profiles instead of tuning manually which takes a lot of effort.

Show current profiles
```sh
tuned-adm list
```

set to virtual-guest profile
```sh
tuned-adm profile virtual-guest
```

- Custom tuned profiles are stored in `/etc/tuned`
- Each profile should have a file `tuned.conf`

---

### Managing User Sessions and Processes

Show processes owned by a specific user
```sh
ps -u username
```

Remove processes owned by a specific user
```sh
pkill -u username
```

Shows users and sessions
```sh
loginctl list-users/list-sessions
```

Shows a tree of processes currently opened by this user
```sh
loginctl user-status <uid>
```

Stop current sessions or users
```sh
loginctl terminate-session/terminate-user
```

