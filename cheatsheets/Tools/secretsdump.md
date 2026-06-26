
```
impacket-secretsdump [domain/username:password@target IP]
```

Get admin hash with mimikatz then use the hash with secretsdump

This will dump user hashes 
- We can try to crack them
- Compare hashes to see if there is password reuse - we might know the password
- Use Pass-the Hash
