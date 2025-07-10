#### Containers

Containers rely on features provided by the Linux operating system:
- Control groups set limits to the amount of resources that can be used
- Namespaces provide isolation to ensure the container only has access to its own data and configurations
- SELinux enforces security

Rootless Containers:
- Container need a user ID to be started on the host
- Root containers are started by the root user
- Rootless containers are started by non-root user
	- It can generate a UID dynamically, or preconfigured

Red Hat Container Tools:
- `podman` manages containers and container images
- `buildah` an advanced tool to create container images
- `skopeo` an advanced tool to manage, copy, delete and sign images

No Docker.

#### Images

- Container images are used to package application with its dependencies into a single unit
- It is build according to the Open Containers Initiative (OCI) specification
- The OCI standard guarantees compatibility
- It is offered by registry

#### Registries

- Public registries like hub.docker.com
- Private registries
- `quay.io` for RHEL optimized community images
- Red Hat distributes certified images on registry.redhat.io
- registry.connect.redhat.com for third-party products
- catalog.redhat.com a web interface to the Red Hat images

#### Red Hat Registries

- `podman login registry.redhat.io` to login to a registry
- `podman login registry.redhat.io --get-login` to get the current login credentials

#### Configuring Registry Access

- Registry access is configured in `/etc/containers/registries.conf`
- Default ones are in the `[registries.search] section`
- Registries with no SSL cert are in `[registries.insecure]`
- User specific is in `~/.config/containers/registries.conf`

#### Containerfile

- Like Dockerfile it contains the instruction to build a container image
- UBI image is used as a base image

---

#### Running Containers

- `podman run` to run a container image
- `podman ps` to see the running container process. Add `-a` to the containers that have stopped
- `podman inspect` to see the container configuration
- You can run an alternative command `podman run ubi8 sleep`
- To run image from a specific registry, provide the FQDN
- Before the name tag on running an image, you can append the command line options to be fed to the image

Reference commands:

- `podman search`
- `podman run`
- `podman stop`
- `podman ps`
- `podman build`
- `podman images`
- `podman inspect`
- `podman pull`
- `podman exec`
- `podman rm`
- `podman logs`
#### Troubleshooting Containers

Hmm? Why would I troubleshoot container if it is meant to run completely on any environment?.

Troubleshooting containers primary to see what the container primary command. Use `podman inspect container` to see the command. `podman run -it ...` to start the container with an interactive terminal.

Run a MariaDB without any command and see the error.


#### Environment Variable and Port Mapping

- Use `-e KEY=VALUE` to set environment variable on the container
- A port on the container host is exposed and forwards traffic to the container port
- Rootless containers can only map to a non-privileged port > 1024

#### Storage

Container storage is volatile. Persistent storage can be provided by making a directory on the host container and bind-mounting that container using:

```sh
podman run -d ... -v /hostdir:/containerdir
```

File ownership on these directories is important.

- Container started by root user, UIDs and GIDs on the host match the UIDs and GIDs in the container
- For a rootless container, make sure UID of the user that runs the container is owner of the bind-mounded directory

Rootless containers are launched in a namespace
- It provides isolation
- `podman unshare` is used to run commands inside the container namespace
- To get UID mapping `podman unshare cat /proc/self_uid_map`

To set directory ownership on bind-mounted directories for rootless containers:

- Find the UID of the user that runs the main application
	- `podman inspect image`
	- `podman exec container grep username /etc/passwd`
- `podman unshare chown nn:nn directoryname` to set the container UID as the owner of the directory on the host 
- Verify mapping with `podman unshare cat /proc/self/uid_map`
- `ls -ld /directory` verify the mapped user is owner on the host
- Before using `podman run -v ...`, you need to take care of SELinux

Configuring SELinux for shared directories:

- To bind mount a host directory in the container, the `container_file_t` SELinux context type must be used
- If ownership on host directory has been configured all right, use the `:Z` option to automatically set this context
	- `podman run ... -v /hostdir:/var/lib/mysql:Z ...`

---

#### Starting Containers as Systemd Services

- To auto start containers, you can create systemd user unit files for rootless containers and manage them with `systemctl`
- If Kubernetes or OpenShift is used, containers will be auto started by default

