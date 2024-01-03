# Chisel - Reverse Port Forwarding

I want to access port **3000 on localhost** in remote machine:

```shell
# In local machine
chisel server -p 1337 --reverse

# In remote machine
chisel client 10.0.0.1:1337 R:9999:127.0.0.1:3000
```

Now I can access in my `http://localhost:9999` the remote `3000` port. In other words, we can access to `http://remote.machine:3000/` via `localhost:9999`

Server: Jetty(11.0.14)

# Enumeration - Env

Environment variables inside Docker container:

```
USER=metabase
META_PASS=An4lytics_ds20223#
META_USER=metalytics
MB_DB_FILE=//metabase.db/metabase.db
MB_DB_PASS=
```

`ssh metalytics@[ip]`
`An4lytics_ds20223#`
# Privilege Escalation

uname -a:
https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629/tree/main