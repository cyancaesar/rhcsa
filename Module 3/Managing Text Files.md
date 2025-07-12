```sh
more
less
head # -[n]nn
tail # -[n]nn
cat # -A -b -s
tac # useless command in linux, a cat command but in reverse
cut
sort
tr
grep

# Text manipulation
awk -F : '{print $4}' /etc/passwd
sed -n 5p /etc/passwd
```