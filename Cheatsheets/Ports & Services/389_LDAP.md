# TCP 389 (LDAP)

## Overview

During directory service enumeration, port 389 is used for LDAP, which is commonly associated with Active Directory environments. It allows querying of users, groups, and domain information. If anonymous binding is enabled or weak credentials are used, it can provide valuable information for further domain enumeration and attack path discovery.

---
```
nmap -n -sV --script "ldap* and not brute" [target IP]
```


LDAPSearch
```
ldapsearch -x -H ldap://[target IP] -b "DC=domain,DC=local"
```
```
ldapsearch -x -H ldap://[target IP] -s base namingcontexts
```
```
ldapsearch -x -H ldap://[target IP] -D '[user@domain]' -w '[password]' -b 'DC=domain,DC=local' | tee [file to save]
```
- Next we can grep for passwords and stuff
```
 cat [file name]| grep -i pass
```
- or description

If LAPS is running (check in Program Files)
```
ldapsearch -x -H ldap://[Target IP] -D '[domain\username]' -w '[password]' -b 'dc=domain,dc=local' "(ms-MCS-AdmPwd=*)" ms-MCS-AdmPwd 
```

netexec
```
netexec ldap [target IP] -u [username] -p '[password]' --users
```
```
netexec ldap -d [domain name] -u '[username]' -p '[password]' -M adcs
```
- Certificate Service 

A webserver running on machine with LDAP enabled may use LDAP for authentication, Try using * in the username and password fields
