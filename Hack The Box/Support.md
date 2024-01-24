# 445 - SMB
```
smbmap -u guest -p '' -H $RHOST -r 'support-tools'

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
 SMBMap - Samba Share Enumerator v1.10.2 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB connections(s) and 1 authentidated session(s)

[+] IP: 10.129.6.201:445        Name: support.htb               Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                NO ACCESS       Logon server share
        support-tools                                           READ ONLY       support staff tools
        ./support-tools
        dr--r--r--                0 Wed Jul 20 19:01:06 2022    .
        dr--r--r--                0 Sat May 28 13:18:25 2022    ..
        fr--r--r--          2880728 Sat May 28 13:19:19 2022    7-ZipPortable_21.07.paf.exe
        fr--r--r--          5439245 Sat May 28 13:19:55 2022    npp.8.4.1.portable.x64.zip
        fr--r--r--          1273576 Sat May 28 13:20:06 2022    putty.exe
        fr--r--r--         48102161 Sat May 28 13:19:31 2022    SysinternalsSuite.zip
        fr--r--r--           277499 Wed Jul 20 19:01:07 2022    UserInfo.exe.zip
        fr--r--r--            79171 Sat May 28 13:20:17 2022    windirstat1_1_2_setup.exe
        fr--r--r--         44398000 Sat May 28 13:19:43 2022    WiresharkPortable64_3.6.5.paf.exe
```