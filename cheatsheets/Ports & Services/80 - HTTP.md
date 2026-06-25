

Requests & Responses
GET - getting info from web server
POST - for submitting data to a web server
PUT - also for submitting data to web server, usually used for updating information

cURL
Use for making requests and view response
use -o to save output to a file 
```
curl -X "POST" -d "data" http://[IP or domain]
```
- to send data 

Try upload webshell for file uploads 

Auth Bypass
```
ffuf -u 'http://[IP/burp POST]' -w Auth_Bypass.txt -d '[burp credentials response]' -X POST --fc 401 -t 50 -H "Content-Type: [from burp]" -r -fr '[error response e.g. incorrect]'
```
- -d 'username=FUZZ&password=admin'

XXE - users private key 
SQL - dump credentials or even get a shell 

If web port is open to local host, we might be able to send commands using curl (Nickel (windows))
```
curl [http://localhost:[port]/?[cmd](http://localhost:[port]/?%5bcmd)]
```

nmap -sV -A --script 'vuln' 192.168.166.168 -oN script