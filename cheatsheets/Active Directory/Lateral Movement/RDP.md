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
