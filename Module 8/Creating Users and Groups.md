```sh
groupadd sales
useradd -u 1234 lisa # -G sales to skip the `usermod -aG sales`
passwd lisa
# Enter "password"
usermod -aG sales
useradd -s /sbin/nologin myapp
```