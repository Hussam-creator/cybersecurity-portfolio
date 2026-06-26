# Active Directory Lab
## Overview

This repository documents my self-hosted Active Directory (AD) lab environment, which I built to develop and practise Windows and Active Directory penetration testing techniques in a safe and controlled setting.

The lab simulates a small enterprise network and is designed to replicate common configurations, services, and vulnerabilities that may be encountered during real-world security assessments.

## Purpose

The primary objectives of this lab are to:

Develop and refine Active Directory enumeration techniques.
Practise privilege escalation and lateral movement scenarios.
Test and understand common AD attack paths and misconfigurations.
Gain hands-on experience with Windows administration and domain management.
Validate and experiment with offensive security tools and methodologies.

## Areas of Practice

This lab is used to explore a range of Active Directory security concepts, including:

Domain enumeration and reconnaissance
Access Control List (ACL) abuse
Kerberos-based attacks
Active Directory Certificate Services (AD CS) attacks
Group Policy Object (GPO) abuse
Credential access techniques
Lateral movement using RDP, WinRM, and SMB
Privilege escalation and attack path analysis using BloodHound and PowerView

##Disclaimer

This environment exists solely for educational and research purposes. All testing is performed on systems that I own and control, and no techniques demonstrated in this repository are intended for use against unauthorised systems.

| Machine | Role | Operating System |
|----------|-------|------------------|
| DC01 | Domain Controller | Windows Server 2022 |
| WIN10-01 | Workstation | Windows 10 |
| KALI | Attacker Machine | Kali Linux |
