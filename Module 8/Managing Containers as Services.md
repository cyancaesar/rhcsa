```sh
loginctl enable-linger lisa # To enable auto-start for systemd service even if the user hasn't logged in (root)

# Continue as user (lisa)
podman pull docker.io/centos/mariadb-103-centos7
mkdir ~/mydb
podman unshare chown 27:27 mydb

# Run podman container
# ":Z" take cares of SELinux as we already changed the ownership of the directory "mydb" to the user id of the container (27)
podman run -d --name mydb -e MYSQL_ROOT_PASSWORD=password -v /home/lisa/mydb:/var/lib/mysql:Z docker.io/centos/mariadb-103-centos7

# Verify everything is running
podman ps

# Creating the container as service
mkdir ~/.config/systemd/user
cd ~/.config/systemd/user
podman generate systemd --name mydb --files --new

# Reboot and verify from root user
ps faux | less
# lisa services exist  ✅
```

