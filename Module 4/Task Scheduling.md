- `systemd` timers are the primary solution for scheduling recurring jobs in RHEL 9
- `crond` older scheduling solutions
- `at` schedule non-recurring user tasks

### systemd Timers

- Systemd unit.timer files to go together with unit.service files
- Enable/disable the timer file NOT the service unit
- `OnCalendar` option in timer unit file, specify the when service should be started
- `OnUnitActivateSec` starts the unit a specific time after the unit was last activated
- `OnBootSec` or `OnStartupSec` starts the unit a specific time after booting



---

### cron

- cron service checks its config every minute
- `/etc/crontab` is the main (managed) config file
- `/etc/cron.d` for drop-in files
- `/etc/cron.{hourly,daily,weekly,monthly}` for drop-in for scripts
- User specific cron jobs created by `crontab -e`

### anacron

- Ensures jobs executed on regular basis, but not at a specific time
- It takes care of the jobs in `/etc/cron.{hourly,daily,weekly,monthly}`
- Configuration is in `/etc/anacrontab`

### at

- atd service must be running to run once-only jobs using at
- `at <time>` to schedule a job
	- Type one or more job specifications in the at interactive shell
	- Use `CTRL+D` to close this shell
- Use `atq` for a list of jobs currently scheduled
- Use `atrm` to remove jobs from the list

---

### Managing Temporary Files

TO BE CONTINUED