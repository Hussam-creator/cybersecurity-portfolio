# UDP 161 (SNMP)

## Overview

During network enumeration, port 161 is used by SNMP to monitor and manage network devices. If community strings are weak or exposed, SNMP can reveal detailed system information such as running processes, network interfaces, and system configuration. This information is often useful for building a clearer picture of the target environment.

---
Snmpwalk queries the OIDs, whereas onesixtyone brute-forces the names of the community strings.

snmpwalk
```
snmpwalk -v2c -c public 10.129.14.128
```

OneSixtyOne
```
onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt 10.129.14.128
```
