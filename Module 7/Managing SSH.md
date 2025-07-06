#### Setting SSH Key-based Login

1. `ssh-keygen` to create public/private key pairs
2. `ssh-copy-id` copies the public key over to the server

When using a passphrase, you will be entering the passphrase for every single command, and that is very inconvenient. The solution is to cache with `ssh-agent` in combination with `ssh-add`.
- `ssh-agent /bin/bash`
- `ssh-add`
- Cached passphrase will stay for the rest of the session

#### Common SSH Server Options

Server options are set in `/etc/ssh/sshd_config`
- Port 22
- PermitRootLogin
- PubkeyAuthentication
- PasswordAuthentication
- X11Forwarding
- AllowUsers

##### Copying Files

scp is used to copy securely over the network and its common. Ignore sftp

#### Synchronizing Files

- rsync is using SSH to synchronize files
- If source and target files already exists, rsync only sync their difference
	- `-r` recursively sync entire directory tree
	- `-l` sync symbolic links
	- `-p` preserves symbolic links
	- `-n` dry run before actually syncing
	- `-a` uses archive mode
	- `-A` uses archive mode and also sync ACLs
	- `-X` sync SELinux context

