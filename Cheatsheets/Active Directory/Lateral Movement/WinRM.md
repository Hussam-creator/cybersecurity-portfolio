# Windows Remote Management (WinRM)

## Overview

Windows Remote Management (WinRM) is a Windows service that enables remote administration and command execution over the network. It is commonly used by administrators for managing systems and is frequently leveraged during Active Directory assessments to execute commands remotely when valid credentials are obtained. Enumerating WinRM access can help identify systems that permit remote management and potential paths for lateral movement or privilege escalation within the domain.

---
Enumerating members of the Remote Management Users group using PowerView

```
Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Management Users"
```

We can use the below custom query in BloodHound to find users with this access

```
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:CanPSRemote*1..]->(c:Computer) RETURN p2
```

Establishing a WinRM session from Windows using Enter-PSSession

```
$password = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force
$cred = new-object System.Management.Automation.PSCredential ("INLANEFREIGHT\forend", $password)
Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential $cred
```
```
Exit-PSSession
```


