
If we find credentials we can login to the mail server and read or send messages
```
curl -k 'imaps://10.129.14.128' --user user:password -v
```

To interact over SSL we can use openssl
```
openssl s_client -connect 10.129.14.128:pop3s
openssl s_client -connect 10.129.14.128:imaps
```

Reading Inbox
```
a1 LOGIN robin robin
a1 LIST "" *
a1 SELECT DEV.DEPARTMENT.INT
a1 FETCH 1 BODY.PEEK[]
```
