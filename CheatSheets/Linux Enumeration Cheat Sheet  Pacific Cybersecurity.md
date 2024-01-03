After gaining shell access to a Linux system, you may want to perform some common tasks to better understand the system, its installed software, its users, and their files. This is referred to as _enumeration_.

Note that different commands will work on different Linux distributions, so experimentation (and learning!) is needed.

### Operating System

What distribution and version is used?

```bash
$ cat /etc/issue $ cat /etc/*-release $ cat /etc/lsb-release $ cat /etc/redhat-release
```

What is the Kernel version? Is it 64-bit?

```bash
$ cat /proc/version $ uname -a $ uname -mrs $ rpm -q kernel $ dmesg | grep Linux
```

What can be learned from the environmental variables?

```bash
$ env $ set $ cat /etc/profile $ cat /etc/bashrc $ cat ~/.bash_profile $ cat ~/.bashrc $ cat ~/.bash_logout $ cat ~/.zshrc
```

### Applications and Services

What services are running? And what users are they running as?

```bash
$ ps aux $ ps -elf $ top $ cat /etc/service
```

Which service(s) are running as root? Of these services, which are vulnerable?

```bash
$ ps aux | grep root $ ps -elf | grep root
```

What applications are installed? What version are they? Are they currently running?

```bash
$ dpkg -l $ dpkg -l PACKAGE-NAME $ rpm -qa
```

What jobs are scheduled?

```bash
$ crontab -l $ cat /etc/cron* $ cat /etc/cron.d/* $ cat /etc/cron.daily/* $ cat /etc/cron.hourly/* $ cat /etc/cron.monthly/* $ cat /etc/crontab $ cat /etc/at.allow $ cat /etc/at.deny $ cat /etc/anacrontab
```

### Communications and Networking

What NIC(s) does the system have? Is it connected to another network?

```bash
$ ifconfig $ ip link $ ip addr $ /sbin/ifconfig -a $ cat /etc/network/interfaces $ cat /etc/sysconfig/network
```

What are the network configuration settings? What can you find out about this network? DHCP server? DNS server? Gateway? Firewall rules?

```bash
$ cat /etc/resolv.conf $ cat /etc/sysconfig/network $ cat /etc/networks $ iptables -L $ hostname $ dnsdomainname
```

What other users & hosts are communicating with this system?

```bash
$ lsof -i $ lsof -i :80 $ netstat -antup $ netstat -antpx $ netstat -tulpn $ chkconfig --list $ chkconfig --list | grep 3:on $ last $ w
```

### Confidential Information and Users

Who are you? Who is logged in now? Who has been logged in previously? Who else is there? Who can do what?

```bash
$ id $ who $ w $ last $ cat /etc/passwd # List of users $ cat /etc/sudoers $ sudo -l
```

What sensitive files can be found?

```bash
$ cat /etc/passwd # User accounts $ cat /etc/group # Groups $ cat /etc/shadow # Password hashes
```

Is anything "interesting" in the home directories? Do you have access?

```bash
$ ls -ahlR /root/ $ ls -ahlR /home/
```

Are there any passwords in scripts, databases, configuration files or log files? The specific files to search will depend on the installed programs determined previously...

```bash
$ cat /var/apache2/config.inc $ cat /var/lib/mysql/mysql/user.MYD $ cat /root/anaconda-ks.cfg
```

What has the user being doing? Are there any password in plain text? What have they been editing?

```bash
$ cat ~/.bash_history $ cat ~/.zsh_history $ cat ~/.nano_history $ cat ~/.atftp_history $ cat ~/.mysql_history $ cat ~/.php_history
```

Are there any private keys accessible?

```bash
cat ~/.ssh/* # Check other user directories too!
```

### File Systems

How are file-systems mounted?

```bash
$ mount $ df -h
```

Are there any unmounted file-systems?

```bash
$ cat /etc/fstab
```

Find world writable folders and files:

```bash
$ find / -xdev -type d -perm -0002 -ls 2> /dev/null $ find / -xdev -type f -perm -0002 -ls 2> /dev/null
```

Find SUIDs (files & programs that have the permission of their owner -- usually root. Useful for privilege escalation)

```bash
$ find / -perm -4000 -user root -exec ls -ld {} \; 2> /dev/null
```

### Next Steps

What development tools/languages are installed/supported?

```bash
$ which perl $ which python $ which python3 $ which gcc # Can look for binaries not in search path # find / -name perl* # find / -name python* # find / -name gcc* # find / -name cc
```

How can files be downloaded to this system?

```bash
$ which wget $ which nc $ which netcat # Can look for binaries not in search path # find / -name wget # find / -name nc* # find / -name netcat*
```