
Connect to windows compromised host
```
xfreerdp3 /v:10.129.42.198 /u:htb-student /p:'HTB_@cademy_stdnt!'
```

Then set up netsh.exe.
```
netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.42.198 connectport=3389 connectaddress=172.16.5.19
```

Now 10.129.42.198:8080 traffic will be forwarded to 172.16.5.19:3389. So we need to xfreerdp to 10.129.42.198:8080 which in turn will connect to 172.16.5.19:3389 (we need credentials for 172.16.5.19)
```
xfreerdp3 /v:10.129.42.198 /u:victor /p:pass@123
```

To disable firewall
```
netsh advfirewall firewall add rule name="port_forward_8080" protocol=TCP dir=in localip=10.129.42.198 localport=8080 action=allow
```