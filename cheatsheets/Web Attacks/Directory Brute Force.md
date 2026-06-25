
Fuzzing
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