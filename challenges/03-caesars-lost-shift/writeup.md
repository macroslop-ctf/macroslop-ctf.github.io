# Caesar's Lost Shift - Writeup

## Challenge Description
ROT13 is too easy. Try finding the original Caesar cipher shift.

## Solution

The encrypted text uses a Caesar cipher with a shift of 7 (not ROT13).

### Ciphertext
`ULDRM{YLZSPHS_JOHWOLY_IYPHRVULKQ}`

### Decryption Process
Test all 26 possible shifts until readable text appears:

- Shift 1-6: Gibberish
- **Shift 7:** `MCTF{REALPYTHON_CIPHERTEXTBREAKTHROUGH}` ✓

### Tools
You can use:
```python
def caesar_decrypt(ciphertext, shift):
    result = ""
    for char in ciphertext:
        if char.isalpha():
            ascii_offset = 65 if char.isupper() else 97
            result += chr((ord(char) - ascii_offset - shift) % 26 + ascii_offset)
        else:
            result += char
    return result
```

## Flag
`MCTF{REALPYTHON_CIPHERTEXTBREAKTHROUGH}`

## Prevention
Caesar cipher is extremely weak. Use modern encryption standards like AES.