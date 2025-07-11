```sh
dnf install -y httpd

vim /etc/httpd/conf/httpd.conf
# Listen=82

systemctl edit httpd.socket
# ListenStream=82

semanage port -a -t http_port_t -p tcp 82

cp /etc/hosts /tmp/hosts
mv /tmp/hosts /var/www/html/hosts

ls -lZ /var/www/html/hosts # It uses the context of tmp directory

restorecon /var/www/html/hosts # To restore inconsistent context
```