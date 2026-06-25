GPP Passwords
---
Group Policy Preferences (GPP) Passwords

When a new GPP is created, an .xml file is created in the SYSVOL share. These files can contain configuration data and passwords. The cpassword value is AES-256 encrypted but we can find the private key to decrypt the password here:
[https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-gppref/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be?redirectedfrom=MSDN](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-gppref/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be?redirectedfrom=MSDN)

The gpp-decrypt utility can be used to decrypt the password
https://www.kali.org/tools/gpp-decrypt/

```
gpp-decrypt VPe/o9YRyz2cksnYRbNeQj35w9KxQ5ttbvtRaAVqxaE
```

These passwords can be found in the SYSVOL share or using crackmapexec
```
crackmapexec smb -L | grep gpp
```


Snaffler
---

[https://github.com/SnaffCon/Snaffler/releases/tag/1.0.244](https://github.com/SnaffCon/Snaffler/releases/tag/1.0.244)

Snaffler obtains a list of hosts in the domain and enumerates for shares and readable directories by our user. We may find passwords, SSH keys, configuration files and more.

```
.\Snaffler.exe -s -d inlanefreight.local -o snaffler.log -v data
```

