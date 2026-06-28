
UDP Scan
```
nmap -T5 10.10.11.48 -sU --max-rtt-timeout 500ms --initial-rtt-timeout 250ms --max-retries 2 -F
```

TCP Scan
```
nmap -sCV -A 10.129.231.23 -p-
```

If Nmap scan is slow
```
nmap 10.129.25.121 -p- --max-retries 0 -Pn
```