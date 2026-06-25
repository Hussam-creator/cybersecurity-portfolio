
```
Import-Module ActiveDirectory
```
 
 For basic domain information
```
Get-ADDomain
```

Get a list of accounts, some that may be susceptible to Kerberoasting
```
Get-ADUser
```

Checks for trust information for child-to-parent relationships
```
Get-ADTrust -Filter *

Get-ADGroup -Filter * | select name
```

Detailed Group Info
```
Get-ADGroup -Identity "Backup Operators"
```

Get a list of group members
```

Get-ADGroupMember -Identity "Backup Operators" 
```

Credential Harvesting
```
Get-Content $env:APPDATA\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
```
