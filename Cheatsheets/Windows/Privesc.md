
```
. .\PowerUp.ps1; Invoke-AllChecks
```
```
whoami /priv
```
WinPEAS

1) Service Misconfigurations
- Services running as **SYSTEM / Administrator**
- Writable service executables
```
Get-Service
```
```
sc qc VulnService
```
- powershell
- Look for no quotes around a path with spaces
```
icacls "C:\Path\To\Service.exe"
```

2) Password Reuse
- Config files
- Saved credentials 
Use creds to;
- runas
- SMB
- psexec
- evil-winrm
```
C:\Users\*\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
```

3) Token Privileges 
```
whoami /priv
```
- SeImpersonate
-- Potato attacks 
--Check if we are service account 
- SeAssignPrimaryTokenPrivilege
- SeBackupPrivilege
- SeRestorePrivilege
- SeTakeOwnershipPrivilege
- SeDebugPrivilege
- SeCreateGlobalPrivilege

4) Scheduled Tasks
- check for tasks running as SYSTEM or Admin

5) AlwaysInstallElevated
- Check MSI install policy in registry

6) Group Membership Abuse 
```
whoami /groups
```
- Administrators
- Backup Operators
- Server Operators
- Print Operators
- Event Log Readers
- IIS_IUSRS / Web Service Accounts
- Remote Desktop Users
- Account Operators
- DNSAdmins

7) systeminfo
- for OS exploits

Interesting File Enumeration
```
findstr /SIM /C:"pass" *.ini *.cfg *.config *.xml
```

Hidden Files
```
dir /a
```

Check if LAPS is enabled in Program Files then run command
```
ldapsearch -x -H ldap://[Target IP] -D '[domain\username]' -w '[password]' -b 'dc=domain,dc=local' "(ms-MCS-AdmPwd=*)" ms-MCS-AdmPwd 
```

