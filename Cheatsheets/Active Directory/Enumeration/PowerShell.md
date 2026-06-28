# PowerShell

## Overview

PowerShell is a powerful scripting and automation framework built into Windows that is widely used during Active Directory assessments. It provides native access to system administration and directory management functions, allowing testers to perform enumeration, execute scripts, and interact with domain resources without requiring additional tools. Due to its flexibility and integration with Windows environments, PowerShell is commonly used to automate reconnaissance tasks and support further stages of an engagement.

---
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
