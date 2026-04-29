---
name: security-review
description: "Deep security review of code. Use when reviewing for security, checking for vulnerabilities, OWASP issues, secrets, auth problems, or injection risks."
---

# Security Review

Perform a security-focused review of the code. Check every item below that applies.

## Checklist

### Injection
- SQL/NoSQL injection via string concatenation or unsanitized input
- Command injection through shell execution with user input
- Path traversal via unvalidated file paths
- Template injection (SSTI) in server-rendered content
- LDAP, XPath, or expression language injection

### Authentication & Authorization
- Missing or bypassable auth checks on endpoints/functions
- Broken access control: horizontal (user A accessing user B's data) or vertical (privilege escalation)
- Hardcoded credentials, API keys, or tokens
- Weak or missing password hashing (plaintext, MD5, SHA1 without salt)
- Missing rate limiting on auth endpoints

### Data Exposure
- Sensitive data in logs, error messages, or API responses
- PII leaked in URLs, query strings, or headers
- Missing encryption for data at rest or in transit
- Secrets committed to source control
- Overly permissive CORS configuration

### Input Validation
- Missing or insufficient validation at trust boundaries
- Reliance on client-side validation only
- Deserialization of untrusted data
- Unvalidated redirects or forwards
- Missing Content-Type / Accept header validation

### Cryptography
- Use of deprecated algorithms (MD5, SHA1, DES, RC4)
- Hardcoded IVs, keys, or salts
- Insufficient key length
- Missing HMAC or signature verification
- Predictable random number generation for security-sensitive operations

### Configuration
- Debug mode enabled in production code
- Verbose error messages exposing internals
- Missing security headers (CSP, HSTS, X-Frame-Options)
- Overly permissive file/directory permissions
- Default credentials or configurations

## Output

For each finding:
- State the vulnerability type and OWASP category if applicable
- Show the vulnerable code location
- Explain the attack vector
- Provide the fix with corrected code
