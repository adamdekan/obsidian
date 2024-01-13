Problem:

-   Some commands, like `su` and `ssh` require a proper terminal to run
-   STDERR usually isn’t displayed
-   Can’t properly use text editors like `vim`
-   No tab-complete
-   No up arrow history
-   No job control
-   Etc…

Resources:
-   [Pentest Monkey - Post Exploitation Without a TTY](http://pentestmonkey.net/blog/post-exploitation-without-a-tty)
-   [Phineas Fisher Hacks Catalan Police Union Website](https://www.youtube.com/watch?v=oI_ZhFCS3AQ#t=25m53s)
-   [Phineas Fisher - Hackingteam Writeup](http://pastebin.com/raw/0SNSvyjJ)

Pentest Monkey has a great [cheatsheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet) outlining a few different methods, but my favorite technique is to use Metasploit’s `msfvenom` to generate the one-liner commands for me.

Metasploit has several payloads under “cmd/unix” that can be used to generate one-liner bind or reverse shells:

[![msfvenom payloads](https://blog.ropnop.com/images/2017/07/msfvenom_payloads.png)](https://blog.ropnop.com/images/2017/07/msfvenom_payloads.png)

Any of these payloads can be used with `msfvenom` to spit out the raw command needed (specifying LHOST, LPORT or RPORT). For example, here’s a netcat command not requiring the `-e` flag:

[![Netcat shell](https://blog.ropnop.com/images/2017/07/netcat_shell_cmd.png)](https://blog.ropnop.com/images/2017/07/netcat_shell_cmd.png)

And here’s a Perl oneliner in case `netcat` isn’t installed:

[![Perl shell](https://blog.ropnop.com/images/2017/07/perl_shell_cmd.png)](https://blog.ropnop.com/images/2017/07/perl_shell_cmd.png)

These can all be caught by using netcat and listening on the port specified (4444).

## Method 1: Python pty module

One of my go-to commands for a long time after catching a dumb shell was to use Python to spawn a pty. The [pty module](https://docs.python.org/2/library/pty.html) let’s you spawn a psuedo-terminal that can fool commands like `su` into thinking they are being executed in a proper terminal. To upgrade a dumb shell, simply run the following command:

<table><tbody><tr><td><pre><code><span>1
</span></code></pre></td><td><pre></pre></td></tr></tbody></table>

This will let you run `su` for example (in addition to giving you a nicer prompt)

[![Python PTY](https://blog.ropnop.com/images/2017/07/python_pty.png)](https://blog.ropnop.com/images/2017/07/python_pty.png)

Unfortunately, this doesn’t get around some of the other issues outlined above. SIGINT (Ctrl-C) will still close Netcat, and there’s no tab-completion or history. But it’s a quick and dirty workaround that has helped me numerous times.

## Method 2: Using socat

[socat](http://www.dest-unreach.org/socat/doc/socat.html) is like netcat on steroids and is a very powerfull networking swiss-army knife. Socat can be used to pass full TTY’s over TCP connections.

If `socat` is installed on the victim server, you can launch a reverse shell with it. You _must_ catch the connection with `socat` as well to get the full functions.

The following commands will yield a fully interactive TTY reverse shell:

**On Kali (listen)**:

<table><tbody><tr><td><pre><code><span>1
</span></code></pre></td><td><pre></pre></td></tr></tbody></table>

**On Victim (launch)**:

<table><tbody><tr><td><pre><code><span>1
</span></code></pre></td><td><pre></pre></td></tr></tbody></table>

If socat isn’t installed, you’re not out of luck. There are standalone binaries that can be downloaded from this awesome Github repo:

[https://github.com/andrew-d/static-binaries](https://github.com/andrew-d/static-binaries)

With a command injection vuln, it’s possible to download the correct architecture `socat` binary to a writable directoy, chmod it, then execute a reverse shell in one line:

<table><tbody><tr><td><pre><code><span>1
</span></code></pre></td><td><pre></pre></td></tr></tbody></table>

On Kali, you’ll catch a fully interactive TTY session. It supports tab-completion, SIGINT/SIGSTP support, vim, up arrow history, etc. It’s a full terminal. Pretty sweet.

[![Socat tty](https://blog.ropnop.com/images/2017/07/socat_tty.png)](https://blog.ropnop.com/images/2017/07/socat_tty.png)

## Method 3: Upgrading from netcat with magic

I watched Phineas Fisher use this technique in his hacking video, and it feels like magic. Basically it is possible to use a dumb netcat shell to upgrade to a full TTY by setting some `stty` options within your Kali terminal.

First, follow the same technique as in Method 1 and use Python to spawn a PTY. Once bash is running in the PTY, background the shell with `Ctrl-Z`

[![Background shell](https://blog.ropnop.com/images/2017/07/background_netcat.png)](https://blog.ropnop.com/images/2017/07/background_netcat.png)

While the shell is in the background, now examine the current terminal and STTY info so we can force the connected shell to match it:

[![Term and STTY info](https://blog.ropnop.com/images/2017/07/term_stty_info.png)](https://blog.ropnop.com/images/2017/07/term_stty_info.png)

The information needed is the TERM type (_“xterm-256color”_) and the size of the current TTY (_“rows 38; columns 116”_)

With the shell still backgrounded, now set the current STTY to type raw and tell it to echo the input characters with the following command:

With a raw stty, input/output will look weird and you won’t see the next commands, but as you type they are being processed.

Next foreground the shell with `fg`. It will re-open the reverse shell but formatting will be off. Finally, reinitialize the terminal with `reset`.

[![Foreground and reset](https://blog.ropnop.com/images/2017/07/fg_reset.png)](https://blog.ropnop.com/images/2017/07/fg_reset.png)

_Note: I did not type the `nc` command again (as it might look above). I actually entered `fg`, but it was not echoed. The `nc` command is the job that is now in the foreground. The `reset` command was then entered into the netcat shell_

After the `reset` the shell should look normal again. The last step is to set the shell, terminal type and stty size to match our current Kali window (from the info gathered above)

<table><tbody><tr><td><pre><code><span>1
</span><span>2
</span><span>3
</span></code></pre></td><td><pre></pre></td></tr></tbody></table>

The end result is a fully interactive TTY with all the features we’d expect (tab-complete, history, job control, etc) all over a netcat connection:

[![Netcat full TTY](https://blog.ropnop.com/images/2017/07/netcat_full_tty.png)](https://blog.ropnop.com/images/2017/07/netcat_full_tty.png)

The possibilities are endless now. Tmux over a netcat shell?? Why not? :D

[![Tmux over Netcat](https://blog.ropnop.com/images/2017/07/tmux_over_netcat-1.png)](https://blog.ropnop.com/images/2017/07/tmux_over_netcat-1.png)

## tl;dr cheatsheet

Cheatsheet commands:

**Using Python for a psuedo terminal**

<table><tbody><tr><td><pre><code><span>1
</span></code></pre></td><td><pre></pre></td></tr></tbody></table>

**Using socat**

<table><tbody><tr><td><pre><code><span>1
</span><span>2
</span><span>3
</span><span>4
</span><span>5
</span></code></pre></td><td><pre></pre></td></tr></tbody></table>

**Using stty options**

<table><tbody><tr><td><pre><code><span> 1
</span><span> 2
</span><span> 3
</span><span> 4
</span><span> 5
</span><span> 6
</span><span> 7
</span><span> 8
</span><span> 9
</span><span>10
</span><span>11
</span><span>12
</span><span>13
</span></code></pre></td><td><pre></pre></td></tr></tbody></table>

Any other cool techniques? Let me know in the comments or hit me up on twitter.

Enjoy! -ropnop

___
