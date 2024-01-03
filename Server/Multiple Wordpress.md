To set up multiple WP installations Its needed to set `/opt/cpanel/ea-php81/root/etc/php-fpm.d/` (also possible to find with `ps aux | grep fpm` ) folder and inside of the config files set the listen socket and its location for newly installed WP application. After that `systemctl restart ea-php81-php-fpm` so that sock is created. Afterwards set the `/etc/nginx/conf.d/*config` file so that it points to the newly created sock. The rest can be just a duplicate. In the `/etc/nginx/snippets/` is a master config for php.

```
vim /etc/nginx/snippets/fastcgi-php.conf
```

Useful `nginx` commands:
```
nginx -t && systemctl reload nginx
```

Error logs are important for possible debugging.
# DNS from Namecheap

Simply on the dashboard page points to the custom Nameserver (3rd option) that is:
```
ns1.lensuno.com
ns2.lensuno.com
```


# Yum notes
```
sudo yum list installed
```


# Uwsgi
```
/etc/uwsgi/vassals/lensuno.ini
tail /var/log/uwsgi.log
```