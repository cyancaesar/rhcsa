```sh
useradd #adduser
usermod
userdel
passwd
vim /etc/login.defs
touch /etc/skel/<drop-in> # drops files on user home directory upon creation
usermod -L user # lock user
usermod -U user # unlock user
usermod -e 2032-01-01 user # user `user` will expire on 2032-01-01
usermod -s /sbin/nologin user # user `user` will not login
newgrp
id
groupadd
groupdel
groupmod
lid -g groupname
chage
```