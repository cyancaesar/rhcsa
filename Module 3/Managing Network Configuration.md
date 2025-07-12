```sh
ip link show
ip addr show
hostnamectl hostname NAME
```

- `/etc/hostname` hostname of the machine
- `/etc/hosts` resolves hostname to IP address
- `/etc/resolv.conf` DNS client configuration
- `/etc/nsswitch.conf` hostname resolution orders

```sh
ip addr add dev ens33 10.0.0.10/24
ip [-s] link
ip route
```

NetworkManager is a systemd service that manages network configuration.

- `/etc/NetworkManager/system-connections` NetworkManager configuration

`nmcli` is a utility acts as an interface to NetworkManager. `nmtui` is the TUI version of `nmcli`

`nmcli` cookbook to be cool:

```sh
nmcli con show
nmcli dev status

nmcli con add con-name newconnection ifname esn33 ipv4.address 10.10.0.10/24 ipv4.gateway 10.10.0.1 ipv4.method manual type ethernet

nmcli con up newconnection

nmcli con show newconnection
nmcli con mod
nmcli ocn reload
```