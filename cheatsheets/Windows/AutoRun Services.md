
Use accesschk.exe to check for permissions to binaries
.\accesschk.exe /accepteula -uwcqv [user]

Querying AutoRun services
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
![[Pasted image 20260416140409.png]]
- Use accesschk.exe to see permissions

![[Pasted image 20260416140423.png]]
- This file is writable by everyone, we can overwrite this with a reverse shell

![[Pasted image 20260416140446.png]]

Now we restart our RDP session (or machine?) to trigger the shell 