Running systemd services as a user
- Systemd user services start when user session is opened and closed when user session is stopped
- `loginctl enable-linger` to change that behavior and make systemd user service to run even if the user hasn't logged in
	- `loginctl enable-linger user`
	- `loginctl show-user user`
	- `loginctl disable-user user`

Managing containers using systemd services
- Create a regular user account to manage all containers
- Use podman to generate a user systemd file for an existing container
- You need to do this before: `mkdir ~/.config/systemd/user;cd ~/.config/systemd/user`
- `podman generate systemd --name web --files --new`
	- For root container, do it from `/etc/systemd/system`
- Edit the file that is generated and change `WantedBy` such that it reads `WantedBy=default.target`
- Manage them with `systemctl --user`
	- `systemctl --user daemon-reload`
	- `systemctl --user enable web.service` (requires linger)
	- `systemctl --user start web.service`

Demo for all of that

```sh
sudo useradd nix; sudo passwd nix
sudo loginctl enable-linger nix
ssh nix@localhost
mkdir-p ~/.config/systemd/user; cd ~/.config/systemd/user
podman run -d --name nginx -p 8081:80 nginx
podman ps
podman generate systemd --name nginx --files --new
vim container-nginx.service
	WantedBy=default.target
systemctl --user daemon-reload
systemctl enable container-nginx.service
systemctl status container-nginx.service
```

Survivability notes:
- Log in as the user that should start the container, do not use `su -` su session
-  As the user, `mkdir -p ~/.config/systemd/user; cd ~/.config/systemd/user`
-  `podman generate --new`
- Check the `WantedBy` from systemd service file and check it is `default.target`
- Check `man podman-generate-systemd`

#### Building Images From Containerfile

Check for Containerfile, install example Containerfiles

```sh
dns provides */Containerfile
dnf install buildah-tests
```

Find the Containerfile

```sh
find / -name "Containerfile" -exec ls -l {} \;
```

#### Lab

- Ensure you have access to the Red Hat container repositories
- Run a MariaDB container in Podman, which meets the following conditions:
	- The container is started as a rootless container by user
	- The container must be accessible at host port 3206
	- The database root password should be set to password
	- The container uses the name mydb
	- A bind-mounted directory is accessible: the directory `/home/user/mariadb` on host must be mapped to `/var/lib/mysql` in the container
- The container must be configured such that it automatically starts as a user systemd unit upon start

Check your login:

```sh
podman login registry.redhat.io --get-login
```

Pull the image from Red Hat registry:

```sh
podman pull registry.redhat.io/rhel9/mariadb-1011:9.6-1751964319
```

Run the image without bind-mounted directory:

```sh
podman pull registry.redhat.io/rhel9/mariadb-1011:9.6-1751964319
```

For bind-mounted directory, we need first to find the UID:

```sh
podman inspect registry.redhat.io/rhel9/mariadb-1011:9.6-1751964319 | grep "User"
# "User": "27"
```

Set the ownership of a host directory to the container user:

```sh
podman unshare chown 27:27 mariadb
```

Verify mapping has changed:

```sh
podman unshare cat /proc/self/uid_map
#          0       1000          1
#          1     100000      65536

ls -ld mariadb
# drwx------. 2 100026 100026 6 Jul 10 08:03 mariadb/
```

SELinux will prevent this, add `:Z` when you bind volume:

```sh
podman run -d --name mydb -p 3206:3306 -e MYSQL_ROOT_PASSWORD=password -v /home/student/mariadb:/var/lib/mysql:Z  registry.redhat.io/rhel9/mariadb-1011:9.6-1751964319
```

It will eventually run :)

Configuring systemd to run the container:

1. Enable linger for the current user `loginctl enable-linger user`
	1. `loginctl show-user user`
2. Make a directory for systemd user services and cd into it `mkdir ~/.config/systemd/user`
3. Use podman to generate systemd file `podman generate systemd --name mydb -f --new`
4. Double check `WantedBy` and ensure it is `WantedBy=default.target`
5. Enable via systemd `systemctl --user daemon-reload` and `systemctl enable --now container-mydb.service`

Perfect!