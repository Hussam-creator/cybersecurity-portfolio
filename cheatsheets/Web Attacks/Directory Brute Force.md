# Directory Brute Forcing

## Overview

During the enumeration phase, it is important to identify any hidden directories, files, or subdomains that may expand the application's attack surface. Directory and file fuzzing can help uncover endpoints that are not linked from the main website but are still accessible. Tools such as ffuf and Gobuster are commonly used for this purpose. By supplying a wordlist and targeting the application, these tools can enumerate hidden content, revealing administrative panels, API endpoints, backup files, development pages, and other resources that may be valuable during further testing.

---
```
ffuf -u http://site.com/FUZZ -w /usr/share/wordlists/dirb/big.txt
```
```
gobuster dir -u [target IP] -w /usr/share/wordlists/dirb/common.txt -t 42 -x php,txt,html,pdf -b 404
```

File Extension 
```
ffuf -u "https://site.com/indexFUZZ" -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt -fs xxx
```

Subdomain
```
ffuf -u http://site.com -H "HOST: FUZZ.site.com" -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -ac
```

Recursion - checking for 1 directory next if found
```
ffuf -c -u http://[target IP]/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt -recursion -recursion-depth 1 
```

Custom Wordlist
```
cewl <domain> -w wordlist.txt
```

Wordlists
- Common.txt (dirb)
- Medium 2-3 (dirbuster)
- Raft Medium Directory (seclist)
- Raft Medium text (seclist)

If nothing works:
Big.txt (seclist)
Check if URL does not need a /
