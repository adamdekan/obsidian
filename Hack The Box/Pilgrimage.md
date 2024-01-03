Getting the **redirect info** `curl 10.10.11.219 -I`

```
HTTP/1.1 301 Moved Permanently
Server: nginx/1.18.0
Date: Thu, 29 Jun 2023 12:13:54 GMT
Content-Type: text/html
Content-Length: 169
Connection: keep-alive
Location: http://pilgrimage.htb/
```

```
# Nmap 7.94 scan initiated Thu Jun 29 14:17:40 2023 as: nmap -sC -sV -v -oA pilgrimage-v -p22,80 10.10.11.219
Nmap scan report for pilgrimage.htb (10.10.11.219)
Host is up (0.071s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey:
|   3072 20:be:60:d2:95:f6:28:c1:b7:e9:e8:17:06:f1:68:f3 (RSA)
|   256 0e:b6:a6:a8:c9:9b:41:73:74:6e:70:18:0d:5f:e0:af (ECDSA)
|_  256 d1:4e:29:3c:70:86:69:b4:d7:2c:c8:0b:48:6e:98:04 (ED25519)
80/tcp open  http    nginx 1.18.0
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-title: Pilgrimage - Shrink Your Images
| http-git:
|   10.10.11.219:80/.git/
|     Git repository found!
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|_    Last commit message: Pilgrimage image shrinking service initial commit. # Please ...
|_http-server-header: nginx/1.18.0
| http-methods:
|_  Supported Methods: GET HEAD POST
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Next step was finding **GitTools** to download and extract git files:
`git clone https://github.com/internetwache/GitTools`

And use it's **Dumper and Extractor**. All the necessary how to can be found in README.md of the GitTools

Upon inspection of index.php and bulletproof.php:
https://book.hacktricks.xyz/pentesting-web/file-upload
Nothing

Unpack of file magick and in the tree I've found version with know vuln:
https://github.com/voidz0r/CVE-2022-44268

```
emily:x:1000:1000:emily,,,:/home/emily:/bin/bash
_laurel:x:998:998::/var/log/laurel:/bin/false
```

From the php files I got db:
sqlite:/var/db/pilgrimage

After hex conversion:
`p@p.pp-emilyabigchonkyboi123` - abigchonkyboi123

`ssh emily@pilgrimage.htb`

76b9911fc17be21a6675180836736ad1

https://1cysec.medium.com/htb-pilgrimage-writeup-e4dba5a9111e