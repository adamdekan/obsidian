# Summary

- default credentials
- deleted strings recovery with `strings` from hard drives
## Port Scan

```r
rustscan:
Open 10.129.27.242:53
Open 10.129.27.242:80
Open 10.129.27.242:1941
Open 10.129.27.242:32400
Open 10.129.27.242:32469
```

## Port 80
```
Pi-hole Version v3.1.4 Web Interface Version v3.1 FTL Version v2.10
```

https://pulsesecurity.co.nz/advisories/pihole-v3.3-vulns

## Port 20
```
22/tcp open  ssh     syn-ack OpenSSH 6.7p1 Debian 5+deb8u3 (protocol 2.0)
```

CVE-2016-6210 - username enumeration
Trying default username and password for pi-hole returns a shell via ssh.

# Privilege escalation
`sudo -l` reveals `NOPASSWD` for all commands
`sudo su` gains root access

Flag can be found in deleted files on /dev/sdb (which is /meda/usbstick) with `strings /dev/sdb`