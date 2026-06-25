
Use telnet to interact with the server
```
telnet 10.129.14.128 25
```

HELO or EHLO to initialize the session
VRFY command can be used to enumerate users but my not always work reliably, response code 252 to confirm
- VRFY root

Username enumeration
```
hydra smtp-enum://<IP>/vrfy -L "/usr/share/seclists/Usernames/top-usernames-shortlist.txt"
```

EXPN similar to VRFY but when used with a distribution list it will list all users
```
EXPN support-team
```

Password Attacks
```
hydra -L users.txt -p 'Company01!' -f 10.10.110.20 pop3
```