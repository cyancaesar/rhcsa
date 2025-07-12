Finding things:

```sh
find / -name "hosts"

find / -type f -size +100M

find /etc -exec grep -l root {} \; -exec cp {} content \; 2>/dev/null

find /etc -name '*' -type f | xargs grep "127.0.0.1"
```

Filesystem mounts:

```sh
mount
findmnt
lsblk
```

Links:

```sh
# Hard link & symbolic link
ln # hard link
ln -s # symbolic link
```

Archiving:

```sh
tar -cvf archive.tar /home /etc # -z, -j, -J for compression
tar -tvf
tar -xvf archive
```

Compression:

```sh
gzip # -z
bzip # -j
zip # Windows-compatible
xz # -J
```

