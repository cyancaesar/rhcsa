#### Linux Time

- `hwclock` is used to set hardware time
- `hwclock [--systohc|--hctosys]` to synchronize
- `date` to show and set time
- `timedatectl` the new utility of managing time and time zone
	- `timedatectl status`
	- `timedatectl set-time`
	- `timedatectl set-timezone`
	- `timedatectl set-ntp`

To synchronize time using NTP, an NTP service must be configured and on RHEL 9, it uses `chrony`.

Another NTP service that is not currently used `systemd-timesyncd.service`

#### NTP Client

- `chronyd` is the default RHEL 9 NTP service.
- `/etc/chrony.conf` for changing synchronization parameters
	- `pool 2.rhel.pool.ntp.org iburst`
	- `server server.example.com`
- Restart chronyd
- `chrony sources` to verify proper sync



