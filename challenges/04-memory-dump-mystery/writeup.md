# Memory Dump Mystery - Writeup

## Challenge Description
Extract the flag from this suspicious memory dump file.

## Solution

### Step 1: Analyze the Memory Dump
Use `strings` command to extract readable text from the binary file:
```bash
strings memory.bin | grep -i flag
```

### Step 2: Look for Patterns
The flag often appears near variable declarations or in stack memory. Search for the pattern `MCTF{`:
```bash
strings memory.bin | grep "MCTF{"
```

### Step 3: Extract the Flag
Found in memory offset: `0x4a7f2c`
```
MCTF{HEAP_OVERFLOW_MEMORY_CORRUPTION}
```

## Tools Used
- `strings` - Extract strings from binary
- `hexdump` or `xxd` - Examine raw hex
- `objdump` - Disassemble binaries

## Key Concepts
- Memory layout (heap, stack, BSS)
- String searching in binary files
- Forensic analysis techniques

## Flag
`MCTF{HEAP_OVERFLOW_MEMORY_CORRUPTION}`