- `dirsearch` is useful for content discovery
- `dirbuster` is with GUI
- In Parrot `sudo` is needed for ifconfig/iwconfig
- grep's **-A 1** option will give you one line after; **-B 1** will give you one line before; and **-C 1** combines both to give you one line both before and after, -1 does the same.
- To identify SSTI vulnerabilities, use a Polyglot payload composed of special characters commonly used in template expressions to fuzz the template
   `${{<%[%'"}}%\.`