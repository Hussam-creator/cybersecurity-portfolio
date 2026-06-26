# Unknown Ports

## Overview 

During network enumeration, it is important to investigate any unknown or non-standard ports as they may be associated with custom services or misconfigured applications. These services can sometimes expose administrative interfaces, debugging endpoints, or internal tools that are not intended for public access. Identifying the service behind an unknown port is a key part of understanding the attack surface.

---
For unknown ports try navigating to port
e.g.
```
http://IP:PORT
https://IP:PORT
```

or try connecting with netcat (you may get direct terminal access the run cmd 'help')
```
nc IP PORT
```
