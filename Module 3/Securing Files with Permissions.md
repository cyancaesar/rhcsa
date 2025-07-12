```sh
chown user[:group] file
chgrp group file
```


|             | File   | Directory     |
| ----------- | ------ | ------------- |
| Read (4)    | Open   | List          |
| Write (2)   | Modify | Create/Delete |
| Execute (1) | Run    | cd            |
Special X?
- When x is applied recursively, it makes directories and files executable
- Use `X` in recursive command which makes directories executable and files get executable if it is set already elsewhere on the file

[Linux permissions: SUID, SGID, and sticky bit](https://www.redhat.com/en/blog/suid-sgid-sticky-bit)

- SUID
- GUID
- Sticky Bit

```sh
chmod [#]###
chmod g+s directory
chomd +t
chmod 2770 directory
umask # /etc/.bashrc or ~/.bashrc
source # or simply "."

```


