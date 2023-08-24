- `dirsearch` is useful for content discovery
- `dirbuster` is with GUI
- In Parrot `sudo` is needed for ifconfig/iwconfig
- grep's **-A 1** option will give you one line after; **-B 1** will give you one line before; and **-C 1** combines both to give you one line both before and after, -1 does the same.
- `tcpdump -i tun0 icmp` - listening to external icmp ping
- default nginx server config location: `/etc/nginx/sites-available/default`
- To identify SSTI vulnerabilities, use a Polyglot payload composed of special characters commonly used in template expressions to fuzz the template
   `${{<%[%'"}}%\.`

## Display all active ports

You can use the `netstat` or `ss` command to display all active ports on a Linux system. Both commands provide information about network connections, listening ports, and various network statistics. Here's how you can use these commands to display active ports:

1. Using `netstat` (Note: `netstat` might not be available on all systems as it's being replaced by `ss`):

```bash
netstat -tuln
```

- `-t`: Display TCP ports
- `-u`: Display UDP ports
- `-l`: Show only listening ports
- `-n`: Show numerical addresses instead of resolving hostnames

2. Using `ss` (a more modern replacement for `netstat`):

```bash
ss -tuln
```

- `-t`: Display TCP ports
- `-u`: Display UDP ports
- `-l`: Show only listening ports
- `-n`: Show numerical addresses instead of resolving hostnames

The output of either command will list all active ports that your system is listening on, along with the protocol (TCP or UDP), the local address (IP and port), and the state of the connection.

## Using Wget to download website:

```
wget -m -p -E -k -np www.example.com
```

```
-m, --mirror            Turns on recursion and time-stamping, sets infinite
                          recursion depth, and keeps FTP directory listings.
-p, --page-requisites   Get all images, etc. needed to display HTML page.
-E, --adjust-extension  Save HTML/CSS files with .html/.css extensions.
-k, --convert-links     Make links in downloaded HTML point to local files.
-np, --no-parent        Dont ascend to the parent directory when retrieving
                        recursively. This guarantees that only the files below
                        a certain hierarchy will be downloaded. Requires a slash
                        at the end of the directory, e.g. example.com/foo/.

```

