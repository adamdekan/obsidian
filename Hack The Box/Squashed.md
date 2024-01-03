Users from show mount scan:
```
47829/tcp open  mountd  syn-ack 1-3 (RPC #100005)
| nfs-showmount: 
|   /home/ross *
|_  /var/www/html *
```

I can mount `/home/ross` with `nfsshell` or 
```
sudo mount -t nfs 10.129.62.125:/home/ross /home/volt/htb/squashed/mnt -o nolock
```
And found `Passwords.kdbx` that needs to be cracked.