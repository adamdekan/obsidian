1) Nmap scan discovers SMB, WinRM services
2) SMB is accessible by anyone
```
enum4linux-ng $RHOST -oA enum4smb
```

3) Using smbget to download whole SMB of /Backups
```
smbget --recursive smb://$RHOST/Backups
```