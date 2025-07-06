#### Socket Statistics (ss)

ss is the standard tool to show socket information
	- `ss` alone shows all connections
	- `ss -tu` shows TCP and UDP sockets
	- `ss -tua` adds sockets that are in listening state
	- `ss -tln` shows TCP sockets that are only in the listening state
	- `ss -tulpn` shows TCP and UDP socket in listening state, and adds process and PID to the output

#### RHEL Firewalling

- Linux kernel provides the netfilter framework to take care of firewall related network operation like packet filtering, NAT, port forwarding
- Netfilter forwards specific operation to kernel modules
- `nftables` is the framework that applies firewalling
- `firewalld` is a service, managed by systemd, which RHEL uses as a front end to manage `nftables` firewalls

#### Configuring a Firewall

- firewall-cmd is the tool to write a firewall configuration
- `--permanent` to write to persistent but not to the runtime (reload firewall)
- without `--permanent` it write to only to the runtime
- So run it twice with and without

```sh
# Allowing incoming HTTP traffic
firewall-cmd --list-all
firewall-cmd --get-services
firewall-cmd --add-service http
firewall-cmd --add-service http --permanent
```

