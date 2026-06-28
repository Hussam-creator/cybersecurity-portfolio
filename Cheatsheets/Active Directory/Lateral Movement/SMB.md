# Server Message Block (SMB)

Server Message Block (SMB) is a network protocol used for file sharing, printer access, and communication between Windows systems. During an Active Directory assessment, SMB is commonly used to enumerate shares, discover sensitive files, and identify accessible systems within the environment. Misconfigured shares, excessive permissions, or exposed administrative shares can provide valuable information and create opportunities for credential theft, lateral movement, and further compromise of the domain.

---
1.  The user authenticating to the target needs to be in the Administrators local group.
2. The ADMIN$ share must be available.
3. File and printer sharing needs to be turned on.

./PsExec64.exe -i  [\\FILES04 -u corp\jen](file://FILES04%20-u%20corp/jen) -p Nexus123! cmd
