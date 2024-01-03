

## Enumeration
`search portscan` for inside of a machine scanning without nmap
`use autoroute` - routing for meterpreter session
`search cve:2015 name:name`
`vulns`

**SMB**
```
smb_version
smb_enumshares
smb_enumusers
smb_login
```

**FTP**
```
ftp_version
ftp_login
ftp/anonymous
```

**Apache**
`search type:auxiliary name:http`

```
- auxiliary/scanner/http/apache_userdir_enum
- auxiliary/scanner/http/brute_dirs
- auxiliary/scanner/http/dir_scanner
- auxiliary/scanner/http/dir_listing
- auxiliary/scanner/http/http_put
- auxiliary/scanner/http/files_dir
- auxiliary/scanner/http/http_login
- auxiliary/scanner/http/http_header
- auxiliary/scanner/http/http_version
- auxiliary/scanner/http/robots_txt
```

```
curl -u username:password http://
curl -u username http://
```

**MySQL**
`search type:auxiliary name:mysql`

```
- auxiliary/admin/mysql/mysql_enum
- auxiliary/admin/mysql/mysql_sql
- auxiliary/scanner/mysql/mysql_file_enum
- auxiliary/scanner/mysql/mysql_hashdump
- auxiliary/scanner/mysql/mysql_login
- auxiliary/scanner/mysql/mysql_schemadump
- auxiliary/scanner/mysql/mysql_version
- auxiliary/scanner/mysql/mysql_writable_dirs
```

