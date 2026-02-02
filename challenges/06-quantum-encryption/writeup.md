# Quantum Encryption - Writeup

## Challenge Description
Can you break this 'unbreakable' encryption? (It's not really quantum)

## Solution

### Step 1: Identify the Cipher
Analysis reveals this is RSA encryption with weak parameters:
- **n:** 143 (= 11 × 13)
- **e:** 7
- **d:** Need to find via Euler's totient

### Step 2: Factor the Modulus
```python
n = 143
# Trial division
for i in range(2, int(n**0.5) + 1):
    if n % i == 0:
        p, q = i, n // i
        break
# p = 11, q = 13
```

### Step 3: Calculate Private Key
```python
phi = (p - 1) * (q - 1)  # φ(143) = 10 × 12 = 120
# Find d such that (e * d) % phi = 1
# 7 * d ≡ 1 (mod 120)
d = 103
```

### Step 4: Decrypt
```python
ciphertext = 17
plaintext = pow(ciphertext, d, n)
```

## Complete Exploit
```python
from sympy import factorint

n = 143
e = 7
c = 17

# Factor
factors = factorint(n)  # {11: 1, 13: 1}
p, q = 11, 13

# Calculate private key
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)

# Decrypt
m = pow(c, d, n)
print(f"MCTF{{{m}}}")  # MCTF{65}
```

## Flag
`MCTF{SMALL_RSA_MODULUS_WEAKNESS}`

## Prevention
- Use large prime numbers (2048+ bits)
- Use proven cryptographic libraries
- Never implement crypto from scratch