On the port 50051 is gRPC server that can be connected with:
[https://github.com/fullstorydev/grpcui](https://github.com/fullstorydev/grpcui)

After registration and login I have used token to use getInfo() method.
I've dumped the request to a file and used sqlmap to check for injection:

```
sqlmap -r sqli.req --dump
```

After obtaining ssh and check with linpeas.sh I've found a process running on 8000.
Using msfconsole and auxiliary/scanner/ssh/ssh_login I've logged into the shell and upgraded the session with post/multi/manage/shell_to_meterpreter.
Next step is to create a port forwarding to my machine:

```
# Inside a meterpreter session
portfwd add -l <attacker_port> -p <Remote_port> -r <Remote_host>
```

I've found an app payload that has vulnerability:
https://github.com/bAuh0lz/CVE-2023-0297_Pre-auth_RCE_in_pyLoad

Finally flag -p for SUID bash:

```
/tmp/bash -p
```

Rooted.