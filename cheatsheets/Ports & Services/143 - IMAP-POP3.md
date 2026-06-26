# TCP 143 (IMAP)

## Overview

During email service enumeration, port 143 is used for IMAP, which allows clients to access and manage email messages on a mail server. If misconfigured or exposed without encryption, it may allow credential capture or brute-force attacks against user accounts. It is often worth checking for authentication mechanisms and supported commands during testing.

---
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
