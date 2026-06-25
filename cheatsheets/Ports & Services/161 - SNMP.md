
Snmpwalk queries the OIDs, whereas onesixtyone brute-forces the names of the community strings.

snmpwalk
```
snmpwalk -v2c -c public 10.129.14.128
```

OneSixtyOne
```
onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt 10.129.14.128
```
