Allport scan:
```
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
2379/tcp  open  etcd-client
2380/tcp  open  etcd-server
8443/tcp  open  https-alt
10249/tcp open  unknown
10250/tcp open  unknown
10256/tcp open  unknown

```


msfconsole etcd scan:
```
{"major"=>"1", "minor"=>"22", "gitVersion"=>"v1.22.3", "gitCommit"=>"c92036820499fedefec0f847e2054d824aea6cd1", "gitTreeState"=>"clean", "buildDate"=>"2021-10-27T18:35:25Z", "goVersion"=>"go1.16.9", "compiler"=>"gc", "platform"=>"linux/amd64"}
```

