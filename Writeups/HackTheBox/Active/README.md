# HackTheBox - Active

![active](Images/active.png)

## Overview

- Difficulty: Easy
- Platform: Active Directory
- Skills Demonstrated: Active Directory, SMB Enumeration, Credential Discovery, Kerberoasting, Password Decryption, Remote Service Execution

## Methodology 

The assessment followed a standard attack methodology:

1. Enumeration
2. Vulnerability Identification
3. Initial Access
4. Privilege Escalation
5. Post Exploitation
---

## Enumeration 

A network port scan was performed to identify accessible ports and services on the target host 
```
nmap 10.129.6.90 -sCV -A -p-
```
```
Starting Nmap 7.95 ( https://nmap.org ) at 2026-05-30 12:14 BST
Nmap scan report for 10.129.6.90
Host is up (0.018s latency).
Not shown: 65513 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-05-30 11:14:44Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
...
```
Key Findings:
- The target is Likely a Windows Active Directory Domain Controller based on the exposed services; Kerberos (88), LDAP (389) and DNS (53)
- Port 135 - RPC is open
- Port 445 - SMB is open

Initial enumeration was focused on port 445 as vulnerable SMB shares can often lead to exposed sensitive information, or misconfigured network shares and can provide direct pathways to initial access.

