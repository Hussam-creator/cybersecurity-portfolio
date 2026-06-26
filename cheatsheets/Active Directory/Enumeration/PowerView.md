# PowerView

## Overview

PowerView is a PowerShell-based reconnaissance tool designed for enumerating Active Directory environments. It provides extensive functionality for gathering information about domain users, groups, computers, trusts, sessions, and access control settings. During an assessment, PowerView enables testers to identify privileged accounts, discover potential privilege escalation opportunities, and understand the overall structure and security posture of the domain.

---
[https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon)

To execute scripts 
```
Powershell -ep bypass
```

Enumeration
```
Import-Module .\PowerView.ps1
```

```
Get-NetDomain
```

```
Get-NetUser
```
```
Get-NetUser | select cn
GetNetUser | select description
```

```
Get-NetGroup
```
```
Get-NetGroup | select cn
```
```
Get-NetGroup "[group]" | select member
```

```
Get-NetGroupMember
```


Test For admin access on current or remote machine
```
Test-AdminAccess - ComputerName ACADEMY-EA-MS01
```

Users with SPN set may be susceptible to Kerberoasting
```
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```
```
Get-NetUser -SPN | select samaccountname,serviceprincipalname
```

Shares we can access in the domain
```
Find-DomainShare -CheckShareAccess
```
