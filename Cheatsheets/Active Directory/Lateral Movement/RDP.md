# Remote Desktop Protocol (RDP)

## Overview

Remote Desktop Protocol (RDP) is a remote management service that allows users to interact with Windows systems through a graphical interface. During an Active Directory assessment, identifying systems with RDP enabled and determining which accounts have remote access can reveal opportunities for lateral movement. Misconfigured permissions, reused credentials, or privileged accounts with RDP access may allow an attacker to move between systems and gain access to sensitive resources.

---
Enumerating members of the Remote Desktop Users group using PowerView

```
Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Desktop Users"
```

After gaining control of a user, always check what remote access they have. We can use Bloodhound for this


Enable RDP and remove firewall
```
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v "fDenyTSConnections" /t REG_DWORD /d 0 /f
```
```
netsh advfirewall set allprofiles state off
```